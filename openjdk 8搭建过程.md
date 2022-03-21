## openjdk-8的编译完整过程

#### 环境说明
- ubuntu18.40 
- linux-内核:5.4.0-104-generic
- make:4.1
- gcc/g++:4.8.5

#### 源码方式获取
git上自己找，如果这个步骤搞不定就没必要往下搞了，先看看官网，网上找几篇文章先搞懂理念再看源码[引-范志毅]，哈哈。

#### bootjdk
根据源码的顶级目录中的README-builds.html文档说明，bootjdk需要jdk7u的或者最新的jdk版本（其实去oracle官网下好像也就只有这个版本），这个玩意儿主要是用于下编译中参数 --with-boot-jdk。

#### 编译过程
- uset JAVA_HOME，这个步骤是官方推荐的，最好在终端执行下，避免没有必要的问题。
- 提前安装一下工具：
        sudo apt-get install ccache
        sudo apt-get install libfreetype6-dev
        sudo apt-get install libcups2-dev
        sudo apt-get install libx11-dev libxext-dev libxrender-dev libxrandr-dev libxtst-dev libxt-dev
        sudo apt-get install libasound2-dev
        sudo apt-get install libffi-dev
        sudo apt-get install autoconf
- sh ./configure --with-boot-jdk=/home/why/source/java/java8/bootjdk/jdk-7u80-linux-x64/jdk1.7.0_80 --with-freetype-include=/usr/include/freetype2 --with-freetype-lib=/usr/lib/x86_64-linux-gnu/。
- make

#### 你可能需要修改的文件
- /hotspot/make/linux/Makefile 第236行,添加4%,5%的linux内核版本检验（依葫芦画瓢）
- hotspot/make/linux/makefiles/gcc.make 第200行 -Werror改成Wno-all
- hotspot/make/linux/makefiles/adjust-mflags.sh
	网上很多说法是：
    约第67行增加一个I，修改
	s/-\([^			|][^       ]*)~（后面省略了），可以自己网上再查查
    
   而我试很多回都没有成功，试了一个办法直接把这一行直接干掉就行了。
   
####  最后的验证
到build/linux-x86_64-normal-server-release/jdk/bin目录下，执行./java -version