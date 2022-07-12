# 第二篇 环境搭建、Linux基本操作、工具使用、开发板初级操作

#### 编译led模块

在使用make编译命令时，会出现如下错误

![image-20220628165430504](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220628165430504.png)

为什么编译不了，是因为自己在打开Makefile文件进行配置时，写成了  vim makefile  在文件夹里==多生成了一个makefile文件==。将该文件删除之后make可以编译。如下图。

![image-20220628172114072](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220628172114072.png)

**make命令在执行时，会默认去当前目录下去寻找名为makefile或者Makefile的文件，如果两个都存在，它直接啥也不顾，表示找不到，直接不工作了**

# 第4篇 嵌入式Linux应用开发基础知识

## 第一章 HelloWorld背后没那么简单

关于mount挂载目录使用方法

比如一个优盘你想使用电脑访问它，当然要把他挂载上去才能访问。

![image-20220629165121866](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220629165121866.png)

上图就是一个挂载目录，服务器的目录也可以看成一个设备。

例如：mount -t nfs -o nolock,vers=3 ==192.168.5.11:/home/book/nfs_rootfs== /mnt

将Ubuntu下面的黄色目录挂载到开发板的mnt目录下面。

***挂载在开发板下的目录是和Ubuntu的nfs_rootfs实时同步的***

例如：我在Ubuntu根目录下有一个hello程序，此时程序并未上传到Ubuntu的nfs_rootfs挂载设备（目录）下，但是在开发板上已经执行了将nfs_rootfs设备挂载在开发板/mnt目录下，后面将hello程序上传到  nfs_rootfs  目录下，不需要再次执行挂载命令，开发板也会同步

#### 在运行程序时，必须要在该程序根目录下进行运行，否则会报错。如下截图（**对比图**）。

![image-20220629172932767](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220629172932767.png)



## 第三章 Makefile的使用

### 3.1Makefile的理解

你在当前目录下使用make命令时，make命令会找到当前目录下的Makefile文件，按照Makefile文件里面的语句进行执行。



### 3.2Makefile语法知识。

目标文件 ==：==  依赖文件

<tab> 命令                                    



**$^**:       可以表示所有的依赖文件。

**$@**:表示目标文件             **$<:**表示依赖文件



##  a. 通配符

假如一个目标文件所依赖的依赖文件很多，那样岂不是我们要写很多规则，这显然是不合乎常理的

我们可以使用通配符，来解决这些问题。

我们对上节程序进行修改代码如下：

```bash
test: a.o b.o 
	gcc -o test $^
	
%.o : %.c
	gcc -c -o $@ $<
```

%.o：表示所用的.o文件

%.c：表示所有的.c文件

\$@：表示目标

\$\<：表示第1个依赖文件

\$\^：表示所有依赖文件

我们来在该目录下增加一个 c.c 文件，代码如下：

```bash
#include <stdio.h>

void func_c()
{
	printf("This is C\n");
}
```

然后在main函数中调用修改Makefile，修改后的代码如下：

```bash
test: a.o b.o c.o
	gcc -o test $^
	
%.o : %.c
	gcc -c -o $@ $<
```

执行： 

```bash
make
```

结果：

```bash
gcc -c -o a.o a.c
gcc -c -o b.o b.c
gcc -c -o c.o c.c
gcc -o test a.o b.o c.o
```

运行：

```bash
./test
```

结果：

```bash
This is B
This is C
```

## b. 假想目标: .PHONY

1.我们想清除文件，我们在Makefile的结尾添加如下代码就可以了：

```bash
clean:
	rm *.o test
```

*1）执行 make ：生成第一个可执行文件。
*2）执行 make clean : 清除所有文件，即执行： rm \*.o test。

make后面可以带上目标名，也可以不带，如果不带目标名的话它就想生成第一个规则里面的第一个目标。

2.使用Makefile

执行：**make [目标]** 也可以不跟目标名，若无目标默认第一个目标。我们直接执行make的时候，会在makefile里面找到第一个目标然后执行下面的指令生成第一个目标。当我们执行 make clean 的时候，就会在 Makefile 里面找到 clean 这个目标，然后执行里面的命令，这个写法有些问题，原因是我们的目录里面没有 clean 这个文件，这个规则执行的条件成立，他就会执行下面的命令来删除文件。

如果：该目录下面有名为clean文件怎么办呢？

我们在该目录下创建一个名为 “clean” 的文件，然后重新执行：make然后make
clean，结果(会有下面的提示：)：

```bash
make: \`clean' is up to date.
```

它根本没有执行我们的删除操作，这是为什么呢？

我们之前说，一个**规则能过执行的条件**：

*1)目标文件不存在
*2)依赖文件比目标新

现在我们的目录里面有名为“clean”的文件，目标文件是有的，并且没有

依赖文件，没有办法判断依赖文件的时间。这种写法会导致：有同名的"clean"文件时，就没有办法执行make clean操作。解决办法：我们需要把目标定义为假象目标，用关键子PHONY

