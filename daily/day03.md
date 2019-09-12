# MySQL

- ExamTableRelations.sql

  ~~~mysql
  HAND_STUDENT -- Student Info
  HAND_TEACHER -- Self Join, include manager
  HAND_COURSE -- Course, Teacher > Primary Key
  HAND_STUDENT_CORE -- Score; Student, Course> Primary Key
  ~~~

- create_table_mysql.sql

  ~~~mysql
  -- Create table 学生信息
  create table HAND_STUDENT
  (
    STUDENT_NO     VARCHAR(10) not null COMMENT '学号',
    STUDENT_NAME   VARCHAR(20) COMMENT '姓名',
    STUDENT_AGE    BIGINT(2) COMMENT '年龄',
    STUDENT_GENDER VARCHAR(5) COMMENT '性别'
  )COMMENT='学生信息表'; 
  alter table HAND_STUDENT add primary key (STUDENT_NO);
  
  -- Create table 教师信息表
  create table HAND_TEACHER
  (
    TEACHER_NO   VARCHAR(10) not null COMMENT '教师编号',
    TEACHER_NAME VARCHAR(20) COMMENT '教师名称',
    MANAGER_NO   VARCHAR(10) COMMENT '上级编号'
  )COMMENT='教师信息表';
  alter table HAND_TEACHER add primary key (TEACHER_NO);
  
  -- Create table 课程信息表
  create table HAND_COURSE
  (
    COURSE_NO   VARCHAR(10) not null COMMENT '课程号',
    COURSE_NAME VARCHAR(20) COMMENT '课程名称',
    TEACHER_NO  VARCHAR(20) not null COMMENT '教师编号'
  )COMMENT='课程信息表';
  alter table HAND_COURSE add constraint PK_COURSE primary key (COURSE_NO, TEACHER_NO);
  
  -- Create table 成绩信息表
  create table HAND_STUDENT_CORE
  (
    STUDENT_NO VARCHAR(10) not null COMMENT '学号',
    COURSE_NO  VARCHAR(10) not null COMMENT '课程号',
    CORE       DECIMAL(4,2) COMMENT '分数'
  )COMMENT='学生成绩表';
  alter table HAND_STUDENT_CORE add constraint PK_SC primary key (STUDENT_NO, COURSE_NO);
  
  /*******初始化学生表的数据******/
  insert into HAND_STUDENT values ('s001','张三',23,'男');
  insert into HAND_STUDENT values ('s002','李四',23,'男');
  insert into HAND_STUDENT values ('s003','吴鹏',25,'男');
  insert into HAND_STUDENT values ('s004','琴沁',20,'女');
  insert into HAND_STUDENT values ('s005','王丽',20,'女');
  insert into HAND_STUDENT values ('s006','李波',21,'男');
  insert into HAND_STUDENT values ('s007','刘玉',21,'男');
  insert into HAND_STUDENT values ('s008','萧蓉',21,'女');
  insert into HAND_STUDENT values ('s009','陈萧晓',23,'女');
  insert into HAND_STUDENT values ('s010','陈美',22,'女');
  commit;
  /******************初始化教师表***********************/
  insert into HAND_TEACHER values ('t001', '刘阳','');
  insert into HAND_TEACHER values ('t002', '谌燕','t001');
  insert into HAND_TEACHER values ('t003', '胡明星','t002');
  commit;
  /***************初始化课程表****************************/
  insert into HAND_COURSE values ('c001','J2SE','t002');
  insert into HAND_COURSE values ('c002','Java Web','t002');
  insert into HAND_COURSE values ('c003','SSH','t001');
  insert into HAND_COURSE values ('c004','Oracle','t001');
  insert into HAND_COURSE values ('c005','SQL SERVER 2005','t003');
  insert into HAND_COURSE values ('c006','C#','t003');
  insert into HAND_COURSE values ('c007','JavaScript','t002');
  insert into HAND_COURSE values ('c008','DIV+CSS','t001');
  insert into HAND_COURSE values ('c009','PHP','t003');
  insert into HAND_COURSE values ('c010','EJB3.0','t002');
  commit;
  /***************初始化成绩表***********************/
  insert into HAND_STUDENT_CORE values ('s001','c001',58.9);
  insert into HAND_STUDENT_CORE values ('s002','c001',80.9);
  insert into HAND_STUDENT_CORE values ('s003','c001',81.9);
  insert into HAND_STUDENT_CORE values ('s004','c001',60.9);
  insert into HAND_STUDENT_CORE values ('s001','c002',82.9);
  insert into HAND_STUDENT_CORE values ('s002','c002',72.9);
  insert into HAND_STUDENT_CORE values ('s003','c002',81.9);
  insert into HAND_STUDENT_CORE values ('s001','c003','59');
  commit;
  ~~~

