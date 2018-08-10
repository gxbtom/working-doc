# working-doc
# NDK: Makefile编译Hello World

##1.配置环境变量
添加make工具path环境变量:

E:\Android\android-ndk-r10b\prebuilt\windows-x86_64\bin 

##2.编写Hello World
新建hello.c 

	#include <stdio.h>

	int main(int argc, char* argv[])
	{
	  printf("Hello Android!\r\n");
	  return 0;
	}

##3.编写makefile文件如下
	#ndk根目录
	NDK_ROOT=E:\Android\android-ndk-r10b
	
	#编译器根目录
	TOOLCHAINS_ROOT=$(NDK_ROOT)/toolchains/arm-linux-androideabi-4.6/prebuilt/windows-x86_64
	
	#编译器目录
	TOOLCHAINS_PREFIX=$(TOOLCHAINS_ROOT)/bin/arm-linux-androideabi
	
	#头文件搜索路径
	TOOLCHAINS_INCLUDE=$(TOOLCHAINS_ROOT)/lib/gcc/arm-linux-androideabi/4.6/include-fixed
	
	#SDK根目录
	PLATFROM_ROOT=$(NDK_ROOT)/platforms/android-14/arch-arm
	
	#sdk头文件搜索路径
	PLATFROM_INCLUDE=$(PLATFROM_ROOT)/usr/include
	
	#sdk库文件搜索路径
	PLATFROM_LIB=$(PLATFROM_ROOT)/usr/lib
	
	#文件名称
	MODALE_NAME=hello
	
	#删除
	RM=del
	
	#编译选项
	FLAGS=-I$(TOOLCHAINS_INCLUDE) \
	      -I$(PLATFROM_INCLUDE)   \
	      -L$(PLATFROM_LIB) \
	      -nostdlib \
	      -lgcc \
	      -Bdynamic \
	      -lc
	
	#所有obj文件
	OBJS=$(MODALE_NAME).o \
	     $(PLATFROM_LIB)/crtbegin_dynamic.o \
	     $(PLATFROM_LIB)/crtend_android.o 
	
	#编译器链接
	all:
	    $(TOOLCHAINS_PREFIX)-gcc $(FLAGS) -c $(MODALE_NAME).c -o $(MODALE_NAME).o
	    $(TOOLCHAINS_PREFIX)-gcc $(FLAGS) $(OBJS) -o $(MODALE_NAME)
	
	#删除所有.o文件
	clean:
	    $(RM) *.o
	
	#安装程序到手机
	install:
	    adb push $(MODALE_NAME) /data/local/tmp
	    adb shell chmod 755 /data/local/tmp/$(MODALE_NAME)
	    adb shell /data/local/tmp/$(MODALE_NAME)
	
	#运行程序
	run:
	    adb shell /data/local/tmp/$(MODALE_NAME) 

##4. 使用make命令便可以编译程序
![](md_images/example_dir.png)

生成一个可以执行文件hello   一个目标文件hello.o
于是有编译NDK的命令:
	// 编译目标文件
	E:\Android\android-ndk-r10b/toolchains/arm-linux-androideabi-4.6/prebuilt/windows-x86_64/bin/arm-linux-androideabi-gcc -IE:\Android\android-ndk-r10b/toolchains/arm-linux-androideabi-4.6/prebuilt/windows-x86_64/lib/gcc/arm-linux-androideabi/4.6/include-fixed -IE:\Android\android-ndk-r10b/platforms/android-14/arch-arm/usr/include -LE:\Android\android-ndk-r10b/platforms/android-14/arch-arm/usr/lib -nostdlib -lgcc -Bdynamic -lc -c hello.c -o hello.o
	
	// 编译可以执行文件
	E:\Android\android-ndk-r10b/toolchains/arm-linux-androideabi-4.6/prebuilt/windows-x86_64/bin/arm-linux-androideabi-gcc -IE:\Android\android-ndk-r10b/toolchains/arm-linux-androideabi-4.6/prebuilt/windows-x86_64/lib/gcc/arm-linux-androideabi/4.6/include-fixed -IE:\Android\android-ndk-r10b/platforms/android-14/arch-arm/usr/include -LE:\Android\android-ndk-r10b/platforms/android-14/arch-arm/usr/lib -nostdlib -lgcc -Bdynamic -lc hello.o E:\Android\android-ndk-r10b/platforms/android-14/arch-arm/usr/lib/crtbegin_dynamic.o E:\Android\android-ndk-r10b/platforms/android-14/arch-arm/usr/lib/crtend_android.o  -o hello

##5.运行程序
同样使用makefile即可

![](md_images/make_install.png)

使用make install即可安装运行我们的程序
使用make run即可运行我们的程序
 
install和run 在makefile可以理解成自定义函数


测试通过代码:
[文件详见](./makefile)





