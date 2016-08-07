## linux下的unix高级编程

1. **入门指南**
2. **编写优质的GNU/Linux 软件**
3. **进程**
4. **线程**
5. **进程间通信**

### 一  入门指南

这章将会介绍创建一个Linux下的c/c++程序的基本步骤，同时这章还会教你怎样去创建，修改和编译c/c++源代码，并且最后调试程序结果。如果你已经对Linux的编程非常熟练，那么你可以跳过这里直接看章节2（编写优质的GNU/Linux软件），此外应该多多留意一下2.3节，“编写和使用库”，对于静态和动态链接你可能知道得并不是很清楚。

针对于这本书，我们假定你熟悉c/c++语言和C标准库的大部分函数。这本书除了在解释C++语言的特性的时候，基他示例源代码均是用C语言来编写的，我们也假定你了解最基本的Linux Shell命令，比如创建目录或者是复制文件之类的。考虑到大多数的Linux 程序员最初的编程环境是在windows上，我们也会偶尔的指出Windows和Linux开发环境的相似和不同之处。

**1.1 使用Emacs来编辑**

你会在Linux系统上你会看到各种样用来编辑代码的编辑器，但是最流行并且功能最全的莫过于GNU Emacs了。

> 关于Emacs
>
>  Emacs已经超越了一个编辑器的范畴，它是一个强大到难以置信的程序，以致于在CodeSourcery上被亲切的称为“ONE True Program”， 即使只是短暂的OTP，你能使用Emacs收发邮件，并且你还能自定义和扩展Emacs，甚至你还能在Emacs里浏览网页。

如果你习惯于使用其他编辑器，毫无疑问，你可以转到Emacs了。在本书的其他部分不会依赖于Emacs，如果你还没有一个非常喜爱的Linux下的编辑器，那么你可以跟着这里给出的这个小指南学习一下。

如果你喜欢Emcas并且希望学习了解到更多的Emcas高级功能，你应该考虑读一本学习Emacs的书了。一个优秀的Emacs学习指南，Debra Cameron写的《学习GNU Emacs》。



 **1.1.1 打开c/c++源文件**

在终端窗口敲下emacs并回车，就能启动eamcs了，启动Emacs之后，可以通过它顶部的菜单项创建一个新的源文件。点击文件菜单，选择打开文件，此时键入你想要的文件名和屏幕下方显示的minibuf，如果你想创建一个C源文件，使用以.c或者.h[头文件]结尾的文件名。同样，想创建C++源文件，则使用以.cpp .cxx .hpp .hxx结尾的文件名，当文件打开后，你就可以像在其他普通文字处理软件中一样进行编辑了，保存文件，选择在文件菜单中的保存缓冲区[1][1]，如果你使用完Emacs，你可以选择文件菜单中的退出Emacs选项。

如果你不喜欢鼠标点击，还可以用键盘快捷键来自动打开，保存文件和退出Emacs。键入C-x C-f打开文件（C-x是指按住Ctrl键再按下x键），键入C-x C-s来保存文件，退出Emacs只需要键入C-x C-c。如果你想更多更好的了解的Emacs，可以在Emacs的帮助菜单中选择Emacs入门指南， 这个入门指南可以为你使用Emacs提供大量的窍门。

​	[1]:如果你没有使用X Window的系统，你可以按F10进入此菜单。



**1.1.2 自动格式化代码**

你可能习惯于在IDE(集成开发环境)下编程，同时你也习惯于用IDE的编辑器帮你格式化源代码，Emacs也提供了相同的功能。打开一个c/c++源代码文件，Emacs就会自动统计文件中包括文本在内的所有的源代码。在文件中的空行敲击tab键，Emacs就会移动光标到一个合适的缩进点，在文件中包含文本内容的行中敲击tab键，Emacs就会为这些文本进行缩进。例如，你键入了如下的文本：

```c
int main()
{
printf(“HelloWord\n”);
}
```

如果你在调用了printf函数的那一行上敲击tab键，Emacs就会重新格式化成像下面的你看到的文本一样：

