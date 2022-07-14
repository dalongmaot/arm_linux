## linux操作系统家目录设置

linux操作系统可以设置自己的家目录。

比如韦东山老师的pc端虚拟机的Ubuntu系统，在进入系统之后他是处在/home/book目录下的，并非处在根目录下。如下图

![image-20220713114628464](https://raw.githubusercontent.com/dalongmaot/arm_linux/main/imgs/image-20220713114628464.png)

下面这个图的home就是自己设置进入系统后的家目录，其实是在/home/book下面的

![image-20220713115111183](https://raw.githubusercontent.com/dalongmaot/arm_linux/main/imgs/image-20220713115111183.png)



#### 如何找到根目录呢

可以通过指令进入根目录，也可以通过文件界面    other locations  -->    computers进入，如下图

![image-20220713115234372](https://raw.githubusercontent.com/dalongmaot/arm_linux/main/imgs/image-20220713115234372.png)





#### [Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020)终端无法使用复制粘贴（解决办法+快捷键设置）

解决方案：[解决方案]([Ubuntu](https://so.csdn.net/so/search?q=Ubuntu&spm=1001.2101.3001.7020)终端无法使用复制粘贴（解决办法+快捷键设置）)

这个方案里面在执行完这一步（1|$  sudo apt-get install open-vm-tools ）之后，完成了Ubuntu系统在vmware里面全屏显示。



#### ubuntu默认使用root用户登录的方法

[解决方案链接](http://t.csdn.cn/8QsLi)



###如果已经将Ubuntu的nfs  挂载到开发板上面去了，会报如下错误

![image-20220628123418339](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220628123418339.png)

#### 在ubuntu中编辑虚拟网络配置时将其还原了，导致在network里面找不到网卡信息，ifconfig也查询不到网卡信息，但是ifconfig -a可以看到网卡信息、

解决方案：

[解决链接](http://t.csdn.cn/wrw8w)



#### ubuntu  使用ifconfig只显示lo或者 能查看到网卡信息但不显示ip地址 的解决方法

[解决链接](https://blog.csdn.net/u012082566/article/details/122665702)



### 所有设置已经配置好，开发板和Ubuntu   ping  不通笔记本   

解决办法：关闭防火墙。



### can notexecute binary file：Execformat error

![image-20220629171843752](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220629171843752.png)

出现这种问题，大概率是因为你的程序是使用x86架构下的编译器编译出来的，想在arm架构下板子上执行该程序，是不行的，要使用交叉编译工具链去编译该程序才行。



### 不同颜色文件在Ubuntu中表示的含义

![image-20220630095636174](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220630095636174.png)

绿色代表可执行文件。



![image-20220630151054249](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220630151054249.png)

荧光蓝色代表是文件夹。文件夹删除必须用到   -r



![image-20220714154712660](https://raw.githubusercontent.com/dalongmaot/arm_linux/main/imgs/image-20220714154712660.png)

红色代表压缩文件。





**在韦东山老师制作的Ubuntu镜像中，登陆系统后默认登陆到/home/book目录下面。**



### linux中挂载详解

**挂在概念** ：  Linux中的根目录以外的文件要想被访问，需要**将其“关联”到**==根目录下==的某个目录来实现，这种关联操作就是“挂载”，这个目录就是“挂载点”



###cp: omitting directory

是因为拷贝的目录底下还有子文件（即递归目录），因此要加上-r选项，强制“递归持续复制，用尽目录的复制行为。





