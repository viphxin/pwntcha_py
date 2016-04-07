# pwntcha_py
PWNTCHA - Pretend We're Not a Turing Computer but a Human Antagonist

This is research software. It comes with absolutely no warranty.

Papers:
http://yann.lecun.com/exdb/publis/index.html

sudo apt-get install libsdl1.2-dev libsdl1.2debian
sudo apt-get install libsdl1.2-dev(比较大，10M左右)
sudo apt-get install libsdl-image1.2-dev

sudo apt-get install libsdl-mixer1.2-dev

sudo apt-get install libsdl-ttf2.0-dev

sudo apt-get install libsdl-gfx1.2-dev

2.安装imlib2

sudo apt-get install libimlib2

sudo apt-get install libimlib2-dev

3.sudo apt-get install opencv,大概60M
#####################################################################

1.1、安装imlib2-version.tar.gz 
1.2、安装SDL-version.tar.gz 
1.3、安装SDL_image-version.tar.gz 
1.4、安装opencv-version.tar.gz 
2、编译pwntcha 

#make
sudo apt-get install libimlib2-dev
思路：就是要先生成configure文件，然后在有configure产生makefile
参考：autoscan，aclocal,autoconf，automake

大概步骤：(假设工作目录为captcha/)
#1, 进入captcha 目录；
#2, 执行autoscan;
#3, 执行aclocal；
#4, 执行autoconf；
#5, 将/usr/share/automake-1.x 目录拷贝到当前目录下作为 .auto目录
    # cp /usr/share/automake-1.x .auto
#6, 新建两个空文件: config.h.in INSTALL,目的仅仅是减少警告
    # touch config.h.in INSTALL
#7, 执行automake; 然后你就得到了configure文件；接下来就和大多数开源项目编译一样了
#8, 执行 ./configure 产生Makefile
#9, make

GO0D LUCK!

////可选./bootstrap ./configure make

!!!!!!!!!!!!!!!
make distclean
./configure LIBS="-lm"
make

里面说：

gcc resolves symbols in the order listed. You need to patch the build system to move `imlib2-config --libs` to the very end of the link command.

根据以下的解决方法，修改./src目录下的makefile文件，更改如下：

查找imlib2-config --libs：

imaging_ldflags = `imlib2-config --libs`

查找imaging_ldflags ：

pwntcha_LDFLAGS = $(imaging_ldflags)

查找pwntcha_LDFLAGS :

1.把

pwntcha_LINK = $(CCLD) $(pwntcha_CFLAGS) $(CFLAGS) $(pwntcha_LDFLAGS) \
        $(LDFLAGS) -o $@

改成：
pwntcha_LINK = $(CCLD) $(pwntcha_CFLAGS) $(CFLAGS) \

        $(LDFLAGS) -o $@

2.在

$(AM_V_CCLD)$(pwntcha_LINK) $(pwntcha_OBJECTS) $(pwntcha_LDADD) $(LIBS) 

后添加

$(pwntcha_LDFLAGS)

这样就把`imlib2-config --libs`放到了gcc的最后面，然后编译通过。
