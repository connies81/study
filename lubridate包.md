## 常用的R语言包的总结——时间处理的包**lubridate**
lubridate包主要有两类函数：一类是处理时点数据(time instants)，另一类是处理时段数据（time spans）。时点类函数，它包括了解析、提取、修改。<br>
**1、解析日期和时间**<br>
ymd("20180213")<br>
ymd("2018-3-9")<br>
dmy("09/10/18")<br>
ymd\_hms("2018-05-14 12:13:40")<br>
x = ymd\_hms("2011-08-10 14:05:08", tz = "Pacific/Auckland")<br>
**2、提取信息**<br>
second(x) &emsp;&emsp;#提取秒<br>
minute(x)&emsp;&emsp;&emsp;#提取分<br>
hour(x)&emsp;&emsp;&emsp;#提取小时<br>
day(x)<br>
month(x)<br>
year(x)&emsp;&emsp;&emsp;#提取年份<br>
wday(x,label = T)<br>
yday(x)&emsp;&emsp;&emsp;看x是一年中的第几天<br>
**3、时区**<br>
奥克兰9点电话会议，求芝加哥几点开<br>
meeting <- ymd\_hms("2011-07-01 09:00:00", tz = "Pacific/Auckland")<br>
with\_tz(meeting, "America/Chicago")<br>

    芝加哥如果误认为9点开，那么奥克兰接到电话的时间<br>
mistake <- force\_tz(meeting, "America/Chicago")
with\_tz(mistake, "Pacific/Auckland")<br>
**4、时间区间**<br>

 **interval**:是两个时间点数据构成的时间区间对象。
arrive <- ymd\_hms("2018-06-04 12:00:00", tz = "Pacific/Auckland")<br>
leave <- ymd\_hms("2018-08-10 14:00:00", tz = "Pacific/Auckland")<br>
auckland <- interval(arrive, leave) 奥克兰出差时间区间<br>
auckland <- arrive %--% leave<br>
jsm <- interval(ymd(20180720, tz = "Pacific/Auckland"), ymd(20180831, tz = "Pacific/Auckland")) 奥克兰朋友出游时间段<br>
auckland<br>返回2018-06-04 12:00:00 NZST--2018-08-10 14:00:00 NZST这个时间区间
jsm
<br>返回2018-07-20 NZST--2018-08-31 NZST<br>
lubridate::setdiff(auckland, jsm)#什么时候去拜访朋友？即朋友和我都在奥克兰？<br>
2018-06-04 12:00:00 NZST--2018-07-20 NZST<br>
**5、时间间隔和周期——Durations and Periods**<br>
**dminutes(2)**#[1] "120s (~2 minutes)<br>
意思是2分钟的长度，单位是秒。类似的函数还有ddays，dyears。<br>还有一类函数也是时间长度，只是显示的形式不一样。如<br>

**years(2)**#[1] "2y 0m 0d 0H 0M 0S"<br>
他们大多数没有区别。比如： ymd(20110101) + dyears(1)和 ymd(20110101) + years(1)
但是，年份要是是闰年就有区别了，因为dyears这类函数都是以365天为一年，而周期函数years就不一样了。比如：ymd(20120101) + **years(1)**[1] "2013-01-01"
<br>ymd(20120101) + **dyears(1)** [1] "2012-12-31"<br>

 定义了一个时间点：meeting <- ymd\_hms("2011-07-01 09:00:00", tz = "Pacific/Auckland")<br>
 meeting+**days(10)**表示10天以后的日期[1] "2011-07-11 09:00:00 NZST"
<br> meetings<-meeting+days(0:5)表示5天会议的具体日期<br>[1] "2011-07-01 09:00:00 NZST" "2011-07-02 09:00:00 NZST"<br>
[3] "2011-07-03 09:00:00 NZST" "2011-07-04 09:00:00 NZST"<br>
[5] "2011-07-05 09:00:00 NZST" "2011-07-06 09:00:00 NZST"<br>
新的会议时间是new\_meeting<-meeting + days(c(2,4,5,7,10))<br>
[1] "2011-07-03 09:00:00 NZST" "2011-07-05 09:00:00 NZST"<br>
[3] "2011-07-06 09:00:00 NZST" "2011-07-08 09:00:00 NZST"<br>
[5] "2011-07-11 09:00:00 NZST"<br>
老会议和新会议时间是否冲突？<br>
**interval**(new\_meeting[1],new\_meeting[5])返回新会议的区间<br>
 meetings **%within%** meeting_in[1] FALSE FALSE  TRUE  TRUE  TRUE  TRUE 有四天冲突。<br>

我在奥克兰呆多长时间？<br>
auckland：2018-06-04 12:00:00 NZST--2018-08-10 14:00:00 NZST

auckland/ddays(1)
[1] 67.08333
呆67天

auckland/weeks(1)
[1] 9.583333呆9个多周

as.period(auckland)[1] "2m 6d 2H 0M 0S"
<br>
有时候不需要小数，只需要知道个大数时段，于是就出现时间的整除和取余算法。<br>
auckland **%/%** weeks(1)[1] 9 呆9周<br>
auckland%/%months(1)[1] 2 呆2个月

auckland**%%**months(1)
[1] 2018-08-04 12:00:00 NZST--2018-08-10 14:00:00 NZST除了那两个月外还有8月的这段时间。但是剩下是几天呢？
**as.period**(auckland %% months(1)) "6d 2H 0M 0S"

**6、时间取整函数——round\_date()**<br>

四舍五入取整：round\_date()，向下取整：floor\_date()，向上取整：ceiling\_date()。
函数中的unit参数是选择以什么单位进行取整。<br>
 round\_date(now(),unit="hour")[1] "2018-03-15 11:00:00 CST"
round\_date(now(),unit="day")[1] "2018-03-15 CST"








