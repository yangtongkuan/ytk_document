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

```

