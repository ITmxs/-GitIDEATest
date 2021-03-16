素材：

![img](https://luckly007.oss-cn-beijing.aliyuncs.com/ff3a7c270d3b4169be336135fa924096.jpeg)

**1、计算性别****(**F**列)**

=IF(MOD(MID(E3,17,1),2),"男","女")

**2、出生年月****(**G**列)**

=TEXT(MID(E3,7,8),"0-00-00")

**3、年龄公式****(**H**列)**

=DATEDIF(G3,TODAY(),"y")

**4、退休日期****(**I**列)**

=TEXT(EDATE(G3,12*(5*(F3="男")+55)),"yyyy/mm/dd aaaa")

**5、籍贯****(**M**列)**

=VLOOKUP(LEFT(E3,6)*1,地址库!E:F,2,)

注：附带示例中有地址库代码表

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/b8542db2637148089f98cbcdfa566144.jpeg)

**6、社会工龄**(T列)

=DATEDIF(S3,NOW(),"y")

**7、公司工龄**(W列)

=DATEDIF(V3,NOW(),"y")&"年"&DATEDIF(V3,NOW(),"ym")&"月"&DATEDIF(V3,NOW(),"md")&"天"

**8、合同续签日期**(Y列)

=DATE(YEAR(V3)+LEFTB(X3,2),MONTH(V3),DAY(V3))-1

**9、合同到期日期**(Z列)

=TEXT(EDATE(V3,LEFTB(X3,2)*12)-TODAY(),"[<0]过期0天;[<30]即将到期0天;还早")

**10、工龄工资**(AA列)

=MIN(700,DATEDIF($V3,NOW(),"y")*50)

**11、生肖(AB列)**

=MID("猴鸡狗猪鼠牛虎兔龙蛇马羊",MOD(MID(E3,7,4),12)+1,1)

**二、员工考勤表公式**

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/6de572f61e4546259f0215bb86fb9aee.jpeg)

**1、本月工作日天数(AG列)**

=NETWORKDAYS(B$5,DATE(YEAR(N$4),MONTH(N$4)+1,),)

**2、调休天数公式(AI列)**

=COUNTIF(B9:AE9,"调")

**3、扣钱公式(AO列)**

婚丧扣10块，病假扣20元，事假扣30元，矿工扣50元

=SUM((B9:AE9={"事";"旷";"病";"丧";"婚"})*{30;50;20;10;10})

**四、员工数据分析公式**

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/92dc50096e534ee5937f0120647c0cf2.jpeg)

**1、本科学历人数**

=COUNTIF(D:D,"本科")

2、**办公室****本科**学历人数

=COUNTIFS(A:A,"办公室",D:D,"本科")

**3、30~40岁总人数**

=COUNTIFS(F:F,">=30",F:F,"<40")

**五、其他公式**

**1、提成比率计算**

=VLOOKUP(B3,$C$12:$E$21,3)

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/b2b7d6891ac14200ac10e6120f3ccf39.jpeg)

**2、个人所得税计算**

假如A2中是应税工资，则计算个税公式为：

=5*MAX(A2*{0.6,2,4,5,6,7,9}%-{21,91,251,376,761,1346,3016},)

**3、工资条公式**

=CHOOSE(MOD(ROW(A3),3)+1,工资数据源!A$1,OFFSET(工资数据源!A$1,INT(ROW(A3)/3),,),"")

注：

- A3:标题行的行数+2,如果标题行在第3行，则A3改为A5
- 工资数据源!A$1:工资表的标题行的第一列位置

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/ac2e227ff6b84bcd949ba641b6cc1358.gif)

4、**Countif函数统计身份证号码出错的解决方法**

由于Excel中数字只能识别15位内的，在Countif统计时也只会统计前15位，所以很容易出错。不过只需要用 &"*"转换为文本型即可正确统计。

=Countif(A:A,A2&"*")

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/9acd223ec67f464baa726307ad52e455.jpeg)

