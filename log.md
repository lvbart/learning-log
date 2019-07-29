Learning log:
开始使用VS code：并安排设置和snippets
自动补全 

2019/03/12  connect mysql to real world data
尝试将tsv文件导入mysql
第一次尝试：使用phpmyadmin-导入-格式（open document spreadsheet）失败
第二次尝试：在数据库中先建立带有一致表头的表格，再导入
什么是TSV文件？使用TAB键分隔的文件
网上都是CSV文件居多，找到在线将TSV转CSV文件，导出和导入都是乱码
估计由于无法显示中文，缩减表格列，仅保留期间、编码、rmb借方、rmb贷方、记账货币借贷方（local currency）
再次导入 提示#1366 - Incorrect integer value: 'month' for column 'month' at row 1
删除表头 再次导入 提示#1265 - Data truncated for column 'code' at row 1被截断
修改”code”为VARCHAR 再次导入 提示#1366 - Incorrect integer value: '' for column 'cr_rmb' at row 1
修改”dr/cr”从int变为decimal 再次导入 #1366 - Incorrect decimal value: '' for column 'cr_rmb' at row 1
修改数字的空 为 是 再次导入 #1366 - Incorrect decimal value: '' for column 'cr_rmb' at row 1
剔除借贷的概念，统一为借方减贷方 缩减两列 再次导入 导入成功！

尝试进行分析
SELECT * FROM data_rent where 成功
使用putty操作链接mysql 成功


尝试通过node 及 javascript 与mysql连接
App.js无法连接，已提交工单
晚上回家问题解决，即使在本地没有安装mysql也可以，使用腾讯云主机及数据库即可连接，
const mysql   = require('mysql');
const express = require('express');

const db = mysql.createConnection({
  host     : '172.21.0.8',
  user     : 'root',
  password : '****',
  database : 'work'
});

const app = express();

 
db.connect((err) => {
  if(err) {
    throw err;
  }
  console.log('Mysql connected...');
})

app.listen('3000', () => {
  console.log('server started on port 3000');
});


	2019/03/13 使用nodejs 对数据库CRUD操作
	在文件夹内npm init，设置初始
{
  "name": "nodemysql",
  "version": "1.0.0",
  "description": "use node to connect mysql",
  "main": "app.js",
  "dependencies": {
    "express": "^4.16.4",
    "mysql": "^2.16.0"
  },
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Lv",
  "license": "MIT"
}

	尝试用nodejs建立数据库
app.get('/createdb', (req, res) => {
  let sql = 'CREATE DATABASE nodemysql';
  db.query(sql, (err, result) => {
    if(err) throw err;
    console.log(result);
    res.send('Database created...')
  });
});
建立好之后因为需要在同一台电脑打开网页发出req，centos不会打开网页，所以安装图形界面
# yum grouplist
# yum groupinstall "X Window System" "KDE Desktop" Desktop
既然是桌面环境，可能还需要诸如字体、管理工具之类的，如，
# yum -y groupinstall "Graphical Administration Tools"
# yum -y groupinstall "Internet Browser"
# yum -y groupinstall "General Purpose Desktop"
# yum -y groupinstall "Office Suite and Productivity"
# yum -y groupinstall "Graphics Creation Tools"
3. 启用
从命令行直接启动图形桌面环境，
# startx
无法启用 暂时放弃


	下载postman试图发送get命令
Could not get any response
There was an error connecting to localhost/createdb.

	03/14
	更新data collection平台录入页面并上传至桶
测试用户反馈：看不见/点add entry直接跳回首页
修改：配色
修改：理清js文件的动作，检查与IE兼容性，共有读私有写应变更？
修改：全部显示中文
	注册阿里云，领取免费主机和数据库
	03/15 通过GET与数据库交互/学习测试API的办法/ 
	下载mongodb server,单位电脑为32位无法安装，
尝试在腾讯云主机安装

	为使用GET与数据库交互，建立简单数据样例

	03/18 设置快报展示模板
	设计（腾讯云）mysql数据表
建立数据库“quickbook”， 
建立数据表”AKL”，表头: 

	ITEMS	YEARMONTH	QUICKBOOK	ACTRUAL
