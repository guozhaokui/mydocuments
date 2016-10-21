
参照 [官方文档](https://crosswalk-project.org/contribute/building_crosswalk/android_build.html)

#  运行环境
ubuntu 16.04 这个可以方便的安装所有的依赖库。
在docker里运行，方便给别人，且不影响别人，不受别人影响。
docker本身缺省是10G空间，xwalk的源码和编译文件太大，所以用挂载的方式放在docker外面，这样也容易访问。
```bash
docker run -it -v /home/layabox/crosswalk:/opt/crosswalk  walkcross_ubuntu /bin/bash
```
    

#  下载（要翻墙）
按照文档。很慢很慢的。我是在win10下翻墙，然后用自带的linux下载的。  
不知道为什么下载完编译的时候发现缺几个库，需要单独下载然后拷贝到相应目录（翻墙）
1. third_party/custom_tabs_client  
    https://chromium.googlesource.com/external/github.com/GoogleChrome/custom-tabs-client  
2. third_party/findbugs    
    https://chromium.googlesource.com/chromium/deps/findbugs
3. jsr-305  
    这个项目已经很久没人维护了，googlecode上的版本库也没了，需要自己从googlecode的备份中找一个出来。也不知道有什么用。
3. android 开发环境  
    android_tools/sdk  
    android_tools/ndk
    缺什么下什么。但是都有了之后，gn依然会报错
    ```
    third_party/android_tools/sdk/extras/google/google_play_services/libproject/google-play-services_lib/res does not exist
    ```  
    因为这个目录在新的android sdk中确实不存在了。临时的解决办法是先从\extras\google\m2repository\com\google\android\gms\play-services\3.1.36 解开一个aar放进去
    
至此 gn gen out/Default 能成功了。

下载安装ant。不过不知道有没有用。  
下载安装JDK。本来看官方文档，说OpenJDK也行，但是在编译 jsr-305的时候会报错。所以还是要安装正规的JDK。由于是ubuntu，所以选择下载tar.gz文件，解开后需要修改.bashrc
```
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_111
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH:/opt/crosswalk/depot_tools

```

### 注意：
不要自己用git命令切换git版本。这样会导致与其他git库不匹配，或者编译不过的情况。
现在的分支是  
HEAD detached at 49772a4
实际就是 origin/crosswalk-23/52.0.2743.116  
例如 xwalk/src就是一个独立的git库，对应的是
fb6e871bf0e7c4f5212bdc814956047b542dff5c

# 编译环境
   1. PATH要指向 depot_tools
   2. 可以执行 src/build/install-build-deps-android.sh 这个脚本来安装所有的依赖项。  
      . 在docker环境下，由于是简版ubuntu，所以需要先安装 sudo， lsb-release。
      . msttcorefonts 这个找不到，可以去掉。
      最终命令是
      ```bash
      sudo apt-get install --reinstall linux-libc-dev:i386 apache2-bin bison cdbs curl devscripts dpkg-dev elfutils fakeroot flex fonts-indic fonts-thai-tlwg g++ g++-5-multilib git-core git-svn gperf language-pack-da language-pack-fr language-pack-he language-pack-zh-hant lib32gcc1 lib32stdc++6 libapache2-mod-php7.0 libasound2 libasound2-dev libatk1.0-0 libav-tools libbluetooth-dev libbrlapi-dev libbrlapi0.6 libbz2-1.0 libbz2-dev libc6 libc6-i386 libcairo2 libcairo2-dev libcap-dev libcap2 libcups2 libcups2-dev libcurl4-gnutls-dev libdrm-dev libelf-dev libexpat1 libffi-dev libffi6 libfontconfig1 libfreetype6 libgbm-dev libgconf2-dev libgl1-mesa-dev libgles2-mesa-dev libglib2.0-0 libglib2.0-dev libglu1-mesa-dev libgnome-keyring-dev libgnome-keyring0 libgtk2.0-0 libgtk2.0-dev libjpeg-dev libkrb5-dev libnspr4 libnspr4-dev libnss3 libnss3-dev libpam0g libpam0g-dev libpango1.0-0 libpci-dev libpci3 libpcre3 libpixman-1-0 libpng12-0 libpulse-dev libpulse0 libsctp-dev libspeechd-dev libspeechd2 libsqlite3-0 libsqlite3-dev libssl-dev libstdc++6 libudev-dev libudev1 libwww-perl libx11-6 libxau6 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxdmcp6 libxext6 libxfixes3 libxi6 libxinerama1 libxkbcommon-dev libxrandr2 libxrender1 libxslt1-dev libxss-dev libxt-dev libxtst-dev libxtst6 mesa-common-dev openbox patch perl php7.0-cgi pkg-config python python-cherrypy3 python-crypto python-dev python-numpy python-opencv python-openssl python-psutil python-yaml realpath rpm ruby subversion ttf-dejavu-core wdiff zip zlib1g
      ```
      对于这个简单ubuntu还需要安装 lib32z1， 否则android sdk无法正常工作，提示找不到 libz.so.1
      ```bash
      sudo apt-get install lib32z1
      ```  
      大约需要下载1个多G。

  3. 需要修改 src/BUILD.gnw文件，否则 xwalk编译不过（不知道）
    在280行
    ``` 
        if (is_android) {
            deps += [
    ```  
    这个部分之后加入：
    ```
      "//components/web_contents_delegate_android:web_contents_delegate_android",
      "//components/auto_login_parser",
      "//components/navigation_interception",    
    ```
    否则会连接错误。

先 ninja -C out/Default xwalk
    这个生成要给xwalk的可执行文件。
再 ninja -C out/Default xwalk_core_library  
    这个可以生成xwalk的webview。在 out/Default/xwalk_core_library目录下。

# docker相关
1. 导入
```bash
cat ubuntu.tar | docker import - crosswalk_ubuntu
```

2. 问题
    1. 总是提示 Cannot connect to the Docker daemon  
    需要把用户加到docker组中  
    ```bash
    sudo usermod -aG docker $(whoami)
    ```
    需要logout