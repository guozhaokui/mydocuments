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


##过程
```bash
checking host system type... Invalid configuration `aarch64-linux-android': machine `aarch64' not recognized
configure: error: /bin/sh ./config.sub aarch64-linux-android failed

```
/usr/share/automake-1.13/config.sub中有aarch64啊

http://git.savannah.gnu.org/gitweb/?p=config.git;a=tree
到这里更新一下


##toolchain和abi的对应关系
  aarch64-linux-android-4.9:  arm64-v8a
  aarch64-linux-android-clang3.4:  arm64-v8a
 aarch64-linux-android-clang3.5:  arm64-v8a
arm-linux-androideabi-4.6:  armeabi armeabi-v7a armeabi-v7a-hard
arm-linux-androideabi-4.8:  armeabi armeabi-v7a armeabi-v7a-hard
arm-linux-androideabi-4.9:  armeabi armeabi-v7a armeabi-v7a-hard
arm-linux-androideabi-clang3.4:  armeabi armeabi-v7a armeabi-v7a-hard
arm-linux-androideabi-clang3.5:  armeabi armeabi-v7a armeabi-v7a-hard
mips64el-linux-android-4.9:  mips64
mips64el-linux-android-clang3.4:  mips64
mips64el-linux-android-clang3.5:  mips64
mipsel-linux-android-4.6:  mips
mipsel-linux-android-4.8:  mips
mipsel-linux-android-4.9:  mips
mipsel-linux-android-clang3.4:  mips
mipsel-linux-android-clang3.5:  mips
x86-4.6:  x86
x86-4.8:  x86
x86-4.9:  x86
x86-clang3.4:  x86
x86-clang3.5:  x86
x86_64-4.9:  x86_64
x86_64-clang3.4:  x86_64
x86_64-clang3.5:  x86_64