尝试导入数据AKL1901快报与实际
#1366 - Incorrect string value: '\xA1\xA2\xCD\xCB\xCB\xB0' for column 'ITEMS' at row 1
修改ITEMS名称，全部整理为四位代码，继续导入
#1292 - Incorrect datetime value: '1901' for column 'YEARMONTH' at row 1
修改YEARMONTH为PERIOD.且格式为VARCHAR
成功，但快报和实际数据为整数，需要调整为保留两位小数（65，2）
基本满足需求，继续导入ATH数据


	03/19 什么都没做
 

	03/20 学习API/Vue.js
	Client ID 2a57ed019b7348a49315bfab29126836
Spotify
没有测试成功，
设置了环境及密钥，没有成功

	Vue
Import
Create app
El/data
Templates: show dynamic data {{ XXX }}
Directives: v-if/v-for/v-model/v-on:click
v-blind
computed
get/set


	03/21 Vue学习
 Vue-cli安装失败

 了解events
Resource events
Network events
Focus events
WebSocket events

 API终于收到第一个成功的请求，
Bing字典，只需录入要查询的中文即可
http://xtk.azurewebsites.net/BingDictService.aspx?Word=singing


	03/26 Vue学习
 cmd下：Vue-cli:安装失败
 node cmd下：Vue-cli:安装失败
原因是在单位上网需要设置代理，因此npm下同样需要设置
找到网络连接设置，10.10.101.9:8080
node cmd：
npm config set proxy http://10.10.101.9:8080
npm config set https-proxy http://10.10.101.9:8080
安装成功！

 codeacmedy-Vue学习完成初步
 
 计划更新录入页面，连接数据库
架构大致：
数据库（mysql）
+ express(nodejs)作为服务端写api(其实就是向数据库拿数据，推荐可以用TypeScript写) 
+ vue 前端(SPA or MPA) 请求api


 Vue安装完毕，跟着官网教程创建新项目


 是否需要安装vue-devtools？


	03/27 Linux学习 安装桌面系统
 Linux文件系统及各部分作用
Linux 文件系统是一个目录树的结构，文件系统结构从一个根目录开始，根目录下可以有任意多个文件和子目录，子目录中又可以有任意多个文件和子目录
•	bin 存放二进制可执行文件(ls,cat,mkdir等)
•	boot 存放用于系统引导时使用的各种文件
•	dev 用于存放设备文件
•	etc 存放系统配置文件
•	home 存放所有用户文件的根目录
•	lib 存放跟文件系统中的程序运行所需要的共享库及内核模块
•	mnt 系统管理员安装临时文件系统的安装点
•	opt 额外安装的可选应用程序包所放置的位置
•	proc 虚拟文件系统，存放当前内存的映射
•	root 超级用户目录
•	sbin 存放二进制可执行文件，只有root才能访问
•	tmp 用于存放各种临时文件
•	usr 用于存放系统应用程序，比较重要的目录/usr/local 本地管理员软件安装目录
•	var 用于存放运行时需要改变数据的文件

2.4通配符
学过一些正则表达式的或者有点基础的同学对通配符应该就不陌生的了，在Linux也有通配符(在搜索的时候挺有用的)
•	*：匹配任何字符和任何数目的字符
•	?：匹配单一数目的任何字符
•	[ ]：匹配[ ]之内的任意一个字符
•	[! ]：匹配除了[! ]之外的任意一个字符，!表示非的意思
绝对路径：
•	以斜线（/）开头 ，描述到文件位置的完整说明 ，任何时候你想指定文件名的时候都可以使用
相对路径 ：
•	不以斜线（/）开头 ，指定相对于你的当前工作目录而言的位置 ，可以被用作指定文件名的简捷方式
•	•  ls：显示文件或目录信息
•	•  mkdir：当前目录下创建一个空目录
•	•  rmdir：要求目录为空
•	•  touch：生成一个空文件或更改文件的时间
•	•  cp：复制文件或目录
•	•  mv：移动文件或目录、文件或目录改名
•	•  rm：删除文件或目录
•	•  ln：建立链接文件
•	•  find：查找文件
•	•  file/stat：查看文件类型或文件属性信息
•	•  cat：查看文本文件内容
•	•  more：可以分页看
•	•  less：不仅可以分页，还可以方便地搜索，回翻等操作
•	•  tail -10： 查看文件的尾部的10行
•	•  head -20：查看文件的头部20行
•	•  echo：把内容重定向到指定的文件中 ，有则打开，无则创建
•	•  管道命令 | ：将前面的结果给后面的命令，例如：ls -la | wc，将ls的结果加油wc命令来统计字数
•	•  重定向 > 是覆盖模式，>> 是追加模式，例如：echo "Java3y,zhen de hen xihuan ni" > qingshu.txt把左边的输出放到右边的文件里去
•	