- 问题

  ~~~sql
  -- 1. 查询所有学生的成绩，没有成绩的也需要显示学生信息，显示（学号、姓名、课程名称、成绩）[6分]
  
  -- 2. 查询没学过“谌燕”老师课的同学，显示（学号、姓名）[10分]
  
  -- 3. 查询“c001”课程比“c002”课程成绩高的所有学生，显示（学号、姓名）[12分]
  
  -- 4. 按各科平均成绩和及格率的百分数，按及格率高到低的顺序排序，显示（课程号、平均分(保留两位小数)、及格率）[12分]
  
  -- 5. 1995年之后出生的学生名单找出年龄最大和最小的同学，显示（学号、姓名、年龄）[12分]
  
  -- 6. 统计列出矩阵类型各分数段人数，横轴为分数段[100-85]、[85-70]、[70-60]、[<60]，纵轴为课程号、课程名称（提示使用case when句式）[12分]
  
  -- 7. 查询两门以上不及格课程的同学及平均成绩，显示（学号、姓名、平均成绩（保留两位小数））[12分]
  
  -- 8. 查询课程名称为“J2SE”的学生成绩信息，90以上为“优秀”、80-90为“良好”、60-80为“及格”、60分以下为“不及格”，显示（学号、姓名、课程名称、成绩、等级）[12分]
  
  -- 9. 查询分数高于课程“J2SE”中所有学生成绩的学生课程信息，显示（学号，姓名，课程名称、分数）[12分]
  ~~~

