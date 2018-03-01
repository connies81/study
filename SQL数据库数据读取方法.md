## SQL数据库读取表数据，转化成excel数据。

方法1——数据库数据界面点击导出

[SQLServer表数据导出为Excel文件](https://jingyan.baidu.com/article/3065b3b68f2ab7becef8a449.html)

但是该方法的缺陷是表格中的数据不能为空，并且数据表中的列数不能超过256个。

方法2——SQL语句直接实现Excel与数据库的导入导出

[通过SQL语句直接实现Excel与数据库的导入导出](http://blog.csdn.net/sophia09shen/article/details/973472)

方法3——R语言连接读取SQL数据库数据

odbcConnect("stocksDSN",uid = "myuser",pwd = "mypassword")

ch<-odbcConnect("gaochenyan")