如何退出vim编辑器？
Shift+zz

安装Linux的桌面系统后，
设置开机自启：失败：[root@VM_0_5_centos ~]# gnome-session-properties

** (gnome-session-properties:6251): WARNING **: 13:21:25.814: Unable to start: Cannot open display:


	03/28 Linux学习 安装桌面系统
 重装了腾讯云实例
 API学习：使用API
qq音乐或者Spotify


	03/29 API学习 Mysql学习
 重装了腾讯云实例后，为了更方便查看API，HTTP request结果，安装GUI
在单位电脑无法远程连接

 重新安装npm, nodejs,yarn, mysql, vue-cli, vue, vuex, vue-resource, express, body-parser, axios, 
更新 openssl, 
[root@VM_0_5_centos tmp]# node -v
v10.15.3
[root@VM_0_5_centos tmp]# npm -v
6.4.1
[root@VM_0_5_centos tmp]#
 用vue-cli脚手架工具创建一个基于webpack的Vue项目
[root@VM_0_5_centos mysql-vue-test]# vue init webpack vue-project

/bin/sh: git: 未找到命令
? Project name vue-project
? Project description a vue mysql test project
? Author Lv
? Vue build standalone
? Install vue-router? Yes
? Use ESLint to lint your code? Yes
? Pick an ESLint preset Standard
? Set up unit tests Yes
? Pick a test runner jest
? Setup e2e tests with Nightwatch? Yes
? Should we run `npm install` for you after the project has been created? (recommended) npm

   vue-cli · Generated "vue-project".



	04/01 Python3学习
 Python3学习，数字及字符
Codecademy和书籍《python编程-入门到实践》
1 变量命名规则：不能以数字开头/不能含空格/不能使用关键字和函数/应简短并有描述性/慎用小写字母l和大写字母O，容易与数字混淆
2 字符串blocks of text：转换大小写【.title()/.upper()/.lower()】、拼接【+】、添加空白【制表符\t、换行符\n】、删除空白【.rstrip()、.lstrip()】、注意单双引号
3 数字
3.1 整数 浮点数 

错误 SyntaxError 和NameError
评论 #
打印 print(“”) tell a computer to talk.


 描绘三个想实现的程序
1 商务日常英文短句生成器
2 多用户在线填写表格 
3 收集并展示数据 脱离office的报表及展示



	04/02 Python3 学习
 Python3学习，列表，字典，if语句，while语句





 安装最新版Django==2.2：cmd,pip
