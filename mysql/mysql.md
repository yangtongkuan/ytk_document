## mysql笔记

### mysql查询练习

```sql
-- 1.查询 student 表的所有行 

SELECT * FROM student;

-- 2.查询 student 表中的 name、sex 和 class 字段的所有行

SELECT NAME,sex,class FROM student;

-- 3.查询 teacher 表中不重复的 department 列
-- DISTINCT  去重

SELECT DISTINCT department FROM teacher;

-- 4.查询 score 表中成绩在60-80之间的所有行（区间查询和运算符查询）

SELECT * FROM score WHERE degree BETWEEN 60 AND 80;
SELECT * FROM score WHERE degree >= 60 AND  degree <=80;

-- 5.查询 score 表中成绩为 85, 86 或 88 的行

SELECT * FROM score WHERE degree IN (85,86,88);

-- 6.查询 student 表中 '95031' 班或性别为 '女' 的所有行

SELECT * FROM student WHERE class = '95031' OR sex ='女';

-- 7.以 class 降序的方式查询 student 表的所有行
-- DESC: 降序，从高到低
-- ASC（默认）: 升序，从低到高

SELECT * FROM student ORDER BY class DESC;

-- 8.以 c_no 升序、degree 降序查询 score 表的所有行

SELECT * FROM score ORDER BY c_no ASC,degree DESC;

-- 9.查询 "95031" 班的学生人数
-- count

SELECT COUNT(*) FROM student WHERE class ="95031";

-- 10.查询 score 表中的最高分的学生学号和课程编号（子查询或排序查询）
-- (SELECT MAX(degree) FROM score): 子查询，算出最高分
-- a
SELECT * FROM score  WHERE degree = (SELECT MAX(degree) FROM score);
-- b
SELECT * FROM score ORDER BY degree DESC LIMIT 0,1;

-- 11. 分页查询 - 查询成绩最好的学生记录
-- limit a,b 从完后查多少条数据 a从0开始

SELECT * FROM score ORDER BY degree  ASC LIMIT 0,1;

-- 12. 查询每门课的平均成绩。
-- avg
-- group by
SELECT c_no, AVG(degree) FROM score GROUP BY c_no;

-- 13.分组条件与模糊查询
-- 查询 score 表中至少有 2 名学生选修，并以 3 开头的课程的平均分数。
-- having 条件查询
-- like 以什么开头

SELECT c_no,AVG(degree),COUNT(1) FROM score GROUP BY c_no HAVING c_no LIKE '3%' AND COUNT(1) > 1;

-- 13.多表查询 
-- 查询所有学生的 name，以及该学生在 score 表中对应的 c_no 和 degree
-- a. 内关联查询
SELECT st.name,sc.c_no,sc.degree FROM student AS st INNER JOIN score AS sc ON st.no = sc.s_no;
-- b. 直接关联查询 缺点 不知道哪些关联条件哪些是过滤条件
SELECT st.name,sc.c_no,sc.degree FROM student st,score sc WHERE st.no=sc.s_no;

-- 14.查询所有学生的 no 、课程名称 ( course 表中的 name ) 和成绩 ( score 表中的 degree ) 列。
-- a.
SELECT sc.s_no,co.name,sc.degree FROM  score sc 
	INNER JOIN course co ON sc.c_no = co.no;

-- 15. 查询所有学生的 name 、课程名 ( course 表中的 name ) 和 degree 。	
-- a.

SELECT st.name  AS s_name ,co.name AS c_name,sc.degree FROM student st INNER JOIN score sc ON st.no=sc.s_no 
INNER JOIN course co ON sc.c_no = co.no;

-- 16.子查询加分组求平均分
-- 查询 95031 班学生每门课程的平均成绩
-- a.
SELECT c_no,AVG(degree) FROM score sc WHERE sc.s_no IN (SELECT NO FROM student WHERE class = '95031') GROUP BY c_no;
-- b.
SELECT c_no,AVG(degree) FROM score sc LEFT JOIN student st ON sc.s_no = st.no WHERE class = '95031' GROUP BY sc.c_no;

-- 17. 子查询
-- 查询在 3-105 课程中，所有成绩高于 109 号同学的记录。
-- 子查询
SELECT * FROM score WHERE c_no='3-105' AND  degree > (SELECT degree FROM score WHERE c_no ='3-105' AND s_no = '109');

-- 18. 查询所有成绩高于 109 号同学的 3-105 课程成绩记录。
-- 子查询
SELECT * FROM score WHERE  degree > (SELECT degree FROM score WHERE c_no ='3-105' AND s_no = '109');

-- 19. 查询所有和 101 、108 号学生同年出生的 no 、name 、birthday 列。
-- year 
-- in 
SELECT st.no,st.name,st.birthday FROM  student st WHERE YEAR(st.birthday) IN (SELECT YEAR(birthday) FROM student WHERE (NO = '101' OR NO = '108'));

-- 20.多层嵌套子查询
-- 查询 '张旭' 教师任课的学生成绩表
-- a
SELECT * FROM score WHERE c_no IN (SELECT NO FROM course WHERE t_no = (SELECT NO FROM teacher WHERE NAME ='张旭'));
-- b
SELECT * FROM score WHERE c_no IN (SELECT co.no FROM course AS co INNER JOIN teacher te ON co.t_no=te.no AND te.name ='张旭');

--  21. 多表查询
-- 查询某选修课程多于5个同学的教师姓名
-- 在此之前向 score 插入一些数据，以便丰富查询条件。
INSERT INTO score VALUES ('101', '3-105', '90');
INSERT INTO score VALUES ('102', '3-105', '91');
INSERT INTO score VALUES ('104', '3-105', '89');
-- 单个子查询
SELECT te.* FROM teacher AS te INNER JOIN course co ON te.no = co.t_no WHERE co.no IN (SELECT c_no FROM score GROUP BY c_no HAVING COUNT(1)>4);

-- 22.查询 “计算机系” 课程的成绩表。

SELECT * FROM score WHERE c_no IN (SELECT co.no FROM course co INNER JOIN teacher te ON co.t_no = te.no AND te.department = '计算机系');

-- 23. UNION 和 NOTIN 的使用
-- 查询 计算机系 与 电子工程系 中的不同职称的教师
-- NOT: 代表逻辑非
SELECT * FROM teacher WHERE department = '计算机系' AND profession NOT IN (
    SELECT profession FROM teacher WHERE department = '电子工程系'
)
-- 合并两个集
UNION
SELECT * FROM teacher WHERE department = '电子工程系' AND profession NOT IN (
    SELECT profession FROM teacher WHERE department = '计算机系'
);

-- 24. 查询课程 3-105 且成绩 至少 高于 3-245 的 score 表。
-- any 
SELECT * FROM score WHERE c_no = '3-105' AND degree > ANY (SELECT degree FROM score WHERE c_no = '3-245') ORDER BY degree DESC;
-- min
SELECT * FROM score WHERE c_no ='3-105' AND degree > (SELECT MIN(degree) FROM score WHERE c_no = '3-245') ORDER BY degree DESC;

-- 25. 查询课程 3-105 且成绩高于 3-245 的 score 表。
-- ALL 表示所有
SELECT * FROM score WHERE c_no = '3-105' AND degree > ALL (SELECT degree FROM score WHERE c_no = '3-245');
-- max 大于最大就是所有
SELECT * FROM score WHERE c_no = '3-105' AND degree > (SELECT MAX(degree) FROM score WHERE c_no ='3-245');

-- 26. 查询某课程成绩比该课程平均成绩低的 score 表。
--
SELECT * FROM score a WHERE degree < (SELECT AVG(degree) FROM score b WHERE a.c_no = b.c_no);
-- left join
SELECT a.* FROM score a LEFT JOIN (SELECT c_no,AVG(degree) AS degree FROM score  GROUP BY c_no) AS b ON a.c_no = b.c_no WHERE a.degree < b.degree;

-- 27. 查询所有任课 ( 在 course 表里有课程 ) 教师的 name 和 department 
-- a
SELECT te.name,te.department FROM teacher te WHERE te.no IN (SELECT t_no FROM course);
-- b
SELECT DISTINCT te.name , department FROM teacher  te INNER JOIN course  co ON te.no = co.t_no;

-- 28. 条件加组筛选
-- 查询 student 表中至少有 2 名男生的 class
-- where 与 having 的区别  where 是在分组条件之前进行执行，having是在分组条件之后进行筛选
SELECT class FROM student WHERE sex ='男' GROUP BY class HAVING COUNT(*) > 1;

-- 29. NOTLIKE 模糊查询取反
-- 查询 student 表中不姓 "王" 的同学记录
--  not like 
SELECT * FROM student WHERE NAME NOT LIKE '王%';

-- 30.YEAR 与 NOW 函数
-- 查询 student 表中每个学生的姓名和年龄
SELECT NAME,YEAR(NOW())-YEAR(birthday) AS age FROM student;

-- 31. MAX 与 MIN 函数
-- 查询 student 表中最大和最小的 birthday 值
SELECT MIN(birthday),MAX(birthday) FROM student;

-- 32.多段排序
-- 以 class 和 birthday 从大到小的顺序查询 student 表。
SELECT * FROM student ORDER BY class DESC, birthday DESC;

-- 33. 查询 "男" 教师及其所上的课程。
SELECT te.* FROM course co INNER JOIN teacher te ON co.t_no = te.no AND te.sex='男';

-- 34. MAX 函数与子查询
-- 查询最高分同学的 score 表。
SELECT * FROM score WHERE degree = (SELECT MAX(degree) FROM score);

-- 35. 查询和 "李军" 同性别的所有同学 name 。
SELECT NAME,sex FROM student WHERE sex = (SELECT sex FROM student WHERE NAME ='李军');

-- 36. 查询和 "李军" 同性别且同班的同学 name 
-- . a子查询
SELECT NAME, sex, class FROM student WHERE sex = (SELECT sex FROM student WHERE NAME = '李军') AND class = (SELECT class FROM student WHERE NAME = '李军');
-- . b关联查询
SELECT a.name,a.sex FROM student a INNER JOIN student b ON a.sex = b.sex AND a.class =b.class AND b.name='李军';

-- 37. 查询所有选修 "计算机导论" 课程的 "男" 同学成绩表。
-- inner join 
SELECT sc.* FROM score sc INNER JOIN course co ON sc.c_no = co.no AND co.name='计算机导论'  INNER JOIN student st ON st.no = sc.s_no AND st.sex='男' ; 
-- left join 特别说明  
SELECT sc.* FROM score sc LEFT JOIN course co ON sc.c_no = co.no  LEFT JOIN student st ON st.no = sc.s_no WHERE  st.sex='男' AND co.name='计算机导论';
-- 38. 按等级查询
CREATE TABLE grade (
    low INT(3),
    upp INT(3),
    grade CHAR(1)
);

INSERT INTO grade VALUES (90, 100, 'A');
INSERT INTO grade VALUES (80, 89, 'B');
INSERT INTO grade VALUES (70, 79, 'C');
INSERT INTO grade VALUES (60, 69, 'D');
INSERT INTO grade VALUES (0, 59, 'E'); 
SELECT * FROM grade;
-- 
SELECT sc.s_no,sc.c_no,sc.degree, gr.grade FROM score sc, grade gr WHERE sc.degree BETWEEN gr.low AND gr.upp; 

-- 连接查询

CREATE DATABASE testJoin;
CREATE TABLE person (
    id INT,
    NAME VARCHAR(20),
    cardId INT
);
CREATE TABLE card (
    id INT,
    NAME VARCHAR(20)
);
INSERT INTO card VALUES (1, '饭卡'), (2, '建行卡'), (3, '农行卡'), (4, '工商卡'), (5, '邮政卡');

INSERT INTO person VALUES (1, '张三', 1), (2, '李四', 3), (3, '王五', 6);
SELECT * FROM person;

```