```bash
.PHONY: clean //把clean定义为假象目标。他就不会判断名为“clean”的文件是否存在，
```

然后在Makfile结尾添加.PHONY: clean语句，重新执行：make clean，就会执行删除操作。

## C. 变量

在makefile中有两种变量：

1), 简单变量(即使变量)：

A := xxx    # A的值即刻确定，在定义时即确定

对于即使变量使用 “:=” 表示，它的值在定义的时候已经被确定了

2）延时变量

B = xxx   # B的值使用到时才确定

对于延时变量使用“=”表示。它只有在使用到的时候才确定，在定义/等于时并没有

确定下来。

想使用变量的时候使用“\$”来引用，如果不想看到命令是，可以在命令的前面加上"\@"符号，就不会显示命令本身。当==我们执行make命令的时候，make这个指令本身，会把整个Makefile读进去，进行全部分析，然后解析里面的变量==。常用的变量的定义如下：

```bash
:= # 即时变量
= # 延时变量
?= # 延时变量, 如果是第1次定义才起效, 如果**在前面该变量已定义**则忽略这句
\+= # 附加, 它是即时变量还是延时变量取决于前面的定义
?=: 如果这个变量在前面已经被定义了，这句话就会不会起效果，
```

实例：

```bash
A := $(C)
B = $(C)
C = abc

#D = 100ask
D ?= weidongshan

all:
	@echo A = $(A)
	@echo B = $(B)
	@echo D = $(D)

C += 123
```

执行：

```bash
make
```

结果：

```bash
A =
B = abc 123
D = weidongshan
```

分析：

1) A := \$(C)：

A为即使变量，在定义时即确定，由于刚开始C的值为空，所以A的值也为空。

2) B = \$(C)：
   B为延时变量，只有使用到时它的值才确定，当执行make时，会解析Makefile里面的所用变量，所以先解析C= abc,然后解析C += 123，此时，C = abc 123，当执行：\@echo B = \$(B) B的值为 abc 123。

3) D ?= weidongshan：

D变量在前面没有定义，所以D的值为weidongshan，如果在前面添加D = 100ask，最后D的值为100ask。

我们还可以通过命令行存入变量的值 例如：

执行：make D=123456 里面的 D ?= weidongshan 这句话就不起作用了。

结果：

```bash
A =
B = abc 123
D = 123456
```



# Makefile函数

makefile里面可以包含很多函数，这些函数都是make本身实现的，下面我们来几个常用的函数。引用一个函数用“\$”。

函数语法结构：$(<function> <arguments>)



## 函数foreach

函数foreach语法如下： 

```bash
$(foreach var,list,text) 
```

前两个参数，‘var’和‘list’，将首先扩展，注意最后一个参数 ‘text’ 此时不扩展；接着，对每一个 ‘list’ 扩展产生的字，将用来为 ‘var’ 扩展后命名的变量赋值；然后 ‘text’ 引用该变量扩展；因此它每次扩展都不相同。结果是由空格隔开的 ‘text’。在 ‘list’ 中多次扩展的字组成的新的 ‘list’。‘text’ 多次扩展的字串联起来，字与字之间由空格隔开，如此就产生了函数 foreach 的返回值。

实例：

```bash
A = a b c
B = $(foreach f, &(A), $(f).o)

all:
	@echo B = $(B)
```

结果：

```bash
B = a.o b.o c.o
```

## 函数filter/filter-out

函数filter/filter-out语法如下：

```bash
$(filter pattern...,text)     # 在text中取出符合patten格式的值
$(filter-out pattern...,text) # 在text中取出不符合patten格式的值
```

实例：

```bash
C = a b c d/

D = $(filter %/, $(C))
E = $(filter-out %/, $(C))

all:
        @echo D = $(D)
        @echo E = $(E)
```

结果：

```bash
D = d/
E = a b c
```

## Wildcard

函数Wildcard语法如下：

```bash
$(wildcard pattern) # pattern定义了文件名的格式, wildcard取出其中存在的文件。
```

这个函数 wildcard 会以 pattern 这个格式，去寻找存在的文件，返回存在文件的名字。

实例：

在该目录下创建三个文件：a.c b.c c.c

```bash
files = $(wildcard *.c)

all:
        @echo files = $(files)
```

结果：

```bash
files = a.c b.c c.c
```

我们也可以用wildcard函数来判断，真实存在的文件

实例：

```bash
files2 = a.c b.c c.c d.c e.c  abc
files3 = $(wildcard $(files2))

all:
        @echo files3 = $(files3)
```

结果：

```bash
files3 = a.c b.c c.c
```

## patsubst函数

函数 patsubst 语法如下：

```bash
$(patsubst pattern,replacement,\$(var))
```

patsubst 函数是从 var 变量里面取出每一个值，如果这个符合 pattern 格式，把它替换成 replacement 格式，

实例：

