# 常用指令

查询ip地址： ifconfig

终止ping命令 :ctrl +c

**exit** :  退出root用户

**#**：  在Ubuntu编辑模式里面是注释命令

**cd** :  进入某一级目录使用      比如进入usr    <u>cd /usr    目录前面必须加斜杠</u>

==**/**==          代表==根目录==           无论身处哪个目录，cd   /filename（根目录下面的目录）/.......           想进入根目录下面的目录，总能跨越进入如下图。

![image-20220630152556338](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220630152556338.png)



**从当前目录进入其子目录时，不需要加斜杠。**

例子：

![image-20220704172002052](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220704172002052.png)

比如这里我输入cd /07   按下tab键并不会生成全目录。则说明有问题。

**pwd**   ：显示当前路径

**file**:     比如file +hello   查看hello文件格式

**cp** ：    用法： cp  [选项]  源文件  目标文件      主要用来复制文件和目录

cp  +  -r  +  filename   +   target_filename      -r代表递归复制，用于复制目录。

**df -h**  :     文件系统  容量  已用  可用  已用占比  ==挂载点== (列标题) df -h

**chmod** :     chmod +x就是赋予用户文件的执行权限

**sudo chmod 777 filename**             777打开文件夹的写入权限。



**make mrproper**：     清理旧的编译生成的文件及其他配置等文件

**ls-la** ::   ls -la  filename     是列出==<u>当前目录中</u>==的所有文件和目录，包括隐藏文件和目录

**lsmod** ：     显示已经加载到内核中的模块的状态信息  

**./**:     代表==当前目录==     处在什么目录下就在什么目录下。

**mkdir** :     mkdir  filename      在当前目录下创建名为filename的文件夹。

mkdir -p   filename1/filename2/filename3       在当前目录下创建递归目录。

**rm**   :   -r表示向下递归删除           sudo   -r    递归子目录     **删除文件夹用这个**

​			-f表示直接强制删除，没有任何提示           sudo -f   ==文件==        -f只能强制删除文件，不能强制删除文件夹。

​			-rf    对于文件夹的删除==一般用rm -rf==  （**==文件夹删除==**必须有**r**，递归删除）

**Ctrl  +  Shift  +  +**              增大字体

**Ctrl  +  - ：**        缩小字体

**tar命令详解**：     tar命令是类Linux中常用的==解压    与   压缩命令==。

可以使用命令 (man tar) 命令来进行查看tar的基本命令。下面举例说明一下tar 的基本命令。

==1、tar        -cvf             sysconfig.tar          /etc/sysconfig==                被压缩的文件放在当前目录下。

===								压缩后的文件名.tar       被压缩的文件目录==

命令解释：**将目录/etc/sysconfig/目录下的文件打包成文件sysconfig.tar文件，并且放在当前目录中**

（可以使用pwd命令查看当前路径，可以使用ls命令来查看当前文件夹）参数解释如下：

-c ==创建==新的文档。										===       -c     在压缩文件时使用==

**-v 显示详细的tar处理的文件信息**               这里可加可不加

-f 要操作的文件名

==2、tar                    -rvf                              sysconfig.tar                            /etc/sysconfig/==

===                                                          

命令解释：**将目录/etc/sysconfig/目录下的文件添加到文件sysconfig.tar文件中去**。参数解释如下：

-r 表示增加文件，把要增加的文件追加在压缩文件的末尾。

\#tar -rvf sysconfig.tar /etc/sysconfig/



==3、tar                      -xvf                         sysconfig.tar==

命令解释：**解压文件sysconfig.tar，将压缩文件sysconfig.tar文件解压到当前文件夹内**。参数解释如下：

-x 解压文件。



==tar调用程序进行压缩与解压缩。==

1、tar调用gzip。

**.gz结尾的文件就是调用gzip程序进行压缩的文件，相反文件以.gz结尾的文件需要使用gunzip来进行解压。tar中使用-z参数来调用gzip程序。**在这里通过举例子来进行解释。

==（1）、tar                                   -czvf                                   sysconfig.tar.gz                                    /etc/sysconfig/==

命令解释：将目录/etc/sysconfig/打包成一个tar文件包，通过使用-z参数来调用gzip程序，对目录/etc/sysconfig/进行压缩，

压缩成文件sysconfig.tar.gz，并且将压缩成的文件放在当前文件夹内。参数解释如下：

-z 调用gzip程序来压缩文件，压缩后的文件名称以.gz结尾。

==（2）、tar                            -xzvf                             sysconfig.tar.gz==

命令解释：这条命令是将上一条命令解压。

==2、tar调用bzip2==

.bz2结尾的文件就是调用bzip2程序来进行压缩的文件，相反，文件以.bz2结尾的文件需要使用bunzip2来解压。tar中==使用-j参数==来调用程序bzip2。

==(1)、tar                      -cjvf                          sysconfig.tar.bz2                          /etc/sysconfig/==

命令解释：将/etc/sysconfig/目录打包成一个tar包，接着使用-j参数调用bzip2来进行压缩文件，对目录/etc/sysconfig/进行压缩，压缩成文件sysconfig.tar.bz2并将其放在当前目录下。

==（2）、tar                               -xjvf                                      sysconfig.tar.bz2==

命令解释：解压上一个命令生成的压缩包。
