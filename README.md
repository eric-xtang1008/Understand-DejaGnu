# Understand-DejaGnu
A guide for study DejaGnu
# DejaGnu

## 什么是 DejaGnu & Tcl & expect

### Tcl

Tcl (Tool Command Language)是一种类似shell的脚本语言

例: HelloWorld.tcl

	#!/usr/bin/tclsh
	puts "Hello World!"
执行：

	tclsh HelloWorld.tcl

可以网上查找 Tcl 语法自己学习一下

### Expect

expect 是建立在tcl基础上的一个工具，它用来让一些需要交互的任务自动化地完成,
比如我们用ssh远程登录一台主机：
1. ssh username@ip
2. 需要人输入登录密码
3. 密码正确登录成功

如果换成脚本怎么去做？

例：ssh.exp

	#!/usr/bin/expect
	set timeout 30

	spawn ssh -l eric.tang 192.168.110.86

	expect "password:"

	send "12345678\r"

	interact
执行：

	expect ssh.exp
or

	sudo chmod u+x ssh.exp 
	./ssh.exp

解释：

	spawn启动指定进程---expect获取指定关键字---send向指定程序发送指定字符---执行完成退出.

> expect常用命令:



	spawn			交互程序开始,后面跟shell命令
	expect			后面是查询返回结果是否包含某字符串
	send			执行交互动作，比如 send "admin\r"
	interact		表示脚本执行到这里后把控制权交给控制条，也就是切回手工操作
	exp_continue		在expect中多次匹配就需要用到
	send_user		用来打印输出 相当于shell中的echo
	exit			退出expect脚本
	set timeout		设置超时时间

### DejaGnu
DejiaGnu 是一种开源自动化测试框架。 是用expect脚本语言来设计的，可以理解为他是一种自定义Tcl程序库。所以DejiaGnu是用tcl语言写的。说的很抽象，我们可以直接下载代码看下：

	https://ftp.gnu.org/gnu/dejagnu/

目录结构如下：

	├── aclocal.m4
	├── AUTHORS
	├── baseboards
	├── ChangeLog
	├── ChangeLog-1992
	├── compile
	├── config
	├── config.guess
	├── config.sub
	├── configure
	├── configure.ac
	├── contrib
	├── COPYING
	├── dejagnu.h
	├── depcomp
	├── doc
	├── INSTALL
	├── install-sh
	├── lib
	├── MAINTAINERS
	├── Makefile.am
	├── Makefile.in
	├── missing
	├── NEWS
	├── README
	├── runtest
	├── runtest.exp
	├── stub-loader.c
	├── testglue.c
	├── testsuite
	├── texinfo.tex
	└── TODO
其中runtest是用shell脚本写的，用来启动 runtest.exp   

> **runtest is the test driver for DejaGnu**, 相当gcc 和 c 之间的关系

运行方式：
1. make check  
	要想支持这种方式需要在Makefile.in中加入对DejaGnu的支持，即AUTOMAKE_OPTIONS = dejagnu make工具会自动生成check 目标以及在测试中需要的文件，比如site.exp

2. runtest  
	这是DejaGnu专门来启动测试的一个脚本命令，make check 最终也是调用这个命令。  

	runtest --tool gas --srcdir path/binutils/gas/testsuite/

输出结果：  
* xxx.sum - 概要结果  
* xxx.log - 详细日志，除了测试结果还有测试交互过程  

#### 输出结果说明

* PASS - 期望测试**成功** 实际测试**成功**  
* XPASS - 期望测试**失败** 实际测试**成功**  
* FAIL - 期望测试**成功** 实际测试**失败**  
* XFAIL - 期望测试**失败** 实际测试**失败**   
* UNRESOLVED - 测试程序有问题，还没有解决  
* UNTESTED - 测试程序还没完成
* UNSUPPORTED - 测试程序依赖某些特性，但这些特性还不支持  
* ERROR - 测试程序自己发现的问题
* WARNING - 表面潜在的问题   
* NOTE - 关于测试程序的一些信息

#### runtest 测试选项

	USAGE: runtest [options...]
	--all, -a		Print all test output to screen
	--build [triplet]	The canonical triplet of the build machine
	--debug			Set expect debugging ON
	--directory name	Run only the tests in directory 'name'
	--help			Print help text
	--host [triplet]	The canonical triplet of the host machine
	--host_board [name]	The host board to use
	--ignore [name(s)]	The names of specific tests to ignore
	--log_dialog		Emit Expect output on stdout
	--mail [name(s)]	Whom to mail the results to
	--objdir [name]		The test suite binary directory
	--outdir [name]		The directory to put logs in
	--reboot		Reboot the target (if supported)
	--srcdir [name]		The test suite source code directory
	--status		Set the exit status to fail on Tcl errors
	--strace [number]	Turn on Expect tracing
	--target [triplet]	The canonical triplet of the target board
	--target_board [name(s)] The list of target boards to run tests on
	--tool [name(s)]	Run tests on these tools
	--tool_exec [name]	The path to the tool executable to test
	--tool_opts [options]	A list of additional options to pass to the tool
	--verbose, -v		Produce verbose output
	--version, -V		Print all relevant version numbers
	--xml, -x		Write out an XML results file
	--D[0-1]		Tcl debugger
	script.exp[=arg(s)]	Run these tests only

#### 关键选项说明
**--tool**  Specifies which testsuite to run, and what initialization module to use

> 参考binutils     
binutils 里面有 gas, objdump ...，如果我们要单独测是gas里面的testsuite 命令如下  

	runtest --tool gas --srcdir path/binutils/gas/testsuite/

>脚本语言没有main这样的函数，所以第一句就是开始

## 官方文档  
  https://www.gnu.org/software/dejagnu/dejagnu.pdf
  