-检查是否成功安装：
$ python
Python 3.6.4 (v3.6.4:d48eceb, Dec 19 2017, 06:04:45) [MSC v.1900 32 bit (Intel)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import django
>>> print(django.get_version())
2.2
-creating a project
A project is a collection of configuration and apps for a particular website
-creating an app

Database set up
Creating models
A model is the single, definitive source of truth about your data. 
It contains the essential fields and behaviours of the data you’re storing.

Activating models
three-step guide to making model changes:
•	Change your models (in models.py).
•	Run python manage.py makemigrations to create migrations for those changes
•	Run python manage.py migrate to apply those changes to the database.
Playing with the API
Introducing the Django Admin
Generating admin sites for your staff or clients to add, change, and delete content is tedious work that doesn’t require much creativity. For that reason, Django entirely automates creation of admin interfaces for models.
The admin isn’t intended to be used by site visitors. It’s for site managers.

Creating an admin user
Start runserver

Make the poll app modifiable in the admin


	04/03 Python3 学习
 Python3学习，list, function
3.1 访问列表元素：print(list_sample[0].title())
3.2 修改、添加和删除元素：
3.2.1 修改：=
3.2.2 添加：末尾添加.append()、位置添加.insert(位置, )
3.2.3 删除：知道位置用del 、.pop()


	04/04 Python3 学习
 Python3学习，
复习各种符号：
-关键字
-and/or/not: 
对python而言
其一, 在不加括号时候, and优先级大于or
其二, x or y 的值只可能是x或y. x为真就是x, x为假就是y
其三, x and y 的值只可能是x或y. x为真就是y, x为假就是x
-删除
del : know the location of item in your list
del list_sample[0]

-while/if/elif/else/for/in

If
Else
Where
For item in items:

for iterating_var in sequence: 
for iterating_var in sequence: 
statements(s) 
statements(s)

while expression: 
while expression: 
statement(s) 
statement(s)

-break/continue/pass
Python break语句，就像在C语言中，打破了最小封闭for或while循环。
break语句用来终止循环语句，即循环条件没有False条件或者序列还没被完全递归完，也会停止执行循环语句。
break语句用在while和for循环中。
如果您使用嵌套循环，break语句将停止执行最深层的循环，并开始执行下一行代码。
Python pass 是空语句，是为了保持程序结构的完整性。
pass 不做任何事情，一般用做占位语句。

Python continue 语句跳出本次循环，而break跳出整个循环。
continue 语句用来告诉Python跳过当前循环的剩余语句，然后继续进行下一轮循环。
continue语句用在while和for循环中。


-try/except/raise



-yield:
yield 是一个类似 return 的关键字，只是这个函数返回的是个生成器
>>> def createGenerator() :
...    mylist = range(3)
...    for i in mylist :
...        yield i*i
...
>>> mygenerator = createGenerator() # create a generator
>>> print(mygenerator) # mygenerator is an object!
<generator object createGenerator at 0xb7555c34>
>>> for i in mygenerator:
...     print(i)
0
1
4
这个例子没什么用途，但是它让你知道，这个函数会返回一大批你只需要读一次的值.
为了精通 yield ,你必须要理解：当你调用这个函数的时候，函数内部的代码并不立马执行 ，这个函数只是返回一个生成器对象，这有点蹊跷不是吗。
那么，函数内的代码什么时候执行呢？当你使用for进行迭代的时候.


-引入
Import
From
import xx导入模块对于模块中的函数，每次调用需要“模块.函数”来用。
from xx import fun 直接导入模块中某函数，直接fun()就可用


-数据类型



-字符串转义序列


-字符串格式化


-操作符


flow



 Django学习，view


Creating the public face: views


 将ORACLE综合账务查询导出数据直接导入MYSQL
因为数据为tsv格式，难以兼容，建议在excel中打开时进行必要转换，变为csv这种接受度更高的数据格式再导入。

	04/11 Python3 学习
 Python3学习，CONTROL FLOW


	04/15 Python3 学习
 Python3学习，LIST
List-zip
.append(): Add a single element, new element is added to the end of the list
Range（10）
range(2, 9, 2)


	04/17 Python3 学习
 Python3学习，LIST

In Python, we call the order of an element in a list its index.
Creating a selection from a list is called slicing
We can use negative indexes to count backward from the last element.
sortedis different from .sort() in several ways:
1.	It comes before a list, instead of after.
2.	It generates a new list.


	04/18 Python3 学习
 Python3学习，LIST
Count()
LEN()
	04/19 Python3 学习
 Python3学习，LOOP
•	Loops that let us move through each item in a list, called for loops
•	Loops that keep going until we tell them to stop, called while loops
•	Loops that create new lists, called list comprehensions

 Django学习:在Linux上安装Django

	04/22 Python3 学习
 Python3学习，LOOP
•	A loop that never terminates is called an infinite loop. These are very dangerous for your code!
•	if you accidentally stumble into one, you can end the loop by using control+ c to terminate the program.
•	Continue: set up filter rules 
•	List comprehension
•	usernames = [word for word in words if word[0] == '@']
This list comprehension:
•	Takes an element in words
•	Assigns that element to a variable called word
•	Checks if word[0] == '@', and if so, it adds word to the new list, usernames. If not, nothing happens.
•	Repeats steps 1-3 for all of the strings in words
cubes = [cube**3 for cube in single_digits]

<automate the boring stuff in Python>
Open new repository in github
Read the files:
>>import os
>>os.getcwd()
>>os.makedirs(‘C:\\test\\makenew’)
Read in 3 steps:
1.	open()
2.	read() or write()
3.	close()


	04/23 Python3 学习
 Python3学习，LOOP
My first def independently 
def over_nine_thousand(lst):
  lst_sum = 0
  for num in lst:
    lst_sum += num
    if lst_sum > 9000:
      break
  return lst_sum
    

	04/24 Python3 学习
 Python3学习，STRING
	Escape Characters
DEF COMMON_LETTERS(STRING_ONE, STRING_TWO):
  COMMON = []
  FOR LETTER IN STRING_ONE:
    IF (LETTER IN STRING_TWO) AND NOT (LETTER IN COMMON):
      COMMON.APPEND(LETTER)
  RETURN COMMON
include `.upper()`, `.lower()`, `.title()`, `.split()`, `.join()`, and `.format()`.


	04/25 Python3 学习
 Python3学习，STRING
author_last_names = []
for name in author_names:
  author_last_names.append(name.split()[-1])
this is awesome!
.join() is essentially the opposite of .split()
•	\n Newline
•	\t Horizontal Tab

.strip()
Stripping a string removes all whitespace characters from the beginning and end. 
.replace()
.find()
.format()
 why is this method better? The answer is legibility and reusability
It is much easier to picture the end result 


	04/26 Python3 学习
 Python3学习，STRING


	04/28 Python3 学习
 Python3学习，STRING


	04/30 Python3 学习
 Python3学习，STRING




	05/05 NumPy 学习
 Python3学习，STRING
•	Create arrays, the basic data type in NumPy, and how to perform calculations like addition, subtraction, and selection.
•	Calculate descriptive statistics, such as means, medians, and ranges.
•	Explore and calculate common statistical distributions, such as the normal and binomial distributions.
•	Explore and create histograms, a great way of visualizing large quantities of numerical data.
Generally, NumPy arrays are more efficient than lists. One reason is that they allow you to do element-wise operations. An element-wise operation allows you to quickly perform an operation, such as addition, on each element in an array.
A two-dimensional array has two axes: axis 0 represents the values that share the same indexical position (are in the same column), and axis 1 represents the values that share an array (are in the same row).


	05/06 NumPy 学习
 


	05/07 前端学习
 写网页用于展示，读书分享
已完成：页面基础架构

	05/08 前端学习
 写网页用于展示，读书分享
计划进行：增加特色页面，例如数据展示
已完成：check-out界面改为快报填写
	05/08 前端学习
 写网页用于展示，读书分享
计划进行：增加特色页面，例如数据展示

	05/08 前端学习
 写网页用于展示，读书分享
计划进行：增加特色页面，例如数据展示


	05/13 HTTP学习
 计划进行：学习POSTMAN


	05/22 NUMPY
 median:  the median would be 3, because it is positioned half-way between the minimum value and the maximum value.

	05/23 NUMPY
 50% of the dataset will lie within the interquartile range. The interquartile range gives us an idea of how spread out our data is. The smaller the interquartile range value, the less variance in our dataset. The greater the value, the larger the variance.
 Similar to the interquartile range, the standard deviation tells us the spread of the data. The larger the standard deviation, the more spread out our data is from the canter. The smaller the standard deviation, the more the data is clustered around the mean.


	05/24 NUMPY
 We can use np.mean with logical statements to calculate a percentage of values.
np.mean(temps < 60)
The mean is also known as the average and is one of the central positions that helps us understand our data.

	05/27 NUMPY
 # This imports the plotting package. We only need to do this once. 
from matplotlib import pyplot as plt 
# This plots a histogram
 plt.hist(data) 
# This displays the histogram
 plt.show()

	05/29 sites build
 VUE
 
	05/31 combine multiple excels
 pandas
可以顺利打印出文件名，但下一步无法读取

	06/06 
 pandas
	06/10 ANT G2 
 学习G2语法中的基础功能
1、	创建图表容器
2、	列出数据 const data
3、	创建chart对象
4、	载入数据源
•	5、创建图形语法，绘制柱状图，由 area 和 cost 两个属性决定图形位置，area 映射至 x 轴，cost 映射至 y 轴
•	      chart.interval().position('area*cost').color('area')
6、渲染图表
分组柱状图

{
          "name": "本期费用",
          "area": "日本大区",
          "cost": 900
      }];
