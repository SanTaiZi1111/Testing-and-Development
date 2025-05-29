# 阶段二Linux与MySQL

> 目标：
>
> - 基础操作
>   - 使用ls、cd查看和切换目录
>   - 使用touch、mkdir创建文件和目录
> - 文件管理
>   - 使用rm、cp、mv进行删除、复制和移动操作
>   - 使用cat、more、grep查看文件内容
> - 高级功能
>   - 掌握管道符|实现内容分屏和过滤
>   - 使用重定向符号(>, >>)输出内容到文件
>
> 掌握查询命令的方法

## 一、使用finalshell远程连接Linux虚拟机系统

IP获取：ifconfig

连通性测试：ping

检查SSH端口状态：netstat - anpt|grepssh

Linux虚拟机已配置完整

需要条件：IP地址、端口号、用户名和密码

> 工具：
>
> FinalShell免费跨平台
>
> Xshell收费仅windows

## 二、Linux文件系统和目录

1.Linux文件系统结构：**树状结构**

2.关键目录：

/：根目录

/root：管理员root目录

/home：普通用户目录

/boot：内核文件目录，与系统启动加载相关

/etc：系统配置相关（系统、网络管理、维护相关文件）

/usr：多用户，存放大多数系统应用程序

/tmp：临时文件夹

 /bin存放系统工具

## 三、Linux基础命令

### 1.命令格式：

`命令名称[选项参数][操作对象]`

> 选项部分以-开头，用于调整命令行为
>
> 参数部分：命令操作对象，可接受多个参数
>
> 各部分用空格分隔

### 2.ls

语法：`ls[选项][文件或目录]`

常用参数：`ls -la`

- -l: 以列表方式**详细**显示内容信息
- -a: 显示所有文件，包含**隐藏文件**及目录
- -h: 配合-l显示出文件的大小（自动转换**单位**）

> （查看隐藏文件）列出该路径下所用文件和目录：ls -a /路径

通配符：`ls [a-z].txt`          `ls /usr/bin/*.sh`该目录sh后缀所有文件

- *: 表示0到多个任意字符
- ?: 表示匹配一个任意字符
- [abcd]：匹配a、b、c、d中的**任意**一个字符
- [a-f]：匹配从a到f范围内的**任意**一个字符

> ~对应当前用户家目录
>
> 命令区分大小写

### 2.cd

语法：cd [目录路径]

**相对路径**：不以（ / ）或家目录开头的路径，相对的不完整的路径

特殊用法：

- cd ..：切换到上一级目录
- cd -：切换上一次所在目录

pwd查看当前位置：`pwd`             //输出路径

> 路径中的空格需要用引号或转义字符
>
> $root$ 家目录

### 3.创建

语法：touch 文件名1 文件名2...         

>  创建多个文件
>
> 文件已存在会更新创建时间

语法：mkdir 文件夹

> 创建文件夹（目录）
>
> 递归创建-p：mkdir -p a/b/c  创建a目录下的b目录，b目录中的c目录

### 4.删除

语法：`rm [选项参数] 目录名或文件名`

选项参数：

- -i：交互式执行，删除之前会询问
- -f：强制删除
- -r：递归删除目录及其所有内容

实例：

`rm -i *.txt`    

> 删除所有文件，删除之前询问

`rm -rf test*`

> 删除所有以test开头的文件

### 5.复制

语法：`cp 源文件 目标文件`

常用选项：

- -f：强制覆盖已存在的目标文件而不提示
- -r：源文件为目录，必须用此参数才能全删除
- -v：显示复制过程进度

### 6.移动

语法：`mv 源文件 目标文件`

常用选项：

- -f：强制覆盖已存在的目标文件不提示
- -i：覆盖时会提示
- -v：显式移动进度

### 7.查看文件

(1).cat 文件

> 查看较少内容的文件而非文件夹
>
> cat -n /etc/hosts
>
> -n：带行号查看

(2).more 文件名

> 分页查看大文件
>
> f：下一页
>
> b：上一页
>
> q：退出
>
> enter：一行一行看

(3).grep 文件名

语法：grep [选项] 查找内容 文件名

> 文本搜索，支持正则表达式
>
> 查找该txt中包含"mike"的行：`grep mike xxx.txt`
>
> 忽略大小写查找mike：grep -i mike xxx.txt

