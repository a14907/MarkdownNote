常用命令：

.loadby sos clr

~0s

打印主线程线程栈的信息
!clrstack -l

!dumpobj /d xxxxxxxx(地址)

查找一个确定类型的方法表的地址
!do xxxxxx(地址)

获得方法表的详细信息
!dumpmt -md <地址>

查看Entry地址中存储的本地代码
!u <地址>