chart.interval().position('area*cost').color('name').adjust([{
        type: 'dodge',
        marginRatio: 1 / 32
      }]);

	06/11 ANT G2 
 学习G2语法中的基础功能

	06/11 ANT G2  and Basic HTML
 HTML 中的表格：
<table>
    <tr>
      <th scope="col">Company Name</th>
      <th scope="col">Number of Items to Ship</th>
      <th scope="col">Next Action</th>
    </tr>
    <tr>
      <td>Adam's Greenworks</td>
      <td>14</td>
      <td>Package Items</td>
    </tr>
  </table>
	Table Borders
	Spanning Columns
<td colspan="2">Out of Town</td>
	Spanning Rows
<td rowspan="2">14</td>
<thead>
<tbody>
<tfoot>

	Styling with CSS

	06/13 ANT G2  and Basic CSS
 copy two forms

	06/14 ANT G2  and Basic CSS
 Node and SQL
One of the most essential skills as a programmer is being able to identify and utilize the appropriate tool for a specified task. 
In the context of database management, this will mean using SQL to specify, store, update and retrieve data. 
In the context of web programming, this will mean writing JavaScript to automate, manipulate, and return relevant values — for presentation in a website or use in a backend script. 
	Open a database
const { printQueryResults } = require('./utils');
// require the 'sqlite3' package here
const sqlite3 = require('sqlite3');

