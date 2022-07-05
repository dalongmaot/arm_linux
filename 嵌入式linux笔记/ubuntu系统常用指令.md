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