```bash
files2  = a.c b.c c.c d.c e.c abc

dep_files = $(patsubst %.c,%.d,$(files2))

all:
        @echo dep_files = $(dep_files)

```

结果：

```bash
dep_files = a.d b.d c.d d.d e.d abc
```



## 第五章  Framebuffer应用编程

常见的24位颜色表。一般像素的位数bpp = 32位，但是前8位不用。

![image-20220704185026280](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220704185026280.png)

##### 32  位RGB - > 16位RGB颜色转换。

![image-20220704194347256](C:\Users\tianfuqiang\AppData\Roaming\Typora\typora-user-images\image-20220704194347256.png)

上面图片就是32位转16位颜色图的方法。



#### 字符数组是如何储存汉字的

基本概念：**汉字存储占用空间大小与使用何种编码方式有关。**

常见的中文编码 GB2312（国标简体中文字符集）和 GBK（国标扩展）使用 2 个字节编码来表示一个汉字，不常用的 GB18030 使用 4 个字节编码来表示一个汉字，更通用的 UTF-8 编码使用 3 个字节编码来表示一个汉字。

**大概有以下几种情况**

**1、使用数组存储1个汉字**

```c
char a[] = "字";    // 定义一个汉字的数组
char a[] = {"字"};  // 也可以加上 { }
这两种情况系统都会在后面自动加上'\0',即a[2] = '\0' 汉字占2个字节a[0]和a[1]；
也可以这样：
char a[3] = "字";
char a[3] = {"字"};
以上两种方法定义的空间是一样大的，都是3个字节。

char i;
for(i=0;i<3;i++)
{
	printf("a[%d] = 0x%x;\r\n",i,a[i]);
}
使用 char 时 output：
a[0] = 0xffffffd7;
a[1] = 0xffffffd6;
a[2] = 0x00;
使用 unsigned char 时output： 
a[0] = 0xd7;
a[1] = 0xd6;
a[2] = 0x00;
```

**2、使用数组存储汉字字符串**

```c
char a[] = "汉字字符串";    // 定义多个汉字的数组
char a[] = {"汉字字符串"};  // 也可以加上 { }
同样系统会在后面自动加上'\0',即 a[10] = '\0' 汉字占10个字节a[0]~a[9]；
也可以这样：
char a[11] = "汉字字符串"; 
char a[11] = {"汉字字符串"};
以上两种方法定义的空间是一样大的，都是11个字节。
```

**3、数组空间大于汉字字符数的情形**

```c
char a[11] = "汉字"; 
char a[11] = {"汉字"};
这种方法定义数组，系统会将 a[5]~a[10] 自动添加'\0'。
```

**4、多维数组的使用**

```c
char a[2][9] = {"多维数组", "这样表示"};     //使用多维数组的情形
char a[2][9] = {{"多维数组"}, {"这样表示"}}; //也可以加 { }
其中：
a[0] = "多维数组";
a[1] = "这样表示";
```

**5、字符指针的使用**

```c
char * ArrayName = "使用字符指针";  // 同样可以加 { }
或：
char * ArrayName;
ArrayName = "使用字符指针";         // 同样可以加 { }

char * ArrayName[2] = {{"字符指针数组"},{"字符指针数组"}};
或：
char * ArrayName[2];
ArrayName[0] = "字符指针数组";     // 同样可以加 { }
ArrayName[1] = "字符指针数组";
```

**6、 数组之间传递汉字**

```c
系统中汉字的输入，一般由键盘输入、文件读取、远程通信等接口函数输入，其本质上还是对字符的传递，下面给出最简单的传递汉字的方法：
char i;
char a[2][9];
char b[9] = {"传递汉字"};
for(i=0;i<9;i++)
{
	a[0][i] = b[i];     // 将 b 数组中的汉字转存到 a[0] 数组中
}
printf("%s",a[0]);

output:传递汉字
```



###如何指定程序源文件以及编译文件的编码格式

我们编写C程序时，可以使用ANSI编码，或是UTF-8编码；在编译程序时，可以使用以下的选项告诉编译器：  

**源文件指定编码格式**

```c
-finput-charset=GB2312
-finput-charset=UTF-8
```

**编译后的文件指定编码格式**

```c
-fexec-charset=GB2312
-fexec-charset=UTF-8
```



### 字段错误（Segmentation Fault）

**可能出现的原因**

（1）、内存越界，数组越界，变量类型不一致等

**一定要注意内存越界问题**！！

之前这里我的代码是：

```c
unsigned char * pen_8 = fb_base + y * var.xres * var.bits_per_pixel + x * var.bits_per_pixel;
```

修改之后。

![image-20220706223608657](https://raw.githubusercontent.com/dalongmaot/arm_linux/main/imgs/image-20220706223608657.png)

因为我是用mmap映射函数映射的那块内存大小有限，你上面pen_8指向越界了。





//这里注意到dots是char类型的，故1个16 * 16位的汉字用char[16][2]就能储存这32个字节点阵的汉字。



















