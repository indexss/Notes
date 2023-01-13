# MySQL笔记

### 1、基本介绍

1. #### SQL分类

   DML: 增删改查

   DCL:用户 权限 事物

   DDL: 逻辑库 数据表 视图 索引

2. #### SQL语句不分大小写 但是字符串区分大小写

   建议关键字大写 非关键字小写

   以英文分号结尾

   空格和换行没有限制

3. #### 注释: # or /* */

### 2、数据库的创建以及数据库表的相关操作

1. #### 创建逻辑空间

   ```sql
   CREATE DATABASE name; #创建逻辑空间
   SHOW DATABASES; #展示空间
   DROP DATABASE name; #删除逻辑空间 
   ```

2. #### 创建数据表

   ```sql
   USE 逻辑空间名; #使用某个逻辑空间
   CREATE TABLE 表名(
   	列名1 数据类型 [约束] [COMMENT 注释],
   	列名2 数据类型 [约束] [COMMENT 注释],
     ......
   )[COMMENT = 注释];
   ```

   中括号内容为可选内容

   ```sql
   CREATE TABLE student(
   	id INT UNSIGNED PRIMARY KEY,
     name VARCHAR(20) NOT NULL,
     sex CHAR(1) NOT NULL,
     birthday DATE NOT NULL,
     tel CHAR(11) NOT NULL, #定长字符串
     remark VARCHAR(200) #不定长字符串
   );
   INSERT INTO student VALUES(1, "李强", "男", "1995-05-15", "19903430351", NULL);
   ```

   数据表的其他操作

   ```sql
   SHOW tables; #打印逻辑空间中表的名字
   DESC student; #打印表的情况
   SHOW CREATE TABLE student; #展示建表时的语句
   DROP TABLE student; #删除数据表
   ```

   添加字段

   ```sql 
   ALTER TABLE 表名称
   ADD 列1 数据类型 [约束] [COMMENT 注释],
   ...;
   ```

   修改字段类型与约束

   ```sql
   ALTER TABLE 表名称
   MODIFY 列1 数据类型 [约束] [COMMENT 注释],
   ...;
   ```

   修改字段名称

   ```sql
   ALTER TABLE 表名称
   CHANGE 列1 新列名1 数据类型 [约束] [COMMENT 注释],
   ...;
   ```

   删除字段

   ```sql
   ALTER TABLE 表名称
   DROP 列1,
   ...;
   ```

3. #### 数据库范式

   构建数据库必须遵循一定的规则，也就是范式。

   关系型数据库有6种范式，一般情况下，只满足第三范式就行。现在先介绍前三个范式。

   1. ##### 原子性

      数据库的基本要求，不满足这一点就不是关系型数据库。

      每一列都应该是不可分割的基本数据项，不能有多个值，也不能有重复的属性.

      `例如：班级：高三年级一班，这个就可以拆分成高三和一班，不符合原子性。`

   2. ##### 唯一性

      数据表中的每条记录必须是唯一的。为了实现区分，通常要为表加上一个列用来存储唯一标识，这个唯一属性被称作主键列。

      | 学号 | 考试成绩 | 日期       |
      | ---- | -------- | ---------- |
      | 230  | 58       | 2018-07-15 |
      | 230  | 58       | 2018-07-15 |

      这个人在同一天进行了两次考试，但由于考试成绩相同，无法区分两行。需要加上流水号。

      | 流水号       | 学号 | 考试成绩 | 日期       |
      | ------------ | ---- | -------- | ---------- |
      | 201807152687 | 230  | 58       | 2018-07-15 |
      | 201807152694 | 230  | 58       | 2018-07-15 |

      这样就保证了唯一性。

   3. ##### 关联性

      每列都与主键有直接关系，不存在**传递依赖**。

      | 爸爸 | 儿子 | 女儿 | 女儿的玩具 | 女儿的衣服 |
      | :--: | :--: | :--: | :--------: | :--------: |
      | 陈华 | 陈晨 | 陈婷 |    娃娃    |    校服    |

      这里发现，女儿的玩具和女儿的衣服与女的产生了传递依赖，与主键“爸爸”没有直接关系。

      | 爸爸 | 儿子 | 女儿 |
      | :--: | :--: | :--: |
      | 陈华 | 陈晨 | 陈婷 |

      | 女儿 | 玩具 | 衣服 |
      | :--: | :--: | ---- |
      | 陈婷 | 娃娃 | 校服 |

      拆分成两个表，就能满足第三范式。

      由于主键查询速度更快，如果产生了传递性依赖，就会加大搜索难度。所以不能把两个主键放在同一行。

      按照第三范式的要求，数据可以拆分到不同的数据表，彼此驳斥关联。

      <img src="https://s3.bmp.ovh/imgs/2023/01/07/4566abccdd8b8cb4.png" style="zoom: 50%;" />

      sql中的字段约束一共四种：

      | 约束名称 | 关键字      | 描述                           |
      | -------- | ----------- | ------------------------------ |
      | 主键约束 | PRIMARY KEY | 字段值唯一，且不能为NULL       |
      | 非空约束 | NOT NULL    | 字段值不能为NULL               |
      | 唯一约束 | UNIQUE      | 字段值唯一，且可以为NULL       |
      | 外键约束 | FOREIGN KEY | 保持关联数据的逻辑性（不推荐） |

      1. ##### 主键约束

         主键一定要使用数字类型，因为数字的检索速度非常快

         如果主键是数字类型，那么还可以设置自动增长。自增原理

         ```sql
         CREATE TABLE t_teacher(
         	id INT PRIMARY KEY AUTO_INCREMENT,
           ...
         );
         ```

         

      2. ##### 非空约束

         NULL并不是空字符串

         ```sql
         CREATE TABLE t_teacher(
         	id INT PRIMARY KEY AUTO_INCREMENT,
         	married BOOLEAN NOT NULL DEFAULT FALSE, #非空 默认为未结婚
           ...
         );
         ```

         

      3. ##### 唯一约束

         加上UNIQUE就行了 没什么好说的

      4. ##### 外键约束

         ```sql
         CREATE TABLE t_dept( # 父表
             deptno INT UNSIGNED PRIMARY KEY,
             dname VARCHAR(20) NOT NULL UNIQUE,
             tel CHAR(4) UNIQUE
         );
         
         CREATE TABLE t_emp( # 子表
             empno INT UNSIGNED PRIMARY KEY,
             ename VARCHAR(20) NOT NULL,
             sex ENUM("男"， "女") NOT NULL,
             deptno INT UNSIGNED NOT NULL,
             hiredate DATE NOT NULL,
             FOREIGN KEY (deptno) REFERENCES t_dept(deptno)
         );
         ```

         **不推荐使用外键约束**的原因是，有可能会造成外键约束的闭环问题。

         一但形成闭环结构，我们无法删除任何一张表的记录。

         真实情况中，有上百张表，很容易形成外键约束。

