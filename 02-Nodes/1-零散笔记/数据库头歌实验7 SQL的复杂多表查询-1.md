---
tags:
  - B-博客
  - 计算机/SQL
---

# 第一关
任务描述
求各颜色零件的平均重量

相关知识
零件表 P 由零件代码（PNO）、零件名 (PNAME)、颜色 (COLOR)、重量 (WEIGHT) 组成；
P 表如下图：

![图 1](https://data.educoder.net/api/attachments/WE5uWktSUlFtclFzTm5DKzJoeVVkZz09)
 

现已构建 P 表，结构信息如下：

![图二](https://data.educoder.net/api/attachments/YmkxL0hNbW1sWFFmQkFqN3F1QVVKUT09)

开始你的任务吧，祝你成功！

**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select COLOR,AVG(WEIGHT)

from P

group by COLOR;

########## End ##########
```

# 第二关
任务描述
求北京和天津供应商的总个数

相关知识
供应商表 S 由供应商代码（SNO）、供应商姓名（SNAME）、供应商状态（STATUS）、供应商所在城市（CITY）组成.
S 表如下图：

![图三](https://data.educoder.net/api/attachments/OWZuRDJBYW5pTEp1V3hJcTkxMHRnQT09)
现已构建 S 表，结构信息如下：

![图三](https://data.educoder.net/api/attachments/V0l6Y2VObWdwMDhNUEVEeFpyai95UT09)

开始你的任务吧，祝你成功！

**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select CITY, COUNT(*)

from S

where CITY="天津" or CITY="北京"

group by CITY;

########## End ##########
```

# 第三关

任务描述
求各供应商供应的零件总数 (SUM_QTY)，结果按 SUM_QTY 降序排序。

相关知识
供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：

![图](https://data.educoder.net/api/attachments/Mi9mUXhzQUowMnVMM1FLVW5QMEtHUT09)

现已构建 SPJ 表，结构信息如下：

![图](https://data.educoder.net/api/attachments/STRJMkdiUjB3ZWtxRWc2dTZ2S3lZdz09)

开始你的任务吧，祝你成功

**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select SNO, SUM(QTY) as SUM_QTY

from SPJ

group by SNO

order by SUM(QTY) desc;

########## End ##########
```

# 第四关

任务描述
求各供应商供应给各工程的零件总数 (SUM_QTY)，结果先按供应商代码 (SNO) 降序排序，再按工程项目代码 (JNO) 降序排序。

相关知识
供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：

![](https://data.educoder.net/api/attachments/Mi9mUXhzQUowMnVMM1FLVW5QMEtHUT09)

现已构建 SPJ 表，结构信息如下：

![](https://data.educoder.net/api/attachments/STRJMkdiUjB3ZWtxRWc2dTZ2S3lZdz09)

开始你的任务吧，祝你成功！
**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select SNO, JNO, sum(QTY)as SUM_QTY

from SPJ

group by SNO

order by SNO desc, JNO desc;


########## End ##########
```

# 第五关

任务描述
求重量大于所有零件平均重量的零件名称

相关知识
零件表 P 由零件代码（PNO）、零件名 (PNAME)、颜色 (COLOR)、重量 (WEIGHT) 组成；
P 表如下图：

 ![](https://data.educoder.net/api/attachments/WE5uWktSUlFtclFzTm5DKzJoeVVkZz09)

现已构建 P 表，结构信息如下：

![](https://data.educoder.net/api/attachments/YmkxL0hNbW1sWFFmQkFqN3F1QVVKUT09)

开始你的任务吧，祝你成功!

**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select PNAME

from P

where WEIGHT>(select avg(WEIGHT) from P);


########## End ##########
```

# 第六关
任务描述
查询供应了 1000 个以上 (不含 1000) 零件的供应商名称, 查询结果按供应商名称降序排序。

相关知识
1、供应商表 S 由供应商代码（SNO）、供应商姓名（SNAME）、供应商状态（STATUS）、供应商所在城市（CITY）组成.
S 表如下图：

![](https://data.educoder.net/api/attachments/OWZuRDJBYW5pTEp1V3hJcTkxMHRnQT09)

现已构建 S 表，结构信息如下：

![](https://data.educoder.net/api/attachments/V0l6Y2VObWdwMDhNUEVEeFpyai95UT09)

2、供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：

![](https://data.educoder.net/api/attachments/Mi9mUXhzQUowMnVMM1FLVW5QMEtHUT09)

现已构建 SPJ 表，结构信息如下：

![](https://data.educoder.net/api/attachments/STRJMkdiUjB3ZWtxRWc2dTZ2S3lZdz09)

开始你的任务吧，祝你成功!

**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select SNAME

from S

where SNO in

(

    select SNO

    from SPJ

    group by SNO

    having sum(QTY)>1000

)

order by SNAME desc;

########## End ##########
```

# 第七关
任务描述
统计 P 表中颜色为蓝色的零件个数，并指定该查询列的名称为“蓝色零件数”

相关知识
零件表 P 由零件代码（PNO）、零件名 (PNAME)、颜色 (COLOR)、重量 (WEIGHT) 组成；
P 表如下图：

 

现已构建 P 表，结构信息如下：

    

开始你的任务吧，祝你成功!

**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select count(COLOR) as "蓝色零件数"

from P

where COLOR="蓝";

########## End ##########
```

# 第八关
任务描述
查询 P 表中各零件的编号，名称及重量按 85%计算后的信息，其中重量按 85%计算后的查询列名改为“零件净重”

相关知识
零件表 P 由零件代码（PNO）、零件名 (PNAME)、颜色 (COLOR)、重量 (WEIGHT) 组成；
P 表如下图：

 

现已构建 P 表，结构信息如下：

    

开始你的任务吧，祝你成功!

**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select PNO, PNAME, WEIGHT*0.85 as "零件净重"

from P;


########## End ##########
```

# 第九关

任务描述
查询 S 表 STATUS 值大于 20 且小于 50，或 SNAME 字段值的第一个字为“精”或第三个字为“益”或“民”的供应商信息

相关知识
供应商表 S 由供应商代码（SNO）、供应商姓名（SNAME）、供应商状态（STATUS）、供应商所在城市（CITY）组成.
S 表如下图：


现已构建 S 表，结构信息如下：



开始你的任务吧，祝你成功！
**代码**
```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select *

from S

where (STATUS>20 and STATUS<50)or SNAME like"精%" or SNAME like"__义%";

########## End ##########
```

# 第十关

任务描述
将 SPJ 表按 QTY 值降序排列，再找出 SPJ 表中前 6 条记录 (用 limit 6)

相关知识
供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select *

from SPJ

order by QTY desc

limit 6;


########## End ##########
```

# 十一关
任务描述
找出供应零件总数量不低于 1000 的供应商号码，及每个供应商供应的总数量 (SUM_QTY)，并且结果按总数量降序排列

相关知识
供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

select SNO, sum(QTY) as SUM_QTY

from SPJ

group by SNO

having sum(QTY)>=1000

order by sum(QTY) desc;

########## End ##########
```

# 十二关
任务描述
找出工程项目 J 1 使用的各种零件的名称及其数量 (SUM_QTY)，查询结果按数量降序排序。

相关知识
1、零件表 P 由零件代码（PNO）、零件名 (PNAME)、颜色 (COLOR)、重量 (WEIGHT) 组成；
P 表如下图：

 

现已构建 P 表，结构信息如下：

    

2、供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

SELECT P.PNAME, SUM(SPJ.QTY) as SUM_QTY

FROM P

JOIN SPJ ON P.PNO = SPJ.PNO

WHERE SPJ.JNO = 'J1'

GROUP BY P.PNAME

order by sum(QTY) desc;

########## End ##########
```

# 十三关

任务描述
求使用了 300 个及以上 P 1 零件的工程名称

相关知识
1、工程项目表 J 由工程项目代码 (JNO)、工程项目名 (JNAME)、工程项目所在城市 (CITY) 组成。
J 表如下图：



现已构建 J 表，结构信息如下：



2、供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

SELECT J.JNAME

FROM J

JOIN SPJ ON J.JNO = SPJ.JNO

WHERE SPJ.PNO = 'P1'

GROUP BY J.JNO, J.JNAME

HAVING SUM(SPJ.QTY) >= 300;

########## End ##########
```

# 十四关

任务描述
求各工程（名）使用的各城市供应的零件总数，结果先按工程名降序排序，再按城市名降序排序。

相关知识
1、供应商表 S 由供应商代码（SNO）、供应商姓名（SNAME）、供应商状态（STATUS）、供应商所在城市（CITY）组成.
S 表如下图：



现已构建 S 表，结构信息如下：



2、工程项目表 J 由工程项目代码 (JNO)、工程项目名 (JNAME)、工程项目所在城市 (CITY) 组成。
J 表如下图：



现已构建 J 表，结构信息如下：



3、供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

SELECT

    J.JNAME ,

    S.CITY ,

    SUM(SPJ.QTY) AS SUM_QTY

FROM

    SPJ

JOIN

    S ON SPJ.SNO = S.SNO

JOIN

    J ON SPJ.JNO = J.JNO

GROUP BY

    J.JNAME, S.CITY

ORDER BY

    J.JNAME DESC, S.CITY DESC;
    
########## End ##########
```

# 十五关

任务描述
用嵌套查询这样的工程：供给该工程的零件 P 3 的平均供应量，大于其中一种供给工程 J 1 的零件的最大供应量

相关知识
供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

SELECT DISTINCT X.JNO

FROM SPJ X

WHERE X.PNO = 'P3'

GROUP BY X.JNO

HAVING AVG(X.QTY) > ANY (

    SELECT MAX(Y.QTY)

    FROM SPJ Y

    WHERE Y.JNO = 'J1'

    GROUP BY Y.PNO

);


########## End ##########
```

# 十六关

任务描述
用链接查询这样的工程：供给该工程的零件 P 3 的平均供应量，大于其中一种供给工程 J 1 的零件的最大供应量

相关知识
供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

SELECT DISTINCT a.JNO

FROM (

    SELECT JNO, AVG(QTY) AS avg_p3_qty

    FROM SPJ

    WHERE PNO = 'P3'

    GROUP BY JNO

) a

JOIN (

    SELECT MAX(QTY) AS max_qty

    FROM SPJ

    WHERE JNO = 'J1'

    GROUP BY PNO

) b ON a.avg_p3_qty > b.max_qty;

########## End ##########
```

# 十七关

任务描述
查询这样的工程号：供应该工程零件 P 3 的平均供应量，不小于工程 J 1 使用各零件合计数量的最小值.

相关知识
供应情况表 SPJ 由供应商代码 (SNO)、零件代码 (PNO)、工程项目代码 (JNO)、供应数量 (QTY) 组成，标识某供应商供应某种零件给某工程项目的数量为 QTY。
SPJ 表如下图：



现已构建 SPJ 表，结构信息如下：



开始你的任务吧，祝你成功!

```sql
USE mydata;

#请在此处添加实现代码

########## Begin ##########

  

SELECT a.JNO

FROM (

    SELECT JNO, AVG(QTY) AS avg_p3_qty

    FROM SPJ

    WHERE PNO = 'P3'

    GROUP BY JNO

) a

WHERE a.avg_p3_qty >= (

    SELECT MIN(total_qty)

    FROM (

        SELECT SUM(QTY) AS total_qty

        FROM SPJ

        WHERE JNO = 'J1'

        GROUP BY PNO

    ) b

);

########## End ##########
```