常用选项：

-v：显示不包含匹配文本的所有行

-n：显示匹配行及其行号

-i：忽略大小写差异

常用正则表达式：`grep -n '^a' hh.txt`

- 基础语法：
  - -ni组合：同时显示行号并忽略大小写
  - -nv组合：显示不匹配的行及行号
- 特殊符号：
  - ^a：匹配以a开头的行
  - ke$：匹配行尾为ke
  - [Ss]igna[Ll]：匹配[]中任意一个字符，搜寻signal的单词匹配的行

### 8.重定向(复制、写入)

`>`：输出到新文件中，会覆盖原有内容

`>>`：追加到新文件中，不会覆盖原内容

示例：`ls -l > aa.txt`

`ls / > mike.txt`    将根目录列表写入mike文件

### 9.管道符   |

概念：LINUX允许将一个命令的输出结果通过管道作为另一个命令的输入，使用竖杠符号”|“表示

语义： |  左边输入内容，右边输出内容

常用的管道输出命令：

- more：分屏显示执行结果
- grep：通过grep过滤执行结果的内容

> ls -l /usr/bin | more    将目录内容用more分页显示
>
> ls /usr/bin | grep '^au'    只显示以au开头的文件名
>
> ls -la /usr/bin | grep .zip$    显示路径下以zip结尾的文件**列表**
>
> 管道可以连续使用，不改动原属程序

### 10.其他命令

pwd：查看当前目录路径

clear：清屏

which：工具名称，查找工具安装位置

## 四、Linux高级命令

### 1.进程与端口相关命令

（1）重启和关机

重启：**reboot**

关机：**shutdown** -h now    零延迟关机

重启：shutdown -r now

定时关机：shutdown -h 20:25        shutdown -h +10  十分钟后

（2）查看系统进程信息

查看进程状态：ps

> 命令格式:   ps -aux  | grep  '关键字'
>
> 选项说明:
>
> - -a 选项: 显示所有用户的进程
>
> - -u选项：显示进程的详细状态
>
> - -x选项：显示没有控制终端进程

终止进程：kill

> 命令格式:  kill -9  进程ID    
>
> - 进程id一般会通过ps命令去查看到。
>
> - -9  表示的是强制的关闭对应的进程。

实时监控进程：top

> 快捷键:  
>
> - M(按shift+m)  可以按内存的使用率降序排列显示进程的信息
> - P(按shift+p)  可以按cpu的使用率降序排列显示进程的信息

**查看系统监听端口**：**netstat**

> 例：查看当前系统中是否已打开3306的端口：  netstat -anptu | grep '3306'
>
> 查看当前系统中是否已打开http的服务：   netstat  -aptu | grep 'http'

> - 命令格式：netstat [-anptu]  |grep '关键字'     （root用户操作）
>
> - 选项说明:
>
>   - -a  选项:  查看所有已打开的端口
>
>   - -n  选项:  以数字方式显示已打开的端口，不显示别名
>
>     （http:80   https:443  mysql:3306   ssh:22  ）
>
>   - -p  选项：显示对应的进程的PID
>
>   - -t 选项：  显示出tcp协议的端口
>
>   - -u 选项：  显示出udp协议的端口

（3）查看日志信息

作用：

​	日志文件记录程序运行信息，通过最后几行信息，看程序的运行状态，通过该状态定位程序问题。

语法：

head 文件名

> 默认显示日志文件前10行
>
> head -20 文件名             显示前20行

tail 文件名

> 默认显示日志文件最后10行
>
> tail -15 文件名            显示日志文件最后15行
>
> tail -f 文件名              实时显示日志文件的信息

（4）用户权限

命令格式：chmod 755 文件名

命令说明：7是文件拥有者的      5用户组权限     5其他用户权限

![image-20250529211315232](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20250529211315232.png)

例：chmod 467 1.txt    读/读写/读写执行

（5）切换用户、修改密码、退出

切换用户：su 用户名   

>  例：su -  到root    

修改密码：passwd 用户名

退出登录用户：exit

> * 如果是图形界面，退出当前终端。
> * 如果是使用ssh远程登录，退出登陆账户。
> * 如果是切换后的登陆用户，退出则返回上一个登陆账号。