- 答案

  ~~~
  
  1. 查询所有学生的成绩，没有成绩的也需要显示学生信息，显示（学号、姓名、课程名称、成绩）[6分]
  SELECT hs.student_no,hs.STUDENT_NAME,hc.COURSE_NAME,hsc.core
    FROM HAND_STUDENT hs 
   LEFT JOIN HAND_STUDENT_CORE hsc ON hs.STUDENT_NO =hsc.student_no
   LEFT JOIN HAND_COURSE hc on (hsc.COURSE_NO=hc.COURSE_NO)
   
   
  2. 查询没学过“谌燕”老师课的同学，显示（学号、姓名）[10分]
  SELECT hs.student_no, hs.student_name
    FROM HAND_STUDENT hs
   WHERE NOT EXISTS
   (SELECT 1
            FROM HAND_COURSE hc, HAND_TEACHER ht, HAND_STUDENT_CORE hsc
           WHERE hc.teacher_no = ht.teacher_no
             AND hc.course_no = hsc.course_no
             AND ht.teacher_name = '谌燕'
             AND hsc.student_no = hs.student_no)
  
  
  3. 查询“c001”课程比“c002”课程成绩高的所有学生，显示（学号、姓名）[12分]
  SELECT hsc1.student_no, hs.student_name
    FROM (SELECT * FROM HAND_STUDENT_CORE sc1 WHERE sc1.course_no = 'c001') hsc1,
         (SELECT * FROM HAND_STUDENT_CORE sc2 WHERE sc2.course_no = 'c002') hsc2,
         HAND_STUDENT hs
   WHERE hsc1.student_no = hsc2.student_no
     AND hsc1.core > hsc2.core
     AND hsc1.student_no = hs.student_no;
  
  SELECT hsc.student_no, hs.student_name
    FROM HAND_STUDENT_CORE hsc, HAND_STUDENT hs
   WHERE hsc.student_no = hs.student_no
     AND hsc.course_no = 'c001'
     AND EXISTS (SELECT *
            FROM HAND_STUDENT_CORE hs
           WHERE hs.course_no = 'c002'
             AND hs.core < hsc.core
             AND hs.student_no = hsc.student_no)
  
  
  4. 按各科平均成绩和及格率的百分数，按及格率高到低的顺序排序，显示（课程号、平均分(保留两位小数)、及格率）[12分]
  SELECT tmp.course_no,ROUND(tmp.avg_core,2),concat(tmp.pass_rate,'%') pass_rate from
  (SELECT hsc.course_no,
         AVG(hsc.core) avg_core,
         (SUM(CASE
                WHEN hsc.core >= 60 THEN
                 1
                ELSE
                 0
              END) / COUNT(*) * 100) AS pass_rate
    FROM HAND_STUDENT_CORE hsc
   GROUP BY hsc.course_no
   ORDER BY pass_rate desc
  ) tmp
  
  
  
  5. 1995年之后出生的学生名单找出年龄最大和最小的同学，显示（学号、姓名、年龄）[12分]
  SELECT hs.student_no, hs.student_name, hs.student_age
    FROM HAND_STUDENT hs,
         (SELECT MAX(hs.student_age) max_age, MIN(hs.student_age) min_age
            FROM HAND_STUDENT hs
           WHERE year(CURRENT_DATE()) - hs.student_age > 1995) hh
   WHERE (hs.student_age = hh.max_age
      OR hs.student_age = hh.min_age)
  
  
  6. 统计列出矩阵类型各分数段人数，横轴为分数段[100-85]、[85-70]、[70-60]、[<60]，纵轴为课程号、课程名称（提示使用case when句式）[12分]
  
  SELECT hsc.course_no,
         hc.course_name,
         SUM(CASE
               WHEN hsc.core BETWEEN 85 AND 100 THEN
                1
               ELSE
                0
             END) AS "[100-85]",
         SUM(CASE
               WHEN hsc.core BETWEEN 70 AND 85 THEN
                1
               ELSE
                0
             END) AS "[85-70]",
         SUM(CASE
               WHEN hsc.core BETWEEN 60 AND 70 THEN
                1
               ELSE
                0
             END) AS "[70-60]",
         SUM(CASE
               WHEN hsc.core < 60 then
                1
               ELSE
                0
             END) AS "[<60]"
    FROM HAND_STUDENT_CORE hsc, HAND_COURSE hc
   WHERE hsc.course_no = hc.course_no
   GROUP BY hsc.course_no, hc.course_name
  
  
  
  7. 查询两门以上不及格课程的同学及平均成绩，显示（学号、姓名、平均成绩（保留两位小数））[12分]
  SELECT hsc.student_no, hs.student_name, ROUND(AVG(hsc.core), 2) avg_core
    FROM HAND_STUDENT_CORE hsc, HAND_STUDENT hs
   WHERE EXISTS (SELECT sc.student_no
            FROM HAND_STUDENT_CORE sc
           WHERE sc.core < 60
             AND sc.student_no = hsc.student_no
           GROUP BY sc.student_no
          HAVING COUNT(sc.student_no) > 1)
     AND hsc.student_no = hs.student_no
   GROUP BY hsc.student_no, hs.student_name;
  
  
  
  8. 查询课程名称为“J2SE”的学生成绩信息，90以上为“优秀”、80-90为“良好”、60-80为“及格”、60分以下为“不及格”，显示（学号、姓名、课程名称、成绩、等级）[12分]
  SELECT hsc.student_no,
         hs.student_name,
         hc.course_name,
         hsc.core,
         CASE
           WHEN hsc.core >= 90 THEN
            '优秀'
           WHEN hsc.core < 90 AND hsc.core >= 80 THEN
            '良好'
           WHEN hsc.core < 80 AND hsc.core >= 60 THEN
            '及格'
           WHEN hsc.core < 60 THEN
            '不及格'
         END core_level
    FROM HAND_STUDENT_CORE hsc, HAND_COURSE hc, HAND_STUDENT hs
   WHERE hsc.course_no = hc.course_no
     AND hsc.student_no = hs.student_no
     AND hc.course_name = 'J2SE';
  
  
  9. 查询分数高于课程“J2SE”中所有学生成绩的学生课程信息，显示（学号，姓名，课程名称、分数）[12分]
  SELECT hsc.student_no, hs.student_name, hc.course_name, hsc.core
    FROM HAND_STUDENT_CORE hsc, HAND_COURSE hc, HAND_STUDENT hs
   WHERE hsc.course_no = hc.course_no
     AND hsc.student_no = hs.student_no
     AND hsc.core > ALL (SELECT hsc.core
            FROM HAND_STUDENT_CORE hsc, HAND_COURSE hc
           WHERE hsc.course_no = hc.course_no
             AND hc.course_name = 'J2SE')
     AND hc.course_name != 'J2SE';
  ~~~
  
  