```c
int main()
{
    printf(“HelloWord\n”);
}
```

我们可以看到对应行的缩进是正确的了。

随着你使用Emacs的次数增多，你会发现它能帮你完成各种各样复杂的格式化代码的工作。如果你想，你可以使用Emacs做任何你能想象到的文本自动格式化的工作。人们已经用这款软件实现了编辑各种各样的文档，还可以在里面玩游戏[2][2]，并且实现了数据库的前端显示，等等。



**1.1.3   语法高亮**

除了能格式化源代码，Emacs还能将C和C++的代码中不同的语法元素用不同的颜色加以区分，可以让我们更加方便的阅读源代码。比如，Emacs能把语言关键字用一种颜色表示，而把内建类型用另一种颜色表示，而注释又是不同于前二者的颜色表示。使用语法高亮颜色可以很容易的定位一些常见的语法错误。

开启语法高亮的方法非常简单，就是编辑~/.emac文件，在其中加上下面这句：

> (globle-font-lock-modet)

保存文件重启Emacs，现在打开一个c/c++文件，尽情享受语法高亮带来的方便吧！

你可能察觉到刚刚加入到.emacs文件中的一串字符看起来像LISP语言，没错！那就是LISP代码。Emacs的大部分功能其实就是LISP实现的。当然你也可以使用LISP代码为你的Emacs增加功能。

[2]: 如果你想玩一款经典的文字冒险游戏，那运行M-x dunnet命令试试吧！ 



**1.2 使用GCC编译**

编译器是把人们可读的源代码转换成机器可读的能实际运行目标代码，Linux系统上编译器的选择就是GUN编译集合的所有部分。常见的比如GCC[3][3]。GCC已经包含了对于C，C++，JAVA，Objective-C，Fortran和Chill等这些语言的编译。这本书重点是对于C和C++的编程。

假定有如清单1.2的c++源文件（reciprocal.cpp）和清单1.1的c源文件（main.c），假设这二个文件被编译和链接生成一个叫reciprotal的程序[4][4]，它用来计算一个整数的倒数。

清单1.1 main.c

----

```c
#include <stdio.h>
#include “reciprocal.hpp”

int main(intargc, char *argv[])
{
    int i;
    i =atoi(atgv[1]);
    printf(“The reciprotal of %d is %g\n”, I,reciprotal(1));
    return 0;
}
```

清单1.2 reciprotal.hpp

----

```c
#include <cassert>
#include “reciprotal.hpp”

doublereciprocal(int i)
{
    //I should be non-zero
    assert(I != 0);
    return 1.0 / I;
}
```

----

