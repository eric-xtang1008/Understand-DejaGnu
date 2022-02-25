riscv dejagnu
==================================

riscv dejagnu 增加了riscv board的相关的信息，链接地址：

https://github.com/riscv/riscv-dejagnu


default config

	TCL_LIBRARY = /usr/local/share/tcl8.0
	DEJAGNULIBS = /usr/local/share/dejagnu

runtest 要和其对应libary path 要对应上，　否则会出现找不到对应库（*.exp）的情况

	export DEJAGNULIBS=runtestlibpath

## 单独测试一个test case

	../../../riscv-dejagnu/runtest --tool gcc --target_board='riscv-sim/-march=rv64gcb/-mabi=lp64d/-mcmodel=medany'  xxx.exp=xxx.c

## 一些有用的链接
https://lists.gnu.org/archive/html/dejagnu/2014-03/msg00005.html

https://www.embecosm.com/appnotes/ean8/ean8-howto-dejagnu-1.0.html

https://gcc.gnu.org/simtest-howto.html

https://dmalcolm.fedorapeople.org/gcc/newbies-guide/working-with-the-testsuite.html

## Other