**六、利用数据透视表完成数据分析**

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/4da11b904c97439798b56604dce95679.jpeg)

1、各部门人数占比

统计每个部门占总人数的百分比

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/47a43c67fbc540379bf8380fcadd75d4.gif)

**2、各个年龄段人数和占比**

公司员工各个年龄段的人数和占比各是多少呢？

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/4fe91f1d11834a50b7adad7cecec62aa.gif)

**3、各个部门各年龄段占比**

分部门统计本部门各个年龄段的占比情况

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/e921c5d8240d429d8f6b7cb4f6e8b0d0.gif)

**4、各部门学历统计**

各部门大专、本科、硕士和博士各有多少人呢？

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/1428948e3971449c8d139e8fec3100c1.gif)

**5、按年份统计各部门入职人数**

每年各部门入职人数情况

![img](http://5b0988e595225.cdn.sohucs.com/images/20180203/2151e0bd9caa44e4bd959e04bc0e0391.gif)

保保说：今天分享的Excel公式虽然很全，但实际和会计实际要用到的excel公式相比，还会有很多遗漏。欢迎做会计的同学们补充你工作中最常用到的公式。

一、条件判断：IF函数。

　　目的：判断成绩所属的等次。

　　方法：

　　1、选定目标单元格。

　　2、在目标单元格中输入公式：=IF(C3>=90."优秀",IF(C3>=80."良好",IF(C3>=60."及格","不及格")))。

　　3、Ctrl+Enter填充。

　　解读：

　　IF函数是条件判断函数，根据判断结果返回对应的值，如果判断条件为TRUE，则返回第一个参数，如果为FALSE，则返回第二个参数。



![img](https://pic3.zhimg.com/v2-abae19c5643eda976779fe6e337cafba_b.jpg)



　　二、条件求和：SUMIF、SUMIFS函数。

　　目的：求男生的总成绩和男生中分数大于等于80分的总成绩。

　　方法：

　　1、在对应的目标单元格中输入公式：=SUMIF(D3:D9."男",C3:C9)或=SUMIFS(C3:C9.C3:C9.">=80",D3:D9."男")。

　　解读：

　　1、SUMIF函数用于单条件求和。暨求和条件只能有一个。易解语法结构为：SUMIF(条件范围，条件，求和范围)。

　　2、SUMIFS函数用于多条件求和。暨求和条件可以有多个。易解语法结构：SUMIFS(求和范围，条件1范围，条件1.条件2范围，条件2.……条件N范围，条件N)。



![img](https://pic1.zhimg.com/v2-4779e1425805ee4f441f26394d7e686c_b.jpg)



　　三、条件计数：COUNTIF、COUNTIFS函数。

　　目的：计算男生的人数或男生中成绩>=80分的人数。

　　方法：

　　1、在对应的目标单元格中输入公式：=COUNTIF(D3:D9."男")或=COUNTIFS(D3:D9."男",C3:C9.">=80")。

　　解读：

　　1、COUNTIF函数用于单条件计数，暨计数条件只能有一个。易解语法结构为：COUNTIF(条件范围，条件).

　　2、COUNTIFS函数用于多条件计数，暨计数条件可以有多个。易解语法结构为：COUNTIFS(条件范围1.条件1.条件范围2.条件2……条件范围N，条件N)。



![img](https://pic2.zhimg.com/v2-1a3fbc6a2373ce37c6529bb1c2bbae25_b.jpg)



　　四、数据查询：VLOOKUP函数。

　　目的：查询相关人员对应的成绩。

　　方法：

　　在目标单元格中输入公式：=VLOOKUP(H3.B3:C9.2.0)。

　　解读：

　　函数VLOOKUP的基本功能就是数据查询。易解语法结构为：VLOOKUP(查找的值，查找范围，找查找范围中的第几列，精准匹配还是模糊匹配)。



![img](https://pic1.zhimg.com/v2-f5c5d6ff856e7ea1ec89cc1b66161290_b.jpg)



　　五、逆向查询：LOOKUP函数。

　　目的：根据学生姓名查询对应的学号。

　　方法：

　　在目标单元格中输入公式：=LOOKUP(1.0/(B3:B9=H3),A3:A9)。

　　解读：

　　公式LOOKUP函数的语法结构为：LOOKUP(查找的值，查找的条件，返回值的范围)。本示例中使用的位变异用法。查找的值为1.条件为0.根据LOOKUP函数的特点，如果 LOOKUP 函数找不到 lookup_value，则该函数会与 lookup_vector 中小于或等于 lookup_value 的最大值进行匹配。



![img](https://pic3.zhimg.com/v2-8572358c5420cec546f2428e60678b12_b.jpg)



　　六、查询好搭档：INDEX+MATCH 函数

　　目的：根据姓名查询对应的等次。

　　方法：

　　在目标单元格中输入公式：=INDEX(E3:E9.MATCH(H3.B3:B9.0))。

　　解读：

　　1、INDEX函数：返回给定范围内行列交叉处的值。

　　2、MATCH函数：给出指定值在指定范围内的所在位置。

　　3、公式：=INDEX(E3:E9.MATCH(H3.B3:B9.0))，查询E3:E9中第MATCH(H3.B3:B9.0)行的值，并返回。



![img](https://pic3.zhimg.com/v2-409447f4527cac9e8637ef626c3fca86_b.jpg)



　　七、提取出生年月：TEXT+MID函数。

　　目的：从指定的身份证号码中提取出去年月。

　　方法：

　　1、选定目标单元格。

　　2、输入公式：=TEXT(MID(C3.7.8),"00-00-00")。

　　3、Ctrl+Enter填充。

　　解读：

　　1、利用MID函数从C3单元格中提取从第7个开始，长度为8的字符串。

　　2、利用TEXT函数将字符的格式转换为“00-00-00”的格式，暨1965-08-21.



![img](https://pic1.zhimg.com/v2-7e378aa1bf73023548ebe6f86476f660_b.jpg)



　　八、计算年龄：DATEDIF函数。

　　目的：根据给出的身份证号计算出对应的年龄。

　　方法：

　　1、选定目标单元格。

　　2、输入公式：=DATEDIF(TEXT(MID(C3.7.8),"00-00-00"),TODAY(),"y")&"周岁"。

　　3、Ctrl+Enter填充。

　　解读：

　　1、利用MID获取C3单元格中从第7个开始，长度为8的字符串。

　　2、用Text函数将字符串转换为：00-00-00的格式。暨1965-08-21.

　　3、利用DATEDIF函数计算出和当前日期(TODAY())的相差年份(y)。



![img](https://pic2.zhimg.com/v2-d08ad78ca4b7651748a8d11a768a2c99_b.jpg)



　　九、中国式排名：SUMPRODUCT+COUNTIF函数。

　　目的：对成绩进行排名。

　　方法：

　　1、选定目标单元格。

　　2、在目标单元格中输入公式：=SUMPRODUCT((C$3:C$9>C3)/COUNTIF(C$3:C$9.C$3:C$9))+1.

　　3、Ctrl+Enter填充。

　　解读：公式的前半部分(C$3:C$9>C3)返回的是一个数组，区域C$3:C$9中大于C3的单元格个数。后半部分COUNTIF(C$3:C$9.C$3:C$9)可以理解为：*1/COUNTIF(C$3:C$9.C$3:C$9),公式COUNTIF(C$3:C$9.C$3:C$9)返回的值为1.只是用于辅助计算。所以上述公式也可以简化为：=SUMPRODUCT((C$3:C$9>C3)*1)+1.



![img](https://pic4.zhimg.com/v2-61911a8b18cbeafacdeef66079770e8b_b.jpg)