[3]: 想更多了解关于GCC的信息， 访问[http://gcc.gun.org](http://gcc.gun.org)

[4]: 在Windows系统中，可执行文件通常都是以.exe结尾的文件，而Linux系统下的程序通常没有扩展名。所以，这个程序在Windows上可能被命名为reciprotal.exe，而Linux的版本，文件名就是reciprotal。

还有一个叫recipratal.hpp的头文件（见清单1.3）       

清单1.3 reciprotal.hpp

----

```c++
#ifdef__cpluscplus
extern “C”{
#endif

externdouble reciprotal(int i);
#ifdef __cpluscplus
}
#endif
```

----

第一步就是将C和C++源代码编译成目标代码。

**1.2.1  编译单个文件**

C的编译器叫做gcc，使用-c选项来编译C源代码文件。例如，输入下面这个命令来快速的编译main.c:

>  % gcc –c main.c

生成的目标文件叫做main.o

C++的编译器叫做g++，它的操作与gcc几乎一样，编译reciprotal.hpp输入以下命令：

> % g++ -creciprotal.hpp

选项-c告诉编译器只生成目标文件，没有这个选项，g++会继续执行链接直到生成可执行文件为止，在你执行了这条命令后，就能得到一个叫recipratal.o的文件。

如果你要构建一个大型的程序，可能就需要结合其他选项一起使用。选项-I通常用来告知GCC搜索头文件的具体位置，默认情况下，gcc会寻找当前目录和标准库安装头文件的目录，如果你想要包含的头文件在其他地方，你就需要使用-I选项来指定。例如，一个工程有一个放源文件的目录叫做src，另一个叫做include，此时你要编译reciprotal.cpp，就应该像下面这样增加../include目录来指示g++寻找头文件reciprotal.hpp：

>  %g++ -c –I ../include reciprotal.cpp 

有时候需要在命令行中定义宏，例如，在产品代码中，你不想因为reciprotal.cpp文件中的断言产生性能开销，它的功能仅仅是帮你调试程序，此时你就可以定义NODEBUG宏来关闭断言检查，此时虽然在reciprotal.cpp加一个显式的#define，但是不会影响源代码本身，像下面这样非常简单的在命令行中定义一个宏：

> % g++ -c –DNODEBUG reciprotal.cpp    

假如你还想为NODEBUG定义几个特殊的值，你就可以像下面这样做

>  %g++ -c –D NODEBUG=3 reciprotal.cpp

在真正构建产品代码的时候，你多半希望GCC将代码优化到运行尽可能的快，这时候你就可以使用-O2这个命令行选项（GCC有若干个不同的优化等级， 第二级优化就适合大部分程序了），比如，像下面将为编译reciprotal.cpp打开优化选项：

> %g++ -c –O2 reciprotal.cpp

需要注意的是，打开优化选项会让程序更难于调试（详情见小节1.4 “使用GDB调试”），同时在大多数情况下，使用优化选项编译会掩盖了程序中的bug从而让你没法提前发现。

你还可以使用更多的gcc和g++编译选项，最好的方法就是获取一份在线帮助文档，然后在命令行中输入如下命令即可：

> %info gcc



**1.2.2  链接目标文件**

现在编译完了main.c和reciprotal.cpp，应该链接它们。如果代码包含C++代码，那么你应该总是使用g++来链接你程序，即使里面包含有C代码。除非程序里只有C代码，那就可以使用gcc代替g++。因为这个程序包含了C和C++的代码所以应该像下面这样使用g++：

> %g++ -o reciprotal main.o reciprotal.o

选项-o后面给出的文件名在链接的时候做为最终生成的文件名，现在可以像下面这样运行reciprotal：

>  %./reciprotal 7
>
>  %The reciprotal of 7 is 0.142857 

如你所见，g++会自动链接实现printf函数的标准C运行库，如果需要链接其他非标准库（比如图形用户界面工具），此时应该使用-l选项指定这个库。在Linux系统下，库的名字通常都会以lib开头，比如，PAM库的名字就是libpam.a，你可以使用下面的命令来链接libpam.a这个库：

> %g++ -o reciprotal  main.o reciprotal.o –lpam

此时编译器就会自动加上lib前缀和.a后缀。

与头文件相同，链接器也会在一些“标准”位置寻找库，包括系统标准库的放置目录/lib和/usr/lib，如果你还想同时搜索其他目录，那么应该加上-L选项。与-L选项相近的是我们更早讨论过的-I选项。你可以使用下面所写的这行命令指示链接器首先查找/usr/local/lib/pam目录，之后再查找常规标准库目录。  

> %g++ -o reciprotal main.o reciprotal.o –L/usr/local/lib/pam –lpam

即使使用-I选项指示预处理器查找当前目录是非必须地，但必须使用-L选项指示链接器搜索当前目录。通常的，使用下面的命令就可以指示链接器在当前目录查找test这个库

> %g++ -o app app.o –L. –ltest



**1.3 使用**GNU Make**自动处理**

如果你习惯在Windows系统上编程，那么你可能习惯使用集成开发环境（IDE），你只管向工程里添加文件，IDE就会自动帮你完成工程的构建。但是即便Linux系统上有许多可用的IDE，但是本书重点不是介绍它们。做为替代方案，本书会教你像大多数Linux程序员那样，使用GNU Make 来自动重新编译代码。

make的基本思想是非常简单的，只需要告诉make你想要构建的目标文件，以及构建这个目标文件所需要的规则。同时你还要指定每个模块目标文件重新构建所需要的文件依赖。

在我们的reciprotal工程中，有三个显而易见的目标文件：reciprotal.o,main.o和reciprotal，在这之前里你已经知道了以命令行的形式构建这些目标文件的规则，至于依赖关系，则需要思考一下，明显的依赖是reciprotal依赖于reciprotal.o和main.o，因为在没有构建各种个目标文件之前是不能成功的完成链接程序的。这些目标文件在源代码文件发生改变的时候会重新构建，就如哪怕reciprotal.hpp文件一点点的更改，都会导致二个目标文件重新构建，原因就是二个源文件都包含了这个头文件。

除了这个明显的目标文件，通常还会有一个clean目标，这个目标会删除所有已经生成的目标文件和可执行程序，以便编译能够更好的重新开始。

这个目标的依赖是使用删除文件的rm命令。

你可以把所有的这些放在一个叫做Makefile的文件里，通过它向make传达信息。以下展示了一个Makefile中应该包含的东西：

```sh
reciprotal:main.o reciprotal.o
	g++ $(CFLAGS) –o reciprotal main.o reciprotal.o
	
main.o:main.c reciprotal.hpp
    gcc $(CFLAGS) –c main.c

reciprotal.o:reciprotal.cpp reciprotal.hpp
    g++ $(CFLAGS) –c reciprotal.cpp

clean:
    rm–f *.o reciprotal
```

可以看到目标被列在左侧，紧接着是一个冒号和一些依赖。构建这个目标的规则写在下一行（暂时忽略$(CFLAGS），规则所在行必须以Tab开头，否则make就会变混乱而找不到规则。如果在Emacs中编辑Makefile，它会帮你自动格式化。

如果你删除了所有已经构建好的目标文件，重新构建只需要输入：

> %make

在命令上会看到如下显示：

```sh
 %make
 gcc –c main.c
 g++ -c reciprotal.cpp
 g++ -o reciprotal main.o reciprotal.o
```

你会看到make已经自动构建目标文件并链接它们，如果此时对main.c做一些细微的无关紧要的更改并重新输入make构建，会看到如下：

```sh
%make
gcc –c main.c 
g++ -o reciprotal reciprotal.o
```

你能发现make知道应该重新构建main.o，重新链接程序，但是它并不会去费心重新编译reciprotal.cpp，原因就是reciprotal.o的依赖没有发生任何变化。

$(CFLAGS)是make中的一个变量，既可以Makefile中定义它，也可以在命令行中定义它。GNU make会在执行规则的时候会把这个变量替换成对应的值，例如，重新编译并打开优化选项，你就可以像下面这样做：

```sh
%make clean
rm–f *.o reciprtal
%make CFLAGS=-O
gcc -O2 –c main.
g++ -O2 –c reciprotal.cp
g++ -O2 –o reciprotal main.o reciprotal.o
```

可以发现在规则里$(CFLAGS)已经被-O2所代替。

在这一小节中，你看到的仅仅是make的最基本的功能，你还以在命令行中输入下面的命令得到更多信息手册：

> %info make

在手册中，可以找到怎样写Makefile更容易维护，如何减少规则数目，如何让Makefile自动推导依赖关系等。

**1.4 使用GNU 调试器调试程序（GDB）**

调试器就是一个帮你找出“为什么你的程序不按照你的逻辑来执行”的程序，当然你还可以用它来做更多的事情[5][5]。一个被大多数Linux程序员使用的调试器就是GNU调试器GDB，你可以使用GDB单步执行代码，设置断点，并且可以查看局部变量的值。

**1.4.1  编译生成调试信息**

你必须在编译的时候开启调试信息，这样才能用GDB调试程序，开启调试信息需要在命令行编译的时候加上-g开关。如果你现在使用的Makefile是之前上文写的，可以在运行make的时候设置CFLAG的值为-g，像下面这样：

```sh
%make CFLAG=-g
gcc –g –c main.c
g++ -g –c recitrotal.cpp
g++ -g –o recipotal main.o reciprotal.o
```

一旦使用-g编译，编译器就会在生成的目标文件和执行文件的时候包含额外的调试信息，调试器使用这些信息就能标识出源文件哪一行对应的哪个地址，打印出局部变量等等。

 

**1.4.2 运行GDB**

输入下边的命令来启动gdb：

> %gdb reciprotal

GDB启动后，可以看到GDB命令提示符:

> (gdb)

[5]: ….除非你的程序总是最先运行

第一步是在调试器里运行你的程序，只需要输入run后跟任意程序参数，试着不带参数的运行这个程序，像下面这样：

```sh
(gdb)run
Startingprogram: reciprotal

Programreceived singal SIGSEGV, Segmention fault.
__strol_internal(nptr=0x0, endptr=0x0, base=10, group=0)
atstrol.c : 287
287  strol.c : No such file or directory

(gdb)
```

问题就出在在main函数中没有错误检查的代码，程序需要一个参数，但是这个例子是不带参数的运行程序，SIGSEGV信息表示程序意外停止，GDB定位到崩溃是在函数strol_internal调用里发生的。那个函数在标准库中，并且源文件没有安装，这就能解释“没有对应文件或者目录”这个错误了，可以使用where命令查看堆栈：

```sh
 (gdb)where
 #0__strol_internal (nptr=0x0, nedptr=0x0, base=10, group=0)
 atstrol.c : 287
 #10x40096fb6 in atoi(nptr=0x0) at ../stdlib/stdlib.h:251
 #20x804863e in main (argc=1, argv=0xbffff5e4) at main.c:8
```

你会发现在上面的显示中main函数调用了atoi并且传递给其一个空指针，这就是问题的原因所在。

可以使用up命令在到达main函数之前向上移动二个栈帧：

```sh
(gdb)up 2
#20x804863e in main (argc=1, argv=0xbffff5e4) at main.c:8
8     I = atoi(argv[1]);
```

可以看出来GDB在main.c源文件中查找功能还是非常强大的，它会显示出函数调用发生错误的行数位置，现在还可以使用print命令查看正在使用变量的值:

```sh
(gdb)print argv[1]
$2= 0x0
```

 这样就更能确认问题原因就是把空指针传给了atoi函数。

你还可以用break命令设置程序断点：

```sh
(gdb)break main
Breakpoint1 at 0x804862e: file main.c, line 8
```

这个命令在main[6][6]函数的第一行设置了一个断点，现在尝试像下面这样带参数重新运行程序：

```sh
(gdb)run 7
Startingprogram: reciprotal 7
Breakpoint1, main (argc=2, argv=0xbffff5e4) at main.c : 8
8     i = atoi(argv[1]);
```

注意调试器已经停在了断点处，现在可以使用next命令单步执行调用atoi函数：

```sh
(gdb)next
9   printf(“The reciprotal of %d is %g\n”, I,reciprotal(i));
```

如果想知道在reciprotal函数里发生了什么，使用step命令如下：

```sh
(gdb)step
reciprtrotal(i = 7) at reciprotal.cpp:6
6     assert(i != 0);
```

现在就是在reciprotal的函数体内了。

你可能会发现在Emacs里使用gdb比直接在命令行里运行gdb更加方便，在Emacs窗口中使用M-x（即alt+x）gdb启动gdb，如果程序停在了断点处，Emacs会自动上拉出源文件，相比于只看源文本的一行，查看整个源文件会更容易显示出在程序调试的时候发生了什么。



**1.5  查看更多信息**

几乎每个Linux发行版都会附带诸多的文档。可以通过阅读你使用的Linux发行版的文档学习更多的关于在本书中提到的内容（尽管这会花费你很长时间），但是并不是所有的文档都结构清晰，所以找出你需要的那部分内容来学习就好。有些文档可能是过期的，那么就要有选择的进行阅读。如果有的系统不像所说的那样支持man page，这种情况下，大多数是因为man page也过期了。

做为你的帮助导航，这里有关于Linux高级编程的最有用的信息源。

[6]: 曾经有人讨论说break main有点儿奇怪，因为通常你想这么做的时候，程序已经停在main处



**1.5.1 帮助手册**

Linux发行版包含了大多数标准命令，系统调用，还有标准库函数。帮助手册的不同的小节按数字来区分，对于程序员来说，最重要的是下面这些：

(1)  用户命令

(2)  系统调用

(3)  标准库函数

(4)  系统/管理员命令

这些数字标识了帮助手册的不同小节，Linux的帮助手册预装在了你的系统里；使用access命令就可以访问它们。简单的使用man 名称 这种命令形式来查看帮助手册，名称就是一个命令或者函数的名字。少数情况下，相同的名称会出现在不同的小节中；可以使用在名称的前面加数字来明确的指定帮助手册小节，例如，像下边这样输入，就会得到sleep命令的帮助手册（在Linux 帮助手册的1小节）：

> % main sleep

查看sleep库函数的帮助手册，使用下面的命令：

> % man 3 sleep

每个帮助手册都包含一个命令或者函数的单行摘要，whatis 名称 命令会显示所有帮助手册中匹配的函数名或者命令名，如果你不确定你想要使用哪个命令或者函数，可以在单行摘要中使用man –k 关键字 进行关键字搜索。

帮助手册包含了非常有用的信息，在需要帮助的时候应该第一时间想到它，命令帮助手册中记录了命令的详细描述，命令行选项和参数，输入输出，错误代码，配置，等等这些，系统调用的帮助手册记录了库函数描述，函数参数，返回值，错误代码列表及副作用，并且会指出调用这个函数需要包含哪个文件等。



**1.5.2  Info 文档**

Info文档系统包含了对GNU/Linux系统核心组件和其他几个应用程序更加详细的文档，Info手册是超文本形式的文档，与网页相似。只需要在shell窗口输入info就可以打开基于文本的Info浏览器，现在会有一个带Info文档的菜单安装在你的系统上（按下Ctrl+H显示Info文档的导航）。

       最为有用的Info文档是下面这些：

       gcc——gcc编译器

       libc——GNU C库，包含众多系统调用

       gdb——GNU 调试器

       emacs——Emacs编辑器

       info——Info系统自己

几乎所有的Linux 编程工具（包括ld链接器，as汇编器，gprof代码分析工具）都带有非常详细的Info手册，你可以在命令行指定手册名称直接跳转到对应的Info文档：

> % info libc

如果你大部分编程工作都用Emacs来完成，那你可以使用M-x info或者C-h i来访问Emacs内建的Info浏览器。



**1.5.3  头文件**

通过系统头文件，你可以了解到更多可用的系统函数，并且查到函数的用法，系统函数头文件都位于/usr/include和/usr/include/sys目录下。如果你在系统函数调用中捕获了一个编译错误，这种情况下，应该查看对应的系统头文件，验证一下函数的签名是否与帮助手册中所列出的签名一致。

在Linux系统中，大量的系统函数如何工作的细节都反映在目录/usr/include/bits，/usr/include/asm，/usr/include/linux的头文件中，例如:信号量的数值就被定义在/usr/include/bits/signum.h中，这些头文件为有好奇心的人提供了更好的阅读资料。但是不要把这些头文件直接包含在你在程序中；总是使用/usr/include目录下的头文件，或者是你想用的函数的帮助手册中提及到的对应的头文件。

 

**1.5.4  源代码**

Linux是开源的，对吧？系统如何运行的最终决定者还是系统源代码自己，Linux程序员是幸运的，系统源代码都是可免费获取的。也就是说，你的Linux发行版包含了整个系统和应用软件的所有的源代码。如果有的没有，那你也有资格根据GNU GPL协议向软件的发行者索要源代码（即使源代码并没有安装在你的磁盘上，你也可以通过查看发行版文档介绍来安装它）。

Linux内核的源代码通常会放在/usr/src/linux目录下，如果这本书让你渴望更详细的了解系统如何运行处理，共享内存，和系统设备如何工作，你可以直接去看源代码学习，这本书中大多数系统函数都已经被GNU C库实现。可以通过查看发行版文档说明，找到C库源代码位置。
