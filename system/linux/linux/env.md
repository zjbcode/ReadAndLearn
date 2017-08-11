


source /etc/profile
echo $GOPATH

* [Linux 下三种方式设置环境变量_Linux教程_Linux公社-Linux系统门户网站 ](http://www.linuxidc.com/Linux/2015-01/111459.htm)

1、在Windows 系统下，很多软件安装都需要配置环境变量，比如 安装 jdk ，如果不配置环境变量，在非软件安装的目录下运行javac 命令，将会报告找不到文件，类似的错误。

2、那么什么是环境变量？简单说，就是指定一个目录，运行软件的时候，相关的程序将会按照该目录寻找相关文件。 设置变量对于一般人最实用的功能就是： 不用拷贝某些dll文件到系统目录中了，而path 这一系统变量就是系统搜索dll文件的一系列路径

在Linux系统下，如果你下载并安装应用程序，很有可能在键入它的名称的时候出现 “command  not found ” 的提示内容。 如果每次都到安装目录文件夹内，找到可执行文件来进行操作就太繁琐了。 这涉及到环境变量path的设置问题，而Path 的设置也是在Linux下定制环境变量的一个组成部分

Linux下环境变量设置的三种方法：

如想将一个路径加入到$PATH中，可以像下面这样做：


# 控制台中设置

1、控制台中设置，不赞成这种方式，因为他只对当前的shell 起作用，换一个shell设置就无效了：

$PATH="$PATH":/NEW_PATH  (关闭shell Path会还原为原来的path)

# /etc/profile

2、修改 /etc/profile 文件，如果你的计算机仅仅作为开发使用时推存使用这种方法，因为所有用户的shell都有权使用这个环境变量，可能会给系统带来安全性问题。这里是针对所有的用户的，所有的shell

在/etc/profile的最下面添加：  export  PATH="$PATH:/NEW_PATH"

# bashrc

3、修改bashrc文件，这种方法更为安全，它可以把使用这些环境变量的权限控制到用户级别，这里是针对某一特定的用户，如果你需要给某个用户权限使用这些环境变量，你只需要修改其个人用户主目录下的 .bashrc文件就可以了。

在下面添加：

Export  PATH="$PATH:/NEW_PATH"