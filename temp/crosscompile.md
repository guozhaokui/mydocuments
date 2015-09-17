##arm64
linux kernel 的arch中有arm64
##指令集
aarch32,aarch64
*注意:*  linux内核在make的时候，设置cpu架构的方法是 ```ARCH=arm64```, 其他的一般都是 ```--target=```

##交叉编译
[参考这里](http://preshing.com/20141119/how-to-build-a-gcc-cross-compiler/)
1. 为什么需要一个内核？
是为了编译一个target平台的c库。这个c库会把函数调用转成对target平台的系统调用。所有的linux平台都是使用相同的内核。
2. 为什么要一个gcc？
是为了编译一个 aarch64-linux-gcc和 aarch64-linux-g++
3. 各个参数的意思

		1. --build 执行编译的系统，就是本机，一般不用指定。
		2. --host 表示编译完了后，在哪执行，如果与--build不同，就是交叉编译
		3. --target 只有编译交叉编译环境才用到。表示编译出来的编译器生成的代码要执行在target指定的环境上。
4. 
##gcc 
版本：4.9.2
gcc和libc的模块和关系：
![gcc和libc的模块和关系](http://preshing.com/images/cross-gcc-steps.png)

##configure
```--target= ```是目标系统。编译的结果也会以这个开头？