### 2.文件操作命令

（1）查找文件： 

命令格式：find   [路径]  -name  文件名

> 例子:    find  .  -name  'abc*.txt'   在当前目录及子目录下查找名称为abc开头的txt文件

（2）软链接

命令格式：ln -s 源文件 链接文件

> 说明:
>
> * 源文件建议使用绝对路径
> * 不加  -s  参数表示的是硬链接

（3）压缩解压缩

gzip：

​	压缩：tar -zcvf 压缩文件名.tar.gz  被压缩的文件或目录

​	解压缩：tar -zxvf 压缩文件名

> tar -zxvf  acd.tar.gz  -C   abc    ：表示将acd.tar.gz文件中的内容解压缩到当前目录的abc文件夹下

zip：

​	压缩：zip [-r] 压缩文件名 文件或目录

​	解压缩：unzip -d 解压目录 压缩文件名

> zip   -r   acd _test   acd   ：表示将当前目录下面的acd目录压缩到 acd_test.zip文件当中。
>
> unzip -d  /home/admin1   acd_test.zip    ：表示将acd_test.zip文件中的内容解压缩到 /home/admin1目录下面

### 3.命令行编辑器vi

（1）模式切换：

命令行模式：Esc     冒号

插入模式：i  

末行模式：主要是用来保存文件或者退出文件

> - w:  表示保存文件并回到命令行模式
>
> - q:  表示的是退出vi编辑器
>
> - ！：表示的是强制
>
> - wq！：表示的是强制保存并退出vi编辑器 

（2）基本操作：

复制所在行：yy

黏贴：p

批量删除：起始行号,结束行号d

搜索：/关键词

> /为向下搜索
>
> n：继续搜索下一个匹配项
>
> N：反向搜索上一个匹配项

## 五、Linux命令的帮助信息查看

该命令简洁的使用说明：command --help

完整文档man手册：man command

- 空格键：翻页查看
- Enter：逐行滚动
- B:显示上一页
- F：显示下一页
- Q：退出man显示
- /关键词：搜索内容

清屏：clear   /   ctrl+L

## 六、数据库基础

### 1.数据库介绍与分类

关系型数据库：以二维表格形式组织数据，通过行列结构表达数据间关系

> Mysql  SQLserver  SQLite轻量型  使用技术术语：RDBMS

非关系型数据库：采用键值对方式存储数据

> Redis高速缓存    MongoDB文档型   HBase大数据

### 2.关系型数据库核心要素和SQL分类

> 列-->字段
>
> 行-->记录

SQL语言：

- 结构化查询语言
- 主要分类
  - DQL（数据查询语言）：如SELECT
  - DML（数据操作语言）：如INSERT/UPDATE/DELETE
  - TPL（事务处理语言）：如BEGIN TRANSACTION
  - DCL（数据控制语言）：如GRANT/REVOKE
  - DDL（数据定义语言）：如CREATE/DROP
  - CCL（指针控制语言）：如DECLARE CURSOR

### 3.MySQL

核心组件：

MySQL服务器：存储数据并解析编译后的SQL语句，将执行结果发给客户端

MySQL客户端：下发给用户要执行的SQL语句，并显示服务器返回的执行结果

### 4.连接数据库（用DBeaver）

命令行：`mysql -h host -u 用户名 -p 密码`

连接四要素：IP    端口号(默认3306)   mysql用户名    mysql密码

常见错误：

- 服务器未启动导致连接超时

- IP地址变更未更新

- 端口号错误或者被安全墙拦截

使用DBeaver核心要点：

- 创建数据库主动选择utf8编码
- 排序规则：utf8编码必须搭配utf8_general_ci
- 遇见错误优先选择删除而非修改

### 5.数据说明

（1）数据类型

整形int

小数decimal        [格式：decimal(5,2) 表示总位数5，小数位2]、

字符串vachar      [格式：vacher(3) 最多三个字符，需要单/双引号包裹] 

时间日期datetime  []

(2)数据约束

定义：对数据库中数据进行限制，以确保数据正确性、有效性、一致性

主要类型：

- 主键
  - 决定排列顺序
  - 唯一标识：一个表只能有一个主键字段
  - 主键必须非空
- 默认值
- 非空
- 唯一
  - 确保字段值不许重复