// open up the SQLite database in './db.sqlite'
const db = new sqlite3.Database('./db.sqlite');
-  do something
db.all("SELECT * FROM TemperatureData WHERE year = 1970", (error, rows) => {
  printQueryResults(rows);
})
db.get("SELECT * FROM TemperatureData WHERE year = 1992")
	Using Placeholders
ids.forEach((id) => { 
  db.get("SELECT * FROM TemperatureData WHERE id = $id",
  {
    $id: id
  },
  (error, rows) => {
    printQueryResults(rows);
  })
})

	Insert value
db.run('INSERT INTO TemperatureData (location, year) VALUES ($location, $year)', {
  $location: newRow.location,
  $year: newRow.year
}, function(error) {
  // handle errors here!

  console.log(this.lastID);
});
	Handling errors
For db.run(), db.each(), db.get(), and db.all(), the first argument to the callback will always be an Errorobject if an error occurred. If there is no error, this argument will be null. We can check if this error exists, and if it does exist, we can handle it.
	Serial Queries
	06/17 deploy a website/ quickbook html	
 Repo
GitHub repository is an online, central storage place where you can store files and all the versions of those files.

 the quick book site
Two parts: one is display; one is adding entry

	06/18 quickbook html- refine
 

	06/19 quickbook html- refine 
 build up another version of budget input and display
Copy some codes from todo list, find a simple one, learn from it

	06/24 quickbook html- input
 完成input页面


	06/25 quickbook html- display
 完成display页面
- show the variance: grouped-bar chart

	06/26 quickbook html- display
 plan: one more page
- 
 learn Java
	06/27 quickbook html- display
 learn about the input function
- 
	06/28 about OAuth 2.0
 learn about the OAuth 2.0
 Refine the display page
- 
	07/01 about OAuth 2.0
 learn about the OAuth 2.0
简单说，OAuth 就是一种授权机制。
数据的所有者告诉系统，同意授权第三方应用进入系统，获取这些数据。
系统从而产生一个短期的进入令牌（token），用来代替密码，供第三方应用使用。
令牌（token）与密码（password）的作用是一样的，都可以进入系统，但是有三点差异。

（1）令牌是短期的，到期会自动失效，用户自己无法修改。密码一般长期有效，用户不修改，就不会发生变化。

（2）令牌可以被数据所有者撤销，会立即失效。以上例而言，屋主可以随时取消快递员的令牌。密码一般不允许被他人撤销。

（3）令牌有权限范围（scope），比如只能进小区的二号门。对于网络服务来说，只读令牌就比读写令牌更安全。密码一般是完整权限。

四种令牌方式：

	授权码（authorization-code）
https://b.com/oauth/authorize?
  response_type=code&
  client_id=CLIENT_ID&
  redirect_uri=CALLBACK_URL&
  scope=read

	隐藏式（implicit）
https://b.com/oauth/authorize?
  response_type=token&
  client_id=CLIENT_ID&
  redirect_uri=CALLBACK_URL&
  scope=read

	密码式（password）
