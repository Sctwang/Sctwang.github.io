转载自 [Jakub Leško](https://dev.to/keysh/spring-security-with-jwt-3j76)





# Spring Security with JWT



*This post were originally posted on my blog.*

Spring Security’s default behavior is easy to use for a standard web application. It uses cookie-based authentication and sessions. Also, it automatically handles CSRF tokens for you (to prevent man in the middle attacks). In most cases you just need to set authorization rights for specific routes, a method to retrieve the user from the database and that’s it.

On the other hand, you probably don’t need full session if you’re building just a REST API which will be consumed with external services or your SPA/mobile application. Here comes the JWT (JSON Web Token) – a small digitally signed token. All needed information can be stored in the token, so your server can be session-less.

JWT needs to be attached to every HTTP request so the server can authorize your users. There are some options on how to send the token. For example, as an URL parameter or in HTTP Authorization header using the Bearer schema:

```
Authorization: Bearer <token string>
```

JSON Web Token contains three main parts:

1. Header – typically includes the type of the token and hashing algorithm.
2. Payload – typically includes data about a user and for whom is token issued.
3. Signature – it’s used to verify if a message wasn’t changed along the way.

## Example token

A JWT token from authorization header will probably look like this:

```
Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpc3MiOiJzZWN1cmUtYXBpIiwiYXVkIjoic2VjdXJlLWFwcCIsInN1YiI6InVzZXIiLCJleHAiOjE1NDgyNDI1ODksInJvbCI6WyJST0xFX1VTRVIiXX0.GzUPUWStRofrWI9Ctfv2h-XofGZwcOog9swtuqg1vSkA8kDWLcY3InVgmct7rq4ZU3lxI6CGupNgSazypHoFOA
```

As you can see there are three parts separated with comma – header, claims, and signature. Header and payload are Base64 encoded JSON objects.

#### Header:

```
{
  "typ": "JWT",
  "alg": "HS512"
}
```

#### Claims/Payload:

```
{
  "iss": "secure-api",
  "aud": "secure-app",
  "sub": "user",
  "exp": 1548242589,
  "rol": [
    "ROLE_USER"
  ]
}
```

## Example application

In the following example, we will create a simple API with 2 routes – one publicly available and one only for authorized users.

We will use page [start.spring.io](https://start.spring.io/) to create our application skeleton and select Security and Web dependencies. Rest of the options are up to your preferences.

[![Spring Initializr screenshot](https://res.cloudinary.com/practicaldev/image/fetch/s--8c8VL0yJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://jakublesko.com/wp-content/uploads/2019/01/jwt_create_app.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--8c8VL0yJ--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://jakublesko.com/wp-content/uploads/2019/01/jwt_create_app.png)

JWT support for Java is provided by the library [JJWT](https://github.com/jwtk/jjwt) so we also need to add following dependencies to the pom.xml file:

```
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.10.5</version>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.10.5</version>
    <scope>runtime</scope>
</dependency>
<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.10.5</version>
    <scope>runtime</scope>
</dependency>
```

## Controllers

Controllers in our example applications will be simple as much as possible. They will just return a message or HTTP 403 error code in case the user is not authorized.

```
@RestController
@RequestMapping("/api/public")
public class PublicController {

    @GetMapping
    public String getMessage() {
        return "Hello from public API controller";
    }
}
@RestController
@RequestMapping("/api/private")
public class PrivateController {

    @GetMapping
    public String getMessage() {
        return "Hello from private API controller";
    }
}
```

## Filters

First, we will define some reusable constants and defaults for generation and validation of JWTs.

*Note: You should not hardcode JWT signing key into your application code (we will ignore that for now in the example). You should use an environment variable or .properties file. Also, keys need to have an appropriate length. For example, HS512 algorithm needs a key with size at least 512 bytes.*

```
public final class SecurityConstants {

    public static final String AUTH_LOGIN_URL = "/api/authenticate";

    // Signing key for HS512 algorithm
    // You can use the page http://www.allkeysgenerator.com/ to generate all kinds of keys
    public static final String JWT_SECRET = "n2r5u8x/A%D*G-KaPdSgVkYp3s6v9y$B&E(H+MbQeThWmZq4t7w!z%C*F-J@NcRf";

    // JWT token defaults
    public static final String TOKEN_HEADER = "Authorization";
    public static final String TOKEN_PREFIX = "Bearer ";
    public static final String TOKEN_TYPE = "JWT";
    public static final String TOKEN_ISSUER = "secure-api";
    public static final String TOKEN_AUDIENCE = "secure-app";

    private SecurityConstants() {
        throw new IllegalStateException("Cannot create instance of static util class");
    }
}
```

The first filter will be used directly for user authentication. It’ll check for username and password parameters from URL and calls Spring’s authentication manager to verify them.

If username and password are correct, then the filter will create a JWT token and returns it in HTTP Authorization header.

```
public class JwtAuthenticationFilter extends UsernamePasswordAuthenticationFilter {

    private final AuthenticationManager authenticationManager;

    public JwtAuthenticationFilter(AuthenticationManager authenticationManager) {
        this.authenticationManager = authenticationManager;

        setFilterProcessesUrl(SecurityConstants.AUTH_LOGIN_URL);
    }

    @Override
    public Authentication attemptAuthentication(HttpServletRequest request, HttpServletResponse response) {
        var username = request.getParameter("username");
        var password = request.getParameter("password");
        var authenticationToken = new UsernamePasswordAuthenticationToken(username, password);

        return authenticationManager.authenticate(authenticationToken);
    }

    @Override
    protected void successfulAuthentication(HttpServletRequest request, HttpServletResponse response,
                                            FilterChain filterChain, Authentication authentication) {
        var user = ((User) authentication.getPrincipal());

        var roles = user.getAuthorities()
            .stream()
            .map(GrantedAuthority::getAuthority)
            .collect(Collectors.toList());

        var signingKey = SecurityConstants.JWT_SECRET.getBytes();

        var token = Jwts.builder()
            .signWith(Keys.hmacShaKeyFor(signingKey), SignatureAlgorithm.HS512)
            .setHeaderParam("typ", SecurityConstants.TOKEN_TYPE)
            .setIssuer(SecurityConstants.TOKEN_ISSUER)
            .setAudience(SecurityConstants.TOKEN_AUDIENCE)
            .setSubject(user.getUsername())
            .setExpiration(new Date(System.currentTimeMillis() + 864000000))
            .claim("rol", roles)
            .compact();

        response.addHeader(SecurityConstants.TOKEN_HEADER, SecurityConstants.TOKEN_PREFIX + token);
    }
}
```

The second filter handles all HTTP requests and checks if there is an Authorization header with the correct token. For example, if the token is not expired or if the signature key is correct.

If the token is valid then the filter will add authentication data into Spring’s security context.

```java
public class JwtAuthorizationFilter extends BasicAuthenticationFilter {

    private static final Logger log = LoggerFactory.getLogger(JwtAuthorizationFilter.class);

    public JwtAuthorizationFilter(AuthenticationManager authenticationManager) {
        super(authenticationManager);
    }

    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response,
                                    FilterChain filterChain) throws IOException, ServletException {
        var authentication = getAuthentication(request);
        if (authentication == null) {
            filterChain.doFilter(request, response);
            return;
        }

        SecurityContextHolder.getContext().setAuthentication(authentication);
        filterChain.doFilter(request, response);
    }

    private UsernamePasswordAuthenticationToken getAuthentication(HttpServletRequest request) {
        var token = request.getHeader(SecurityConstants.TOKEN_HEADER);
        if (StringUtils.isNotEmpty(token) && token.startsWith(SecurityConstants.TOKEN_PREFIX)) {
            try {
                var signingKey = SecurityConstants.JWT_SECRET.getBytes();

                var parsedToken = Jwts.parser()
                    .setSigningKey(signingKey)
                    .parseClaimsJws(token.replace("Bearer ", ""));

                var username = parsedToken
                    .getBody()
                    .getSubject();

                var authorities = ((List<?>) parsedToken.getBody()
                    .get("rol")).stream()
                    .map(authority -> new SimpleGrantedAuthority((String) authority))
                    .collect(Collectors.toList());

                if (StringUtils.isNotEmpty(username)) {
                    return new UsernamePasswordAuthenticationToken(username, null, authorities);
                }
            } catch (ExpiredJwtException exception) {
                log.warn("Request to parse expired JWT : {} failed : {}", token, exception.getMessage());
            } catch (UnsupportedJwtException exception) {
                log.warn("Request to parse unsupported JWT : {} failed : {}", token, exception.getMessage());
            } catch (MalformedJwtException exception) {
                log.warn("Request to parse invalid JWT : {} failed : {}", token, exception.getMessage());
            } catch (SignatureException exception) {
                log.warn("Request to parse JWT with invalid signature : {} failed : {}", token, exception.getMessage());
            } catch (IllegalArgumentException exception) {
                log.warn("Request to parse empty or null JWT : {} failed : {}", token, exception.getMessage());
            }
        }

        return null;
    }
}
```

## Security configuration

The last part we need to configure is Spring Security itself. The configuration is simple, we need to set just a few details:

- Password encoder – in our case bcrypt
- [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) configuration
- Authentication manager – in our case simple in-memory authentication but in real life, you’ll need something like [UserDetailsService](https://www.baeldung.com/spring-security-authentication-with-a-database)
- Set which endpoints are secure and which are publicly available
- Add our 2 filters into the security context
- Disable session management – we don’t need sessions so this will prevent the creation of session cookies

```java
@EnableWebSecurity
@EnableGlobalMethodSecurity(securedEnabled = true)
public class SecurityConfiguration extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.cors().and()
            .csrf().disable()
            .authorizeRequests()
            .antMatchers("/api/public").permitAll()
            .anyRequest().authenticated()
            .and()
            .addFilter(new JwtAuthenticationFilter(authenticationManager()))
            .addFilter(new JwtAuthorizationFilter(authenticationManager()))
            .sessionManagement()
            .sessionCreationPolicy(SessionCreationPolicy.STATELESS);
    }

    @Override
    public void configure(AuthenticationManagerBuilder auth) throws Exception {
        auth.inMemoryAuthentication()
            .withUser("user")
            .password(passwordEncoder().encode("password"))
            .authorities("ROLE_USER");
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        final UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", new CorsConfiguration().applyPermitDefaultValues());

        return source;
    }
}
```

## Test

#### Request to public API

```
GET http://localhost:8080/api/public

---

Hello from public API controller
```

#### Authenticate user

```
POST http://localhost:8080/api/authenticate?username=user&password=password

---

<Response body is empty>
```

#### Request to private API with token

```
GET http://localhost:8080/api/private
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzUxMiJ9.eyJpc3MiOiJzZWN1cmUtYXBpIiwiYXVkIjoic2VjdXJlLWFwcCIsInN1YiI6InVzZXIiLCJleHAiOjE1NDgyNDI1ODksInJvbCI6WyJST0xFX1VTRVIiXX0.GzUPUWStRofrWI9Ctfv2h-XofGZwcOog9swtuqg1vSkA8kDWLcY3InVgmct7rq4ZU3lxI6CGupNgSazypHoFOA

--- 

Hello from private API controller
```

#### Request to private API without token

You’ll get HTTP 403 message when you call secured endpoint without a valid JWT.

```json
GET http://localhost:8080/api/private

---

{
  "timestamp": "2019-01-13T12:27:25.020+0000",
  "status": 403,
  "error": "Forbidden",
  "message": "Access Denied",
  "path": "/api/private"
}
```

## Conclusion

The goal of this article is not to show the one correct way how to use JWTs in Spring Security. It’s an example of how you can do it in your real-life application. Also, I did not want to go too deep into the topic so few things like token refreshing, invalidation, etc. are missing but I’ll cover these topics probably in the future.

**tl;dr** You can find the full source code of this example API in my [GitHub repository](https://github.com/keysh/jwt-security).