- 外键
  - 维护两个表之间的关联关系
  - 从表的外键字段引用主表的主键字段

## 七、命令行操作及表操作

### 1.命令行操作数据库

查看所有数据库：show databases;

使用数据库：use 数据库名;

查看当前数据库：select database(当前使用数据库名);

创建数据库：create database 数据库名 charset=utf8;

删除数据库：drop database 数据库名;

ctrl+d退出MySQL

#### 使用DBeaver编辑区：

![image-20250525110036719](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20250525110036719.png)

运行的快捷键：ctrl+enter    （运行时鼠标放在语句位置）

![image-20250525113914316](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20250525113914316.png)

![image-20250526152517061](C:\Users\a\AppData\Roaming\Typora\typora-user-images\image-20250526152517061.png)

### 2.数据库表操作

（1）操作前提：

use 数据库;

查看所有表：show tables;

查看表结构：desc 表名;

查看创建该表的完整语句：show create table 表名;

删除表：DROP TABLE 表名;             *drop table if exists表名*

（2）表结构：

字段类型：varchar(100)表示可变长度字符串

字段属性：

- default null表示字段允许为空且默认为null
- YES表示子段允许为空
- 主键字段会显示PRI标识

存储引擎：engine=InnoDB表示使用InnoDB存储引擎

字符集：DEFAULT CHARSET=utf8

（3）表操作语句

创建数据表：

`create table 表名(`

​	`字段1 类型 可选的约束,`

​	id int unsigned primary key auto_increment,

​	height decimal(5,2)

`);`

> id字段：无符号整数主键，自动增长
>
> height字段：总长度5位，2位小数

插入数据：

insert into 表名 values(0,'亚瑟',22,185)

insert into 表名 (字段1,字段2,...) values(值1,值2,...)

简单查询：

select * from 表名;   //查询表中所有数据

> 常见错误：
>
> 表结构修改不生效必须删除表重建
>
> 数据类型不匹配
>

## 八、SQL语句

```yacas
1. 能够通过select语句查询表的所有信息及部分字段
2. 能够通过条件判断（比较运算、逻辑运算、范围查询、模糊查询）查询表中指定数据
3. 能够通过order by关键字进行排序
4. 掌握聚合函数的用法
5. 能够通过group by关键字进行分组操作
6. 能够通过having关键字对分组后的数据进行筛选
```

### 1.增删改查基础

> 排序
>
> 分组和聚合

（1）批量插入：

**insert** **into** students **values**(0,'mike江',18,177.88);

**insert** **into** students **values**(0,'LL',22,165),(0,'春川',17,180.8);

（2）修改：

**update 表名 set 字段名1=值1,字段名2=值2... where 条件**

没有条件限制会全改

> 修改id为3的学生数据，姓名改为狄仁杰，年龄改为18   ：
>
> **update** students **set** name='狄仁杰',age=18 **where** id=3;

（3）删除

物理删除：delete from 表名 where 条件;

逻辑删除：通过设定一个字段标识记录已删除（压根没删）

> delete删除部分，数据不会重置自增长
>
> truncate清空整个表数据，从1开始计数     truncate table students
>
> drop删表

（4）数据查询操作

全查：

**select** * **from** students;

查部分：

**select** name,age **from** students;

起别名：

> select 别名.字段名 from 表名 AS 别名

**select** *stu*.name **from** students **AS** *stu*

**select** name **as** 姓名,age **as** 年龄 **from** students;

去重：

select distinct 字段 from 表名

### 2.条件查询（where）

（1）比较运算符

​	年龄小于二十：**select** * **from** students **where** age < 20        

​	不在北京：**select** * **from** students **where** hometown !='北京';      

（2）逻辑运算符（写在where之后）

​	AND同时满足多个条件：**select** * **from** students **where** age < 20 **and** sex='女';

​	OR满足任一条件：

​	NOT用于取反：**select** * **from** students **where** **not** hometown='天津'

（3）模糊查询：

关键字：like

%：匹配任意多个字符

_：匹配一个任意字符

​	查找姓孙的学生：**select** * **from** students **where** name **like** '孙%'

​	查找名字以‘乐’结尾的：**select** * **from** students **where** name **like** '%乐'

​	查询姓名为两个字的学生：**select** * **from** students **where** name **like** '__';