https://oauth.b.com/token?
  grant_type=password&
  username=USERNAME&
  password=PASSWORD&
  client_id=CLIENT_ID

	客户端凭证（client credentials）
https://oauth.b.com/token?
  grant_type=client_credentials&
  client_id=CLIENT_ID&
  client_secret=CLIENT_SECRET

	07/02 about CSS GRID
 learn about basic CSS GRID
 Refine the display page

	07/03 about bootstrap
 learn about bootstrap
•	There are a few required links to use Bootstrap (the CSS file and two <meta> tags)

Bootstrap simplifies the layout of a website using a grid system
One benefit of using Bootstrap is that it incorporates responsive design. With responsive design, the layout of the content changes to accommodate a user’s screen size.
The naming convention follows a pattern of "col-{breakpoint}-{width}"


	07/04 about bootstrap
 learn about bootstrap
	Bootstrap Utilities and Components
	Adding Colors
<div class="bg-success text-white">
	Styling Text
<p class="text-uppercase font-italic text-right">

	07/05 about bootstrap
 learn about bootstrap

	Element Positioning
<div class="position-static">...</div>
<div class="position-relative">...</div>
<div class="position-absolute">...</div>
<div class="position-fixed">...</div>
<div class="position-sticky">...</div>

	The Navigation Component
The nav component is often specific to one or a few webpages, whereas a navbar often appears on all the pages of a website
<nav class="nav justify-content-center">
				  <a href="#" class="nav-link">About Us</a>
          <a href="#" class="nav-link">Contact Us</a>
          <a href="#" class="nav-link">Subscribe</a>
        </nav>

	The Button Component
Buttons represent a clickable object that does something else in return like navigating to a different page, submitting a form, or opening a popup, just to name a few.
	Collapsing a Component
Toggle the visibility of content across your project with a few classes and our JavaScript plugins.
<button class="btn btn-link" aria-expanded="false" aria-controls="seedSpecial" data-target="#seedSpecial" data-toggle="collapse">
          Gearing to Grow?
        </button>
        <p class="collapse" id="seedSpecial">
          Use promo code "GGarden" for additional 15% off your entire purchase!
        </p>

	07/08 about bootstrap
 learn about bootstrap

	Creating a Navbar

"navbar-brand"
class="nav-link"
	The Jumbotron Component
makes content stand out.

	Adding a Card
	The Carousel Component
<!-- Edit the elements below to make a carousel component: -->
        <!-- Parent Carousel div -->
        <div id="gardeningAlbum" class="carousel slide" data-ride="carousel">
          <!-- Inner Carousel div -->
          <div class="carousel-inner">
            <!-- First slide div -->
            <div class="carousel-item active">
              <img src="https://s3.amazonaws.com/codecademy-content/courses/learn-bootstrap-components/garden-slide1.jpg" class="d-block w-100" alt="butterfly in field">
            </div>
            <!-- End of first slide -->
            <!-- Second slide div -->
            <div class="carousel-item">
              <img src="https://s3.amazonaws.com/codecademy-content/courses/learn-bootstrap-components/garden-slide2.jpg" class="d-block w-100" alt="colorful tulips">
            </div>
            <!-- End of second slide -->
            <!-- Third slide div -->
            <div class="carousel-item">
              <img src="https://s3.amazonaws.com/codecademy-content/courses/learn-bootstrap-components/garden-slide3.jpg" class="d-block w-100" alt="inside of a greenhouse">
            </div>
            <!-- End of third slide -->
          </div>
          <!-- End of inner carousel -->
          <!-- Add code for controls below: -->
          <a class="carousel-control-prev" href="#gardeningAlbum" role="button" data-slide="prev">
            <span class="carousel-control-prev-icon" aria-hidden="true"></span>
            <span class="sr-only">Previous</span>
          </a>
          <a class="carousel-control-next" href="#gardeningAlbum" role="button" data-slide="next">
            <span class="carousel-control-next-icon" aria-hidden="true"></span>
            <span class="sr-only">Next</span>
          </a>
        </div>
        <!-- End of carousel component -->


	07/09 about bootstrap
 learn about bootstrap
A project
- navbar
-- add logo

	07/10 about bootstrap
 learn about bootstrap
