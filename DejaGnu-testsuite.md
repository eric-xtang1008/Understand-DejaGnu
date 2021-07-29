复盘 DejaGnu testsuite
=========================

当利用DejaGnu做测试的时候如果遇到失败，那么怎么去复现这个测试，也就是说我们怎么把这个测试单独提出来，手动测试一边，以便于我们查找问题。

在终端我们看到这样情况：  

	Running /home/eric.tang/riscv-gnu-toolchain/riscv-gcc/gcc/testsuite/gcc.dg/dg.exp ...                                                                                                                                   
	XPASS: gcc.dg/attr-alloc_size-11.c missing range info for signed char (test for warnings, line 50)                                                                                                                               
	XPASS: gcc.dg/attr-alloc_size-11.c missing range info for short (test for warnings, line 51)                                                                                                                                     
	FAIL: gcc.dg/attr-ifunc-1.c execution test                                                                                                                                                                                       
	FAIL: gcc.dg/attr-ifunc-3.c execution test                                                                                                                                                                                       
	FAIL: gcc.dg/attr-ifunc-4.c execution test                                                                                                                                                                                       
	FAIL: gcc.dg/attr-ifunc-5.c execution test                                                                                                                                                                                       
	FAIL: gcc.dg/cleanup-10.c execution test                                                                                                                                                                                         
	FAIL: gcc.dg/cleanup-11.c execution test                                                                                                                                                                                         
	FAIL: gcc.dg/cleanup-8.c execution test  


### 1. 我们找到保存的log   

	build-gcc-linux-stage2/gcc/testsuite/gcc/gcc.log

### 2. 打开gcc.log查看log 【以gcc.dg/cleanup-11.c 为例】   

	153742 Executing on host: /home/eric.tang/riscv-gnu-toolchain/_build/build-gcc-linux-stage2/gcc/xgcc -B/home/eric.tang/riscv-gnu-toolchain/_build/build-gcc-linux-stage2/gcc/ /home/eric.tang/riscv-gnu-toolchain/riscv-gcc/gcc/testsuite/gcc.dg/cleanup-11.c  -march=rv64gcb -mabi=lp64d -mcmodel=medany   -fno-diagnostics-show-caret -fno-diagnostics-show-line-numbers -fdiagnostics-color=never  -fdiagnostics-urls=never          -fexceptions -fnon-call-exceptions -O2      -lm  -o ./cleanup-11.exe    (timeout = 600)                                                                                                                                  
	153743 spawn -ignore SIGHUP /home/eric.tang/riscv-gnu-toolchain/_build/build-gcc-linux-stage2/gcc/xgcc -B/home/eric.tang/riscv-gnu-toolchain/_build/build-gcc-linux-stage2/gcc/ /home/eric.tang/riscv-gnu-toolchain/riscv-gcc/gcc/testsuite/gcc.dg/cleanup-11.c -march=rv64gcb -mabi=lp64d -mcmodel=medany -fno-diagnostics-show-caret -fno-diagnostics-show-line-numbers -fdiagnostics-color=never -fdiagnostics-urls=never -fexceptions -fnon-call-exceptions -O2 -lm -o ./cleanup-11.exe^M                                                                                                                                                             
	153744 PASS: gcc.dg/cleanup-11.c (test for excess errors)                                                                                                                                                                        
	153745 spawn riscv64-unknown-linux-gnu-run ./cleanup-11.exe^M                                                                                                                                                                    
	153746 /home/eric.tang/riscv-gnu-toolchain/_build/../scripts/wrapper/qemu/riscv64-unknown-linux-gnu-run: line 15: 3262064 Aborted(core dumped) qemu-riscv$xlen -r 5.10 "${qemu_args[@]}" -L ${RISC_V_SYSROOT} "$@"^M                                                                                                                                                                                                             
	153747 FAIL: gcc.dg/cleanup-11.c execution test

### 3. 根据以上log，在终端重新执行
	$ /home/eric.tang/starfive/riscv-gnu-toolchain/_build/build-gcc-linux-stage2/gcc/xgcc -B/home/eric.tang/starfive/riscv-gnu-toolchain/_build/build-gcc-linux-stage2/gcc/ /home/eric.tang/starfive/riscv-gnu-toolchain/riscv-gcc/gcc/testsuite/gcc.dg/cleanup-11.c  -march=rv64gcb -mabi=lp64d -mcmodel=medany   -fno-diagnostics-show-caret -fno-diagnostics-show-line-numbers -fdiagnostics-color=never  -fdiagnostics-urls=never          -fexceptions -fnon-call-exceptions -O2      -lm  -o ./cleanup-11.exe 

	$ qemu-riscv64 cleanup-11.exe
	Aborted (core dumped)

### 4. 查找具体原因
