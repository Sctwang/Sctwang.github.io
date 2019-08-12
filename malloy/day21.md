## React

- ~~~html
  <!DOCTYPE html>
  <html>
  <head>
  <meta charset="UTF-8" />
  <title>Hello React!</title>
  <script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
  <script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
  <script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
  </head>
  <body>
   
  <div id="example"></div>
  <script type="text/babel">
  ReactDOM.render(
      <h1>Hello, world!</h1>,
      document.getElementById('example')
  );
  </script>
   
  </body>
  </html>
  ~~~

- 实例中我们引入了三个库： react.min.js 、react-dom.min.js 和 babel.min.js：

  - **react.min.js** - React 的核心库
  - **react-dom.min.js** - 提供与 DOM 相关的功能
  - **babel.min.js** - Babel 可以将 ES6 代码转为 ES5 代码，这样我们就能在目前不支持 ES6 浏览器上执行 React 代码。Babel 内嵌了对 JSX 的支持。通过将 Babel 和 babel-sublime 包（package）一同使用可以让源码的语法渲染上升到一个全新的水平。

- 使用 create-react-app 快速构建 React 开发环境

  ~~~java
  $ cnpm install -g create-react-app
  $ create-react-app my-app
  $ cd my-app/
  $ npm start
  ~~~

  