4. #### 索引机制

   一单数据排序之后，查找的速度就会翻倍。索引就是Excel里面的下拉框

   1. ##### 如何创建索引 

      ```sql
      CREATE TABLE 表名称(
      	......,
      	INDEX [索引名称](字段),
        ......
      );
      CREATE TABLE t_message(
      	id INT UNSIGNED PRIMARY KEY,
        content VARVHAR(200) NOT NULL,
        type ENUM("公告","通知","个人信息"),
        create_time TIMESTAMP NOT NULL,
        INDEX idx_type (type)
      );
      ```

   2. ##### 如何添加与删除索引

      ```sql
      CREATE INDEX 索引名称 ON 表名(字段);
      ALTER TABLE 表名 ADD INDEX [索引名](字段);
      SHOW INDEX FROM 表名;
      DROP INDEX 索引名称 ON 表名;
      ```

   - 数据量很大，且经常被查询的数据表可以设置索引。

   - 索引只添加在常作为检索条件的字段上面。

   - 不要再大字段上创建索引

   一切都是因为，索引原理为二叉树，维护二叉树需要资源。不要浪费资源。

### 3、数据库的查询

1. #### 普通查询

   1. ##### 记录查询

   ```sql
   SELECT * FROM t_emp;
   SELECT empno, ename, sal FROM t_emp;
   ```

   2. ##### 使用列别名

   ```sql
   SELECT
   	empno,
   	sal*12 AS "income"
   FROM t_emp;
   ```

   执行顺序 读取SQL语句 -> FROM -> SELECT

   3. ##### 数据分页

   ```sql
   SELECT ... FROM ... LIMIT 起始位置, 偏移量;
   ```

   FROM -> SELECT -> LIMIT

   4. ##### 结果集排序

   ```sql
   SELECT ... FROM ... ORDER BY 关键字列名 [ASC | DESC]; #asc升序 desc降序
   ```

   排序关键字，如果是数字，那就按数字大小排序，如果是日期，就按日期大小排序，如果是字符串就按照字符集序号排序。

   关键字相同时，默认情况按照主键大小排序。如果非默认，可以在ORDER BY后面加很多关键字。

   ```sql
   SELECT ... FROM ... ORDER BY 关键字1 [ASC|DESC], 关键字2 [ASC|DESC];
   ```

   FROM -> SELECT -> ORDER BY -> LIMIT

   5. ##### 去除重复记录

   ```sql
   SELECT DISTINCT ... FROM ...;
   ```

   使用DISTINCT的SELECT子句中只能查询一列数据，如果想查询多列，去除重复记录就会失效。

   DISTINCT只能在SELECT子句中使用一次，且紧接着SELECT。