project
	07/12 about bootstrap
 learn about bootstrap
project
	07/15 about bootstrap
 learn about bootstrap
Complete the course of bootstrap
	07/16 git learn
 learn about git workflow

Learn git
Git is a software that allows you to keep track of changes made to a project over time. Git works by recording the changes you make to a project, storing those changes, then allowing you to reference them as needed.
1: git init
The word init means initialize. The command sets up all the tools Git needs to begin tracking changes made to the project.

2: three parts
A Git project can be thought of as having three parts:
1.	A Working Directory: where you’ll be doing all the work: creating, editing, deleting and organizing files
2.	A Staging Area: where you’ll list changes you make to the working directory
3.	A Repository: where Git permanently stores those changes as different versions of the project

•	git init creates a new Git repository
•	git status inspects the contents of the working directory and staging area
•	git add adds files from the working directory to the staging area
•	git diff shows the difference between the working directory and the staging area
•	git commit permanently stores file changes from the staging area in the repository
•	git log shows a list of all previous commits

A project managed with Git is called a Git repository.
 GitHub allows you to store your local Git repositories in the cloud. With GitHub, you can backup your personal files, share your code, and collaborate with others.
	07/17 git learn
 learn about git workflow
Backtrack in git
Git reset 7 characters

three ways to 
Let’s take a moment to review the new commands:
•	git checkout HEAD filename: Discards changes in the working directory.
•	git reset HEAD filename: Unstages file changes in the staging area.
•	git reset commit_SHA: Resets to a previous commit in your commit history.
	07/18 git learn
 learn about git workflow
Backtrack in git
	07/19 git learn
 learn about git workflow
Git branching
You can use the command below to answer the question: “which branch am I on?”
git branch

To create a new branch, use:
git branch new_branch
git checkout branch_name


git merge branch_name
-  merge conflict.
The command
git branch -d branch_name
will delete the specified branch from your Git project.
The following commands are useful in the Git branch workflow.
•	git branch: Lists all a Git project’s branches.
•	git branch branch_name: Creates a new branch.
•	git checkout branch_name: Used to switch from one branch to another.
•	git merge branch_name: Used to join file changes from one branch to another.
•	git branch -d branch_name: Deletes the branch specified
	07/22 git learn
 learn about git workflow
The workflow for Git collaborations typically follows this order:
1.	Fetch and merge changes from the remote
2.	Create a branch to work on a new project feature
3.	Develop the feature on your branch and commit your work
4.	Fetch and merge from the remote again (in case new commits were made while you were working)
5.	Push your branch up to the remote for review

•	A remote is a Git repository that lives outside your Git project folder. Remotes can live on the web, on a shared network or even in a separate folder on your local computer.

•	git clone: Creates a local copy of a remote.
•	git remote -v: Lists a Git project’s remotes.
•	git fetch: Fetches work from the remote into the local copy.
•	git merge origin/master: Merges origin/master into your local branch.
•	git push origin <branch_name>: Pushes a local branch to the origin remote.




	07/23 git learn
 learn about git workflow
Git fetch origin
Git teamwork

	07/24 interactive webpage
 learn about JavaScript in HTML

	07/25 interactive webpage
 learn about JavaScript in HTML
Quick review
HTML creates the skeleton of a webpage, but JavaScript introduces interactivity
•	The defer attribute ensures that the entire HTML file has been parsed before the script is executed.
•	The async flag will allow the HTML parser to continue parsing as the script is being downloaded, but will execute immediately after it has been downloaded.

 l Learn about the DOM architecture and manipulation, 

The Document Object Model, abbreviated DOM, is a powerful tree-like structure that allows programmers to conceptualize hierarchy and access the elements on a web page.
For this reason, we like to think of the DOM as the link between an HTML web page and scripting languages.
In the DOM tree, the top-most node is called the root node, and it represents the HTML document. The descendants of the root node are the HTML tags in the document, starting with the <html> tag followed by the <head> and <body> tags and so on.

A parent node is the closet connected node to another node in the direction towards the root.
A child node is the closest connected node to another node in the direction away from the root.

	07/26 interactive webpage
 learn about JavaScript in HTML
Get element by ID
querySelector

innerHTML

## 07/29 interactive Javascript

### Traversing the DOM


### about NLP




 Refine the display page



 API学习：使用API

