Tcl 语法说明
==================================

## 字符
一个Tcl标识符是用来标识变量，函数，或任何其它用户定义的项目的名称。一个标识符开始以字母A到Z或a〜z或后跟零个或多个字母下划线（_），下划线，美元（$）和数字（0〜9）。TCL不允许标点字符，如@和％标识符。TCL是大小写敏感的语言。



|  字符  | 作用 |
| :---: | :---: |
| # | 注释 |
| ; 或 newline | 语句分隔符|
| {} | 函数体 参数列表 |
| [] | 表达式|

## 变量

如果在proc 里使用global　声明，说明这个全局可见，但必须set后才可用，否则就会出现未定义错误（no such variable）。

set 是定义　

global　是限定作用域　　　

## 过程（函数）
语法:

	proc procedureName {arguments} {
		body
	}
多个参数1

	#!/usr/bin/tclsh
	set a 1
	set b 2
	proc add {var1 var2} {
		upvar $var1 svar1
		upvar $var2 svar2
		set x [expr $svar1 + $svar2];
		return $x
	}

	puts [add a b]

多个参数2

	#!/usr/bin/tclsh
	proc add {a b} {
		set x [expr $a + $b];
		return $x
	}
	puts [add 1 2]


多个参数3

	#!/usr/bin/tclsh
	set a 1
	set b 2
	proc sub {} {
		upvar 1 a a
		upvar 1 b b
		set x [expr $a - $b]
		puts $x
		return [expr $x]
	}

	puts [sub]

默认参数

	#!/usr/bin/tclsh
	proc add {a {b 100}} {
		set x [expr $a + $b];
		return $x
	}
	puts [add 1 2]
	puts [add 1]
## 命令说明
	https://www.tcl.tk/man/tcl8.4/TclCmd/

### set
	 set 作用相当与 '='
	 [ set a 10 => a=10 ]
### upvar   
从stack frame角度去理解： 当调用一个proc 就新建一个stack frame， 函数在这个新建的frame里执行，默认情况下他只能访问这个frame里面的变量， 如果要访问外部变量使用upvar

> level 0
>> level 1
>>> level 2

	#0 #1 #2 从外层到里层
	0   1  2 从里层到外层
### info
	https://www.tcl.tk/man/tcl8.4/TclCmd/info.html