2. #### 条件查询

   ```sql
   SELECT ... FROM ... WHERE 条件 [AND|OR]条件 ...;
   SELECT empno, ename, sal FROM t_emp WHERE deptno=10 AND sal>=2000;
   
   SELECT empno, ename, sal, hiredate
   FROM t_emp
   WHERE deptno=10 AND (sal+IFNULL(comm,0))*12 >=15000
   AND DATEDIFF(NOW(),hiredate)/365 >=20;
   ```

   - 特殊运算符：

   = 表示等于 而不是==

   IN 表示包含 例子：```deptno IN (10,30,40```

   DATE进行比较 要和字符串比较 ```hiredate<"1985-01-01"```

   | 表达式      | 意义       | 例子                       |
   | ----------- | ---------- | -------------------------- |
   | IS NULL     | 为空       | comm IS NULL               |
   | IS NOT NULL | 不为空     | comm IS NOT NULL           |
   | BETWEEN AND | 范围       | sal BETWEEN 2000 AND 3000  |
   | LIKE        | 模糊查找   | ename LIKE "A%" #以A开头   |
   | REGEXP      | 正则表达式 | ename REGEXP "[a-zA-Z]{4}" |

   ```sql
   SELECT
   	ename, comm
   FROM t_emp WHERE comm IS NOT NULL OR comm=0
   AND sal BETWEEN 2000 AND 3000
   AND ename LIKE "%A%"; #含有A _下划线表示一个字母通配符
   AND ename REGEXP "^[\\u4e00-\\u9fa5]{2,4}$" #正则表达式
   ```

   ![](https://s3.bmp.ovh/imgs/2023/01/07/f72e1daf34a226b9.png)

   - 逻辑运算符

   AND OR NOT XOR(异或)

   - 按位运算符

   &与 |或 ~取反 ^异或 <<左移 >>右移

   - WHERE注意事项

   WHERE中，条件从左到右执行，所以应该把能筛选掉最多记录的条件放在左边

   FROM -> WHERE -> SELECT -> ORDER BY -> LIMIT

3. #### 聚合函数

   求平均月收入？

   ```sql
   SELECT AVG(sal + IFNULL(comm,0) FROM t_emp);
   ```

   - ##### SUM函数

     用于求和，用于数字类型，字符类型的统计结果为0，日期类型的统计结果是毫秒数相加。

     ```sql
     SELECT SUM(sal) FROM t_emp
     WHERE deptno IN (10,20);
     ```

   - ##### MAX函数

     用于获得非空值的最大值

     ```sql
     SELECT MAX(comm) FROM t_emp;
     #查询10，20部门中，月收入最高的员工
     SELECT MAX(sal+IFNULL(comm,0)) FROM t_emp
     WHERE deptno IN(10,20);
     #查询员工名字最长的是是几个字符
     SELECT MAX(LENGTH(ename)) FROM t_emp;
     ```

   - ##### MIN函数

     用于获得非空值的最小值

   - ##### AVG函数

     用于获得非空值的平均值，非数字类型的统计结果为0

   - ##### COUNT函数

     两种用法：

     COUNT(*)用于获得包含空值的记录数

     COUNT(列名)用于获得包含**非空值**的记录数

     ```sql
     #查询10，20部门中，底薪超过2000元且工龄超过15年的员工人数
     SELECT COUNT(*) FROM t_emp
     WHERE deptnp IN (10,20)
     AND sal>= 2000
     AND DATEDIFF(NOW(),hiredate)/365>=15;
     
     #查询1985年以后入职的员工。底薪超过公司平均底薪的员工数量 错误写法！！！！！
     SELECT COUNT(*) FROM t_emp
     WHERE sal>=AVG(sal) #聚合函数不能写到WHERE里
     AND hiredate>="1985-01-01";
     #因为执行顺序为FROM -> WHERE -> SELECT -> ORDER BY -> LIMIT
     #WHERE在执行的时候并没有执行SELECT，不知道统计主题，自然也就不能执行聚合函数
     #正确方法将在 10.分组查询 中给出
     ```

4. #### 分组查询

   默认情况下汇总函数对全表范围进行统计

   GROUP BY子句作用是通过一定的规则将一个数据集划分为若干个小的区域，然后针对每个小区域分别进行数据汇总处理。

   `分组查询是如果语句中含有GROUP BY子句，那么SELECT子句中的内容就必须遵守规定：SELECT子句中可以包含聚合函数，以及GROUP BY子句所分组的那一列，其余的内容均不可以出现在SELECT子句中`

   为什么？

   ```sql
   SELECT deptno, ROUND(AVG(sal)) #round四舍五入为整数
   FROM t_emp
   GROUP BY deptno;
   ```

   - ##### 逐级分组

     数据库支持多列分组条件，执行的时候逐级分组

     ```sql
     SELECT deptno,job,COUNT(*),AVG(sal)
     FROM t_emp GROUP BY deptno,job
     ORDER BY deptno;
     ```

   - ##### 对分组结果集再次做汇总计算

     ```sql
     SELECT
     deptno,COUNT(*),AVG(sal),MAX(sal),MIN(sal)
     FROM t_emp GROUP BY deptno WITH ROLLUP; #with rollup是对整个结果再次汇总
     ```

   - ##### GROUP_CONCAT函数

     GROUP_CONCAT函数可以把分组查询中某个字段拼接成一个字符串

     用这个函数可以解决分组不能查询其他列的问题。

     例如：查询每个部门底薪超过2000元的人数和员工姓名

     ```sql
     SELECT deptno,GROUP_CONCAT(ename),COUNT(*)
     FROM t_emp WHERE sal>=2000
     GROUP BY deptno;
     ```

     **执行顺序: FROM -> WHERE -> GROUP BY -> SELECT -> ORDER BY -> LIMIT**

   - ##### HAVING语句

     HAVING语句是和分组一起用的，用来解决分组查询遇到的困难。

     HAVING是一种条件判断的补充，可以连接一个聚合函数，但是不能两边都是聚合函数。

     例如：查询部门平均底薪超过2000元的部门编号

     ```sql
     #错误写法为
     SELECT deptno FROM t_emp
     WHERE AVG(sal) >=2000
     GROUP BY deptno; #错误原因为 聚合函数用到了WHERE上
     
     #正确写法为
     SELECT deptno FROM t_emp
     GROUP BY deptno HAVING AVG(sal)>=2000;
     ```

     查询每个部门中 1982年以后入职的员工超过两个人的部门编号

     ```sql
     SELECT deptno, COUNT(*)FROM t_emp
     WHERE hiredate>="1982-01-01"
     GROUP BY deptno HAVING COUNT(*) >=2;
     ```

   - ##### HAVING子句和GROUP BY的特殊用法

     按照数字1分组，sql会依据SELECT子句中的列进行分组，HAVING子句也可以正常使用

     从1开始数，SELECT后面的字段进行分组

     ```sql
     SELECT deptno, COUNT(*) FROM t_emp
     GROUP BY 1;   # === GROUP BY deptno
     ```

12. #### 表连接查询

    从多张表中提取条件，必须指定关联的条件。如果不定义关联条件，就会出现无条件的连接，两张表的数据产生交叉连接，进而产生笛卡尔积。

    连接范例:

    ```sql
    SELECT e.empno, e.ename, d.dname
    FROM t_emp e JOIN t_dept d
    ON e.deptno=d.deptno;
    ```

    - ##### 表连接分为内连接和外连接

      - ###### **内连接：结果集之中只保留符号连接条件的记录。**

        内连接的数据表不一定有同名字段，只哟啊字段之间符合逻辑关系就行

        查找多张连接表的“交集”。例如，上面的例子，部门表有40部门，但员工表没有人在40部门，那么40部门不会在结果中显示。

        ```sql
        #语法1
        SELECT ... FROM 表1
        [INNER] JOIN 表2 ON 连接条件
        [INNER] JOIN 表3 ON 连接条件
        ...
        #语法2
        SELECT ... FROM 表1 JOIN 表2 WHERE 连接条件;
        #语法3
        SELECT ... FROM 表1,表2 WHERE 连接条件;
        #结果一致，底层一致，效率一致
        ```

        - ###### 例：查询每个员工的工号，姓名，部门名称，底薪，职位，工资等级

        ```sql
        SELECT e.empno,e.ename,d.dname,e.sal,e.job,s.grade
        FROM t_emp e
        JOIN t_dept d ON e.deptno=d.deptno
        JOIN t_salgrade s ON e.sal BETWEEN s.losal AND s.hisal;
        ```

        - ###### 例：查询与SCOTT相同部门的员工都有谁？ - 相同数据表也可以表连接

        ```sql
        #方法一，子查询，嵌套查询。缺点：慢 
        #执行顺序
        #(1)FROM -> (2)ON -> (3)JOIN -> (4)WHERE -> (5)GROUP BY -> (6)WITH -> #(7)HAVING -> (8)SELECT -> (10)DISTINCT -> (11)ORDER BY -> (12)LIMIT
        #每次FROM表读出一行后，WHERE都要查询一次，查询次数多，速度慢。
        SELECT ename
        FROM t_emp
        WHERE deptno=(SELECT deptmo FROM t_emp WHERE ename="SCOTT")
        AND ename !="SCOTT";
        
        #方法二，表连接
        SELECT e2.ename
        FROM t_emp e1 JOIN t_emp e2 ON e1.deptno=e2.deptno
        WHERE e1.ename="SCOTT" AND e2.ename!="SCOTT";
        
        #顺序
        (8) SELECT (9)DISTINCT<Select_list>
        (1) FROM <left_table> (3) <join_type>JOIN<right_table>
        (2) ON<join_condition>
        (4) WHERE<where_condition>
        (5) GROUP BY<group_by_list>
        (6) WITH {CUBE|ROLLUP}
        (7) HAVING<having_condtion>
        (10) ORDER BY<order_by_list>
        (11) LIMIT<limit_number>
        ```

        - ###### 例：查询底薪超过工资平均底薪的员工信息

        ```sql
        #错误！ ON和WHERE一样，avg不能在on里面
        SELECT e2.empno,e2.ename,e2.sal
        FROM t_emp e1 JOIN t_emp e2 ON e2.sal>=AVG(e1.sal);
        
        #正确写法
        SELECT e.empno,e.ename,e.sal
        FROM t_emp e JOIN (SELECT AVG(sal) avg FROM t_emp) t ON e.sal>=t.avg;
        ```

        - ###### 例：查询RESEARCH部门的人数，最高底薪，最低底薪，平均底薪，平均工龄(取下)

        ```sql
        SELECT COUNT(*),MAX(e.sal),MIN(e.sal),AVG(e.sal),
        FLOOR(AVG(DATEDIFF(NOW(),e.hiredate)/365)) #对应是CEIL()
        FROM t_emp e
        JOIN t_dept d ON e.deptno=d.deptno
        WHERE d.dname="RESEARCH";
        ```

        - ###### 例：查询每种职业的最高工资，最低工资，平均工资，最高工资等级和最低工资等级

        ```sql
        SELECT
        e.job,
        MAX(e.sal+IFNULL(e.comm,0)),
        MIN(e.sal+IFNULL(e.comm,0)),
        AVG(e.sal+IFNULL(e.comm,0)),
        MAX(s.grade),MIN(s.grade)
        FROM t_emp e
        JOIN t_salgrade s
        ON e.sal+IFNULL(e.comm,0) BETWEEN s.losal AND s.hisal
        GROUP BY e.job
        ```

        - ###### 例：查询每个底薪超过部门平均底薪的员工信息

        ```sql
        SELECT e.ename,e.deptno,e.sal,t.avg
        FROM t_emp e JOIN
        (SELECT deptno,AVG(sal) AS avg FROM t_emp GROUP BY deptno) t
        ON e.deptno=t.deptno
        WHERE e.sal>=t.avg;
        ```

        

      - ##### **外连接：不管是否符合连接条件，记录都保留在结果集之中。**

        如果我们有临时人员，这些人没有具体部门编制，那么如果想查询每名员工以及他的部门信息时，采用内连接的话一定会漏掉临时人员的问题。所以要引入外连接。

        ```sql
        SELECT e.empno,e.ename,d.dname
        FROM t_emp e
        LEFT JOIN t_dept d ON e.deptno=d.deptno; 
        #左外连接，保留左表所有记录，右表怼上来，没有对应数据就留null
        #右外连接就是保留右表所有记录，左表怼上来。
        ```

        - ###### **例：查询每个部门的名称与部门的人数**

        ```sql
        SELECT d.dname,COUNT(e.deptno)  #如果用count(*)会有空部门为1的情况，
        #因为空部门也是一条记录
        #由于我们用e去右连接d，所以e的空记录就被省略了,所以用e的deptno
        FROM t_emp e
        RIGHT JOIN t_dept d ON e.deptno=d.deptno
        GROUP BY d.deptno;
        ```

        - ###### **例：查询每个部门的名称和部门的人数，包含无员工的部门，也包含自由部门**

          要采用UNION关键字让多个查询语句的结果集合并，也就是两个结果的并集

          `(查询语句) UNION (查询语句) UNION (查询语句)...`

          结果要包含陈浩的空部门，要算为NULL

        ```sql
        (
        SELECT d.dname, COUNT(e.deptno)
        FROM t_emp e RIGHT JOIN t_dept d ON e.deptno=d.deptno
        GROUP BY d.deptno
        ) 
        UNION
        (
        SELECT d.dname, COUNT(*)
        FROM t_emp e LEFT JOIN t_dept d ON e.deptno=d.deptno
        GROUP BY d.deptno
        );
        ```

        - ###### **例：查询每名员工的编号，姓名，部门，月薪，工资等级，工龄，上司编号，上司姓名，上司部门？**

        ```sql
        SELECT e1.ename                                  AS ename
             , e1.deptno                                 AS deptno
             , d.dname                                   AS dept
             , e1.sal + IFNULL(e1.comm, 0)               AS salary
             , s.grade                                   AS grade
             , FLOOR(DATEDIFF(NOW(), e1.hiredate) / 365) AS gl
             , e2.empno                                  AS mgrno
             , e2.ename                                  AS mgrname
             , h.dname                                   AS deptname
        FROM t_emp e1
                 LEFT JOIN t_emp e2 ON e1.mgr = e2.empno
                 LEFT JOIN t_dept d ON e1.deptno = d.deptno
                 LEFT JOIN t_salgrade s ON e1.sal BETWEEN s.losal AND s.hisal
                 LEFT JOIN (SELECT a.empno, b.dname
                            FROM t_emp a
                                     LEFT JOIN t_dept b ON a.deptno = b.deptno) h
                           ON e2.empno = h.empno;
        ```

        - ###### 外连接注意事项

          内连接只保留符合条件的记录，所以查询条件现在ON子句和WHERE子句中的效果相同，但是在外连接中，条件写在WHERE子句里，不符合条件的记录会被过滤掉，而不是保留。

13. #### 子查询

    子查询是一种查询中嵌套查询的语句

    - ###### 查询底薪超过公司平均底薪的员工信息

      ```sql
      SELECT
      	empno,ename,sal
      FROM t_emp
      WHERE sal>=(SELECT AVG(sal) FROM t_emp);
      ```

      当然，不推荐在WHERE中使用子查询！而是采用表连接，因为这种查询每有一条员工记录就会查询一次，这种叫做相关子查询，效率太低。应当避免相关子查询。

    - ###### 子查询可以写在三个地方：WHERE、FROM、SELECT。只有FROM子句的子查询最合理可取。

      FROM改写上方题：

      ```sql
      SELECT e.empno, e.ename, e.sal, t.acg
      FROM t_emp e JOIN
      (SELECT deptno,AVG(sal) AS avg FROM t_emp GROUP BY deptno) t
      ON e.deptno=t.deptno AND e.sal>=t.avg;
      ```

      SELECT子查询也是相关子查询，不推荐。

    - ###### 子查询可以分为单行子查询和多行子查询

      单行子查询的结果只有一条记录，多行子查询的结果有多行记录。

      多行子查询只能出现在WHERE子句和FROM子句中，不能出现在SELECT中。

      例：用子查询查找FORD和MARTIN两人的同事

      ```sql
      SELECT ename
      FROM t_emp
      WHERE deptno IN
      (SELECT deptno FRROM t_emp WHERE ename IN("FORD","MARTIN"))
      AND ename NOT IN("FORD","MARTIN");
      ```

      WHERE子句中，可以使用IN, ALL, ANY, EXISTS关键字来处理多行表达式结果集的条件判断

      查询比所有人底薪高，用ALL。ANY的意思是，比其中一个人高就行，不用所有，也可以所有。

      EXISTS关键字就是把原来在子查询之外的条件判断，写到了子查询里面

       `SELECT ... FROM 表名 WHERE [NOT] EXISTS (子查询)`

      ```sql
      SELECT ename
      FROM t_emp
      WHERE EXISTS(
                    SELECT *
                    FROM t_salgrade
                    WHERE sal BETWEEN losal AND hisal
                      AND grade IN (3, 4)
                );
      ```

      其实就是换了一种方法写WHERE

### 4、数据的插入

1. #### 数据插入INSERT

   INSERT语句可以向数据表中写入记录，**可以是一条记录，也可以是多条记录。**

   ```sql
   INSERT INTO 表名 (字段1, 字段2, ...)
   VALUES (值1, 值2, ...);
   #表名后面的字段声明的确可以不写，但是执行速度会受到影响
   INSERT INTO 表名 (字段1, 字段2, ...)
   VALUES (值1, 值2, ...),(值1, 值2, ...);
   ```

   INSERT语句方言

   ```sql
   INSERT [INTO] 表名 SET 字段1=值1, 字段2=值2, ....;
   ```

2. #### IGNORE关键字

   IGNORE关键字会让INSERT只插入数据库不存在的记录和不冲突，不报错的记录

   ```sql
   INSERT [IGNORE] INTO 表名...;
   ```

### 5、数据的更新

1. #### UPDATE语句

   ```sql
   UPDATE [IGNORE] 表名
   SET 字段1=值1, 字段2=值2, ...
   [WHERE 条件1 ...]
   [ORDER BY ...] #先排序，后修改
   [LIMIT ...]; # LIMIT 10 表示修改前10条数据
   ```

   - ###### 例：把每个员工的编号和上司编号+1，用ORDER BY完成

     ```sql
     UPDATE t_emp SET empno=empno+1, mgr=mgr+1
     ORDER BY empno DESC ;
     ```

   - ###### 例：把月收入前三名的员工底薪减100元，用LIMIT完成

     ```sql
     UPDATE t_emp SET sal=sal+100
     ORDER BY (sal+IFNULL(comm,0)) DESC
     LIMIT 3;
     ```

   - 例：把10部门中，工龄超过20年的员工，底薪增加200元

     ```sql
     UPDATE t_emp SET sal = sal+200
     WHERE DATEDIFF(NOW(),hiredate)/365>=20
     AND deptno=10
     ```

   - 例：把ALLEN调往RESEARCH部门，职务调整为ANALYST

     ```sql
     #常规写法
     UPDATE t_emp e JOIN t_dept d ON e.deptno=d.deptno
     SET e.deptno=(SELECT deptno FROM t_dept WHERE dname="RESEARCH"), e.job="ANALYST"
     WHERE e.ename="ALLEN";
     
     #好的写法
     UPDATE t_emp e JOIN t_dept d
     SET e.deptno=d.deptno, e.job="ANALYST"
     WHERE e.ename="ALLEN" AND d.dname="REACHER";
     ```

     

2. #### UPDATE语句的表连接

   由于相关子查询的查询效率极低，所以可以利用表连接的方法来改造UPDATE语句

   ```sql
   UPDATE 表1 JOIN 表2 ON 条件
   SET 字段1=值1, 字段2=值2, ...;
   
   #也可以写成
   UPDATE 表1,表2
   SET 字段1=值1,字段2=值2, ...
   WHERE 连接条件;
   ```

   ###### 例：把底薪低于公司平均底薪的员工，底薪增加150元

   ```sql
   UPDATE t_emp e JOIN
   (SELECT AVG(sal) AS avg FROM t_emp) t
   ON e.sal < t.avg
   SET e.sal = e.sal+150;
   ```

   UPDATE语句的表连接可以是内连接，也可以是外连接

   ```sql
   UPDATE 表1 [LEFT|RIGHT] JOIN 表2 ON 条件
   SET 字段1=值1,字段2=值2, ...;
   ```

   ###### 例：把没有部门的员工，或者SALES部门低于2000元底薪的员工调往20部门

   ```sql
   UPDATE t_emp e LEFT JOIN t_dept d
   SET e.deptno=20
   WHERE e.deptno IS NULL OR (d.dname="SALES" AND sal<2000);
   ```

### 6、数据的删除

1. #### DELETE语句

   - ###### 用于删除记录，不能删除表，语法如下

   ```sql
   DELETE [IGNORE] FROM 表名 #若存在外键约束，有的可能不能删，不加IGNORE就会报错，加上就会不删，忽略。
   [WHERE 条件1, 条件2, ...]
   [ORDER BY ...]
   [LIMIT ...];
   ```

   - ###### 例：删除10部门中，工龄超过20年的员工记录

   ```sql
   DELETE FROM t_emp
   WHERE deptno=10
   AND DATEDIFF(NOW(),hiredate)/365>20;
   ```

   - ###### 例：删除20部门中工资最高的员工记录

   ```sql
   DELETE FROM t_emp
   WHERE sal=(SELECT MAX(sal) FROM t_emp);
   
   #非子查询方法
   DELETE FROM t_emp
   WHERE deptno=20
   ORDER BY sal+IFNULL(comm,0) DESC
   LIMIT 1;
   ```

   - ###### 例：删除SALES部门和该部门全部员工记录

   ```sql
   DELETE e,d
   FROM t_emp e JOIN t_dept d ON e.deptno=d.deptno
   WHERE d.dname="SALES";
   ```

   - ###### DELETE表连接

   ```sql
   DELETE 表1, ... FROM 表1 JOIN 表2 ON 条件
   [WHERE 条件1, 条件2, ...]
   [ORDER BY ...]
   [LIMIT ...];
   ```

   - ###### 例：删除每个部门低于平均底薪的员工记录

   ```sql
   DELETE e
   FROM t_emp e JOIN (SELECT deptno,AVG(sal) AS sal FROM t_emp GROUP BY deptno) t
   ON e.deptno=t.deptno AND e.sal<t.sal;
   ```

   - ###### 例：删除员工KING和他直接下属的员工记录，用表连接实现

   ```sql
   DELETE e
   FROM t_emp e JOIN (SELECT empno FROM t_emp WHERE ename="KING")
   ON e.mgr=t.empno OR e.empno=t.empno;
   ```

   - ###### DELETE 也支持外连接

   ```sql
   DELETE 表1, ... FROM 表1 [LEFT|RIGHT] JOIN 表2 ON 条件 ...;
   ```

   - ###### 删除SALES部门的员工，以及没有部门的员工

   ```sql
   DELETE e
   FROM t_emp e LEFT JOIN t_dept d ON e.deptno=d.deptno
   WHERE d.dname="SALES" OR d.dname IS NULL;
   ```

2. #### TRUNCATE语句

   ##### 快速删除 表的全部数据

   DELETE语句是在事务机制下删除记录，删除记录前，要把将要删除额度记录保存在日志文件里，再删除记录，这样就会导致删除很慢。可以用TRUNCATE语句在事务机制外删除记录，速度远超DELETE

   ```sql
   TRUNCATE TABLE 表名;
   ```

### 7、sql基本函数

1. #### 数字函数

   | 函数    | 功能       | 用例        |
   | ------- | ---------- | ----------- |
   | ABS     | 绝对值     | ABS(-100)   |
   | ROUND   | 四舍五入   | ROUND(4.62) |
   | FLOOR   | 向下取整   | FLOOR(9.9)  |
   | CEIL    | 向上取整   | CEIL(3.2)   |
   | POWER   | 求幂       | POWER(2,3)  |
   | LOG     | 对数函数   | LOG(7,3)    |
   | LN      | 对数函数   | LN(10)      |
   | SQRT    | 开根       | SQRT(9)     |
   | PI      | π          | PI()        |
   | SIN     | sine       | SIN(1)      |
   | COS     | cosine     | COS(1)      |
   | TAN     | tangent    | TAN(1)      |
   | COT     | cotangent  | COT(1)      |
   | RADIANS | 角度转弧度 | RADIANS(30) |
   | DEGREES | 弧度转角度 | DEGREES(1)  |

2. #### 字符函数

   | 函数      | 功能          | 用例                                               |
   | --------- | ------------- | -------------------------------------------------- |
   | LOWER     | 转换小写字符  | LOWER(ename)                                       |
   | UPPER     | 转换大些字符  | UPPER(ename)                                       |
   | LENGTH    | 字符数量      | LENGTH(ename)                                      |
   | CONCAT    | 连接字符串    | CONCAT(sal,"$")                                    |
   | INSTR     | 字符出现位置  | INSTR(ename,'A')                                   |
   | INSERT    | 插入/替换字符 | INSERT("你好",1,0,"先生") #1是位置，0是替换0个字符 |
   | REPLACE   | 替换字符      | REPLACE("你好先生","先生","女士")                  |
   | SUBSTR    | 截取字符串    | SUBSTR("你好先生",3,2) #起始位置(1开始数) 偏移量   |
   | SUBSTRING | 截取字符串    | SUBSTRING("你好先生",3,2)                          |
   | LPAD      | 左侧填充字符  | LPAD("Hello",10,"*") # 10为最后字符串长度          |
   | RPAD      | 右侧填充字符  | RPAD("Hello",10,"*")                               |
   | TRIM      | 去除首位空格  | TRIM(" 你好先生")                                  |

3. #### 日期函数

| 函数                     | 说明                    | 用例                               |
| ------------------------ | ----------------------- | ---------------------------------- |
| NOW()                    | 返回yyyy-MM-dd hh:mm:ss | NOW()                              |
| CURDATE()                | 返回yyyy-MM-dd          | CURDATE()                          |
| CURTIME()                | 返回hh:mm:ss            | CURTIME()                          |
| DATE_FORMAT(日期,表达式) | 按表达式返回日期        | DATE_FORMAT(hiredate, "%Y")        |
| DATE_ADD()               | 前后偏移日期，单位灵活  | DATE_ADD(日期,INTERVAL 偏移量 单位 |
| DATE_DIFF()              | 计算两个日期相差天数    | DATEDIFF(日期. 日期)               |

| 占位符 | 作用         | 占位符 | 作用         |
| ------ | ------------ | ------ | ------------ |
| %Y     | 年份         | %m     | 月份         |
| %d     | 日期         | %w     | 星期（数字） |
| %W     | 星期（名称） | %j     | 本年第几天   |
| %U     | 本年第几周   | %H     | 小时（24）   |
| %h     | 小时（12）   | %i     | 分钟         |
| %s     | 秒           | %r     | 时间（12）   |
| %T     | 时间（24）   |        |              |

- ###### 利用日期函数，查询明年我的生日是星期几？

  ```sql
  SELECT DATE_FORMAT("2023-10-17","%W");
  ```

- ###### 利用日期函数，查询1981年上半年入职的员工有多少？

  ```sql
  SELECT COUNT(*) FROM t_emp
  WHERE DATE_FORMAT(hiredate,"%Y")=1981
  AND DATE_FORMAT(hiredate,"%m")<=6;
  ```

- ###### 日期函数注意事项

  sql数据库俩面，两个日期不能加减，日期也不能和数字加减，只能用函数

  如果你对数字加数字后，那么你得到的将不是日期这个数据结构，而是一个数字

  如果想要偏移，所用DATE_ADD()函数

  ```sql
  SELECT DATE_ADD(NOW(),INTERVAL 15 DAY);
  SELECT DATE_ADD(NOW(),INTERVAL -300 MINUTE);
  #嵌套使用DATE_ADD从而达到偏移多少天，多少小时的目的
  ```

- ###### 对手机号的处理

  ```sql
  SELECT LPAD(SUBSTRING("13812345678",8,4),11,"*"); 
  ```

4. #### 条件函数

   - ###### SQL语句可以利用条件函数来实现编程语言里的条件判断

   - ###### IFNULL(表达式,值)

   - ###### IF(表达式,值1,值2) 为真返回值1，不为真返回值2

   - ###### 例：中秋节公司发放礼品，SALES部门发放产品A，其余部门发放礼品B，打印每名员工获得的礼品

     ```sql
     SELECT e.empno, e.ename, d/dname
     IF(d.dname="SALES","礼品A","礼品B")
     FROM t_emp e JOIN t_dept d ON e.deptno=d.deptno;
     ```

   - ###### 复杂的条件判断可以用条件语句来实现，比IF语句功能更强大

     ```sql
     CASE
     	WHEN 表达式 THEN 值1
     	WHEN 表达式 THEN 值2
     	...
     	ELSE 值N
     END
     ```

   - ###### 例：公司年庆决定组织员工集体旅游，每个部门旅游的目的地是不同的。SALES部门去P1，ACCOUNTING部门去P2地点，RESEARCH部门P3地点，查询每名员工的旅行地点

     ```sql
     SELECT e.empno e.name,
       CASE
         WHEN d.dname="SALES" THEN "p1"
         WHEN d.dname="ACCOUNTING" THEN "p2"
         WHEN d.dname="RESEARCH" THEN "p3"
       END AS place
     FROM t_emp e JOIN t_dept d ON e.deptno=d.deptno;
     ```

   - ###### 例：公司决定为员工调整基本工资，具体调整方案如下:

     | 序号 | 条件                           | 涨幅  |
     | ---- | ------------------------------ | ----- |
     | 1    | SALES部门中工龄超过20年        | 10%   |
     | 2    | SALES部门中工龄不满20年        | 5%    |
     | 3    | ACCOUNTING部门                 | 300元 |
     | 4    | RESEARCH部门中低于部门平均底薪 | 200元 |
     | 5    | 没有部门的员工                 | 100元 |

     ```sql
     UPDATE t_emp e LEFT JOIN t_dept d ON e.deptno=d.deptno
     LEFT JOIN (SELECT deptno,AVG(sal) AS avg FROM t_emp GROUP BY deptno) t
     ON e.deptno=t.deptno
     SET e.sal=(
     	CASE
       	WHEN d.dname="SALES" AND DATEDIFF(NOW(),e.hiredate)/365>=20
       		THEN e.sal*1.1
         WHEN d.dname="SALES" AND DATEDIFF(NOW(),e.hiredate)/365<20
       		THEN e.sal*1.05
         WHEN d.dname="ACCOUNTING"
       		THEN e.sal+300
         WHEN d.dname="RESEARCH" AND e.sal<t.avg
       		THEN e.sal+200
       	WHEN e.deptno IS NULL
       		THEN e.sal+100
       	ELSE e.sal
       END
     )
     ```

### 8、事务机制

> sql在5.0版本引入了事务机制，这也是大版本号为5的sql最流行的原因

1. #### 避免写入直接操作数据文件

   - ###### 如果数据的写入直接操作数据文件是非常危险的事情，比如说中间可能会遇到系统崩溃，这样是我们不希望看到的。

2. #### 利用日志来实现间接写入

   - ###### sql一共有5种日志，其中只有redo和undo日志与事务有关。

     数据库 ---(拷贝数据)---> undo日志 ---(记录修改)---> redo日志 ---(同步数据)--> 数据库

     undo和redo完成后才会修改数据库，有哪个失败都会导致数据库不被修改，当SQL语句可以正常执行的时候才会进行重新执行SQL。

3. #### 事务机制(Transaction)

   - ###### REBMS = SQL语句 + 事物(ACID)

   - ###### 事务是一个或者多个SQL语句组成的整体，要么全部执行成功，要么全部执行失败。

     举例：我们可以接受用户注册成功，领取代金劵，或者用户未注册成功，无法领取代金券，但是我们无法接受用户注册未成功，却领取代金劵，或者用户注册成功，却无法领取代金券。

   - ###### 事务案例

     - 把10部门中的MANGER员工调往20部门，其他岗位的员工调往30部门，然后删除10部门

       在没有手动开启事务时，每执行一条语句之前，SQL会自动帮你开启事务，在执行语句之后，SQL会自动帮你提交事务。在这个案例中，由于先清空10部门，才能删除10部门，也就是先UPDATE再DELETE，所以我们需要将UPDATE和DELETE绑定在一个事务流程中，让他们共同成功或者共同失效，这样才能保证数据的安全。

       事务流程：开启事务->UPDATE语句->DELETE语句->提交事务

       ```sql
        #手动管理事务的语句如下
        START TRANSACTION;
        SQL语句
       [COMMIT|ROLLBACK];
       ```

       ```sql
       #上面的题
       START TRANSACTION;
       ...;
       ...;
       COMMIT; #提交事务
       ROLLBACK #回滚日志
       ```

4. #### 事务的ACID属性

   - ###### 原子性，一致性，隔离性，持久性
     
     - 原子性：一个事务的所有操作要么全部完成，要么全部失败，事务执行后，不允许停留在中间态。
     - 一致性：不管在任何给定的时间，并发事务有多少，事务必须保证运行结果的一致性。
       - 比如说，在同一时间，ABCD四个人互相转账，但是，就算转账之后，他们的账户总额应该保持不变。在他们转账的时候，读不到其他临时数据，也就保证了数据的安全。
     - 隔离性：隔离性要求事务不受其他并发事务的影响，如同在给定的时间内，该事务时数据库唯一运行的事务。自己的事务不能读取到其他事务的临时数据。
       - 实现原理：undo，redo文件虽然是通用的，但是其中的数据会被标记来自哪个事务，不会混淆。
     - 持久性：事务一旦提交，结果就是永久性的。即使发生了宕机，依旧可以依靠事务日志来完成事务。

5. #### 事务的并发

   > 数据库的事务是并发进行的。我们可以设置事务的隔离级别从而让其读一些临时数据。

   | 序号 | 隔离级别         | 功能           |
   | ---- | ---------------- | -------------- |
   | 1    | read uncommitted | 读取未提交数据 |
   | 2    | read committed   | 读取已提交数据 |
   | 3    | repeatable read  | 重复读取       |
   | 4    | serializable     | 序列化         |

   - ###### 事务案例1 READ UNCOMMITTED

     A事务和B事务买同一张票，由于B事务提交够快，所以B事务买到了票，当A事务提交的时候，发现票已经被卖完了，所以A事务要开始回滚。虽然这是可行的，但是动不动就购票失败是十分影响心情浪费时间的。所以，如果能让B事务读取A事务的临时数据，B事务读取到A事务的读取请求时，就不再进行提交。这个场景中，就应该使用READ UNCOMMITTED，读取其他事务为提交的数据。

     ```sql
     SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
     #这个语句并不是全局的，影响范围为当前会话。会话就是当前的console
     ```

   - ###### 事务案例2 READ COMMITTED

     两个人同时对同一个账户5000元进行操作，A向账户转入1000，B向账户转入100，由于事务并行的特性，而B比A提交或回滚地更快，如果A读取到了B的临时数据5100元，在B正常提交的情形下，那么账户余额是正常的。在B事务进行回滚的情形下，A事务读取到了B事务中的临时数据5100元，那么最终这个账户会变成6100元，但正确的情况应该是6000元，因为100元被回滚了。所以这个场景中，不能使用READ UNCOMMITTED隔离级别，而应该使用READ COMMITTED事务级别

     ```sql
     SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
     ```

     | 时刻 | 事务A                                           | 事务B                                           |
     | :--- | :---------------------------------------------- | :---------------------------------------------- |
     | 1    | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; | SET TRANSACTION ISOLATION LEVEL READ COMMITTED; |
     | 2    | BEGIN;                                          | BEGIN;                                          |
     | 3    |                                                 | SELECT * FROM students WHERE id = 1; -- Alice   |
     | 4    | UPDATE students SET name = 'Bob' WHERE id = 1;  |                                                 |
     | 5    | COMMIT;                                         |                                                 |
     | 6    |                                                 | SELECT * FROM students WHERE id = 1; -- Bob     |
     | 7    |                                                 | COMMIT;                                         |

   - ###### 业务案例3 REPEATABLE READ ->默认的隔离级别

     一分钟前，一个商品的价格为500元，一分钟后，一个商品的价格为600元。用户在价格为500元的时候下单购买商品，但是由于下单购买是一个有时长的动作，所以当真正付钱的时候，商品已经涨价为600元了。但是用户应该付款500元，这不需要过多解释。所以我们应该只让这个SQL过程读取500元的数据。这个过程应该使用REPEATABLE READ事务隔离级别，代表事务在执行中赶赴读取数据，得到的结果都是一致的，不管其他事务是否提交，不受其他事务的影响

     ```sql
     SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
     ```

     对于SELECT，第一次执行会调用数据库中的数据，哪怕你的隔离级别是REPEATABLE READ，这是因为REPEATABLE READ的原理是调取undo日志中的数据，但第一次执行SELECT的时候并没有undo日志，所以要生成undo日志，所有要调用数据库中的数据。所以第一次执行select的时候，是没有REPEATABLE READ的限制的，因为限制了也没用，SQL只知道去读取数据库中的数据生成undo。

   - ###### 事物的序列化 SERIALIZABLE ->很少使用

     由于事务并发执行带来的各种问题，前三种隔离级别只适合在某些业务场景中，但是序列化的隔离性，让事务按照顺序逐一执行，就不会产生上面的任何问题。

     ```sql
     SET SESSION TRANSACTION ISOLATION LEVEL SERIALIZABLE;
     ```

### 9、数据的持久化

1. #### 数据导出与备份的区别

   - ###### 数据导出，导出的纯粹是业务数据，不包含数据库的当前状态。

   - ###### 数据备份，备份的是数据文件，日志文件，索引文件等等。

     第一次sql进行全量备份，后面的备份都是增量备份

2. #### 数据导出与导入

   - ###### 数据导出有两种格式：SQL文档和文本文档。在数据不是很多的时候，建议导出为SQL文档

   - ###### mysqldump用来把业务数据导出为SQL文档，其中就包含了表结构

     ```sql
     mysqldump -uroot -p [no-data] 逻辑库 > 路径
     ```

   - ###### source命令用于导入SQL文件，包括创建数据表，写入记录等

     ```sql
     USE 逻辑库
     SOURCE backup.sql;
     ```

     