（4）范围查询：

in：查询不连续的离散值

​	**select** * **from** students **where** hometown **in** ('北京','上海');

​	**select** * **from** students **where** age **in**('18','25') **and** sex='女';

between ... and ...：查询数值或日期的连续区间（左右边界都包括）

​	**select** * **from** students **where** age **between** 18 **and** 20;

（5）空判断

> null与‘ ’  (空字符串)不一样，置为空和没填写

select * from students where ID is NULL;

select * from students where ID='';

### 3.排序与聚合（order by 字段）

（1）排序：

语法：select * from 表名 order by 字段名1 asc/desc, 字段名2 asc/desc,...

​	**select** * **from** students **where** sex='女' **order** **by** age;

说明：将行数据按照字段1排序，若值相同依次递推字段2

- asc小到大升序(默认升序)
- desc降序

（2）聚合函数：

规定：聚合函数方便数据统计，聚合函数**不能作为where**条件

常见类型：

- count() 查询总记录数
- max(字段名)查询最大值  min(字段名)
- sum(字段名)求和
- avg(字段名)求平均数

女生最大年龄为多少：**select** **max**(age) **from** students **where** sex='女';

一班有多少个学生：**select** **count**(*) **from** students **where** class='1班';

### 4.分组（group by）分页（limit）查询

（1）分组查询：

目的：对每一组数据进行统计（使用聚合函数）

语法：`select 字段名1,字段名2，聚合函数.... from 表名  group by 字段名1，字段名2....`

例：

- 查询各种性别的**人数**:`select sex,count(*) from students group by sex;`

- 查询各种性别**最大的**年龄:`select sex,max(age) from students group by sex;`

分组后的**数据筛选**：having

`select 字段名1,字段名2，聚合函数.... from 表名  group by 字段名1，字段名2....  having 条件`

> having与where其实是一样的。但是在having后面可以使用聚合函数

例：

- 查询男生总人数：`select sex,count(*) from students group by sex having sex='男';`
- 查询该班男生总人数：`select class,sex,count(*) from students group by class,sex having sex='男';`

（2）分页查询：

语法格式：select * from 表名 limit start,count

> start表示的是开始的记录，**索引是从0开始**，0表示第一条记录
>
> count表示的是从start开始，查询**多少条**记录

例：

查询前三行学生信息：`select * from students limit 0,3;`

### 5.内连接

（1）概述：

内连接（inner join）：连接两个表，取得是两个表中都存在的数据

**左**连接（left join）：连接两个表，取得是**左表中特有**的数据，对于右表中不存在的数据用null填充

右连接（right join）：连接两个表，取得是右表中特有的数据，对于左表中不存在的数据用null填充

（2）内连接：

语法：`select * from  表名1 inner join 表名2 on 表1.列=表2.列;`

> 查询的是两个表的交集数据
>
> 表一的列与表二的列一定存在关联关系，在on后连接字段
>
> 内连接连接时可以连接多个表
>
> 起别名记得用as

例：

查询学生信息和学生成绩：

`select * from students inner join scores on students.studentNo = scores.studentNo;`

查询王昭君的成绩，要求显示姓名、课程号、成绩

**select** *stu*.name,*sc*.courseNo,*sc*.score **from** students *stu* **inner** **join** scores *sc* **on** *stu*.studentNo=*sc*.studentNo **where** *stu*.name='王昭君';

（3）左连接

语法：`select * from 表1 left join  表2 on 表1.列=表2.列`

> 左连接查询的是左表特有的数据，对于右表中不存在的数据用null来填充

例：查询所有学生成绩包括没有成绩的

`select * from students stu left join scores sc on stu.studentNo=sc.studentNo;`

### 6.子查询

嵌入在其他语句中的select语句，子查询辅助主查询要么充当条件，要么充当数据源。子查询是一条完成的可执行select语句

## 九、实战项目部署

lnmp（linux+nginx+mysql+php7.0）项目

Tpshop项目压缩包放在指定目录下

nginx配置文件需修正监听端口避免冲突

> 注意解决SElinux问题：sudo setsebool -P httpd_can_network_connect 1
>
> 端口都要开放：8080  80  
>
> 检查SElinux状态：sudo getenforce

# ！商城项目测试主体部分在下一阶段！
