###《游戏引擎架构》读书笔记
最近开始读Milo翻译的**《Game Engine Architecture》**，希望能够在还剩下一年的学生阶段中，尽可能在进入职场前了解游戏开发以及游戏引擎的方方面面，从研二开始才真正接触图形学，图形引擎，瞬间点燃了我的学习热情，期间，了解过Opengl，认真学习了D3D11，钻研了OGRE，玩了UE4，但是慢慢地我发现，一个完整的游戏(这里仅仅指端游)或游戏引擎，是一个对于现在我来说过于“可怕”的庞然大物，仅仅通过零散时间的学习零散的知识点，始终无法能够在脑海中建立起一个游戏开发的完整呈象。一方面当然是接触时间还太短，另一方面也是很重要的一方面，是因为自己对于软件工程的概念太过于模糊，因此除了好好学习《设计模式》和《代码大全》这一类提高自己对于软件架构和软件工程高层次的认识，还需要对于游戏开发和游戏引擎有一个具体的认识，这也是我读这本书的出发点，希望记录下每当有所领悟时，自己的理解。

首先，还是先贴出一张引擎的架构图，从而时时刻刻在脑海中刻下一个印记，然后从方方面面去深入：
![Game Engine Architecture:](EngineArc.jpg)

* #####P8 电子游戏作为软实时模拟
电子游戏本质，实际上是**软实时**(soft real-time)**互动**(interactive)**基于代理**(agent-based)的**计算机模拟**(compter simulation)
___

* #####P10 游戏引擎是什么
**数据驱动架构**(data-driven architecture)或许可以用来分辨一个软件的哪些部分是引擎，哪些部分是游戏.
其实读过以后才认识到，其实游戏引擎与游戏之间界限是很模糊的，并且，不同类型游戏的引擎，其差异也很大，关键是技术的侧重点不同，典型的例子，FPS(CS)，MMO(WOW)，格斗类游戏(铁拳)，竞速类游戏(极品飞车)，实时策略游戏(星际)，RPG(FF系列)，体育(FIFA)等等。
___

* #####P27 运行时引擎架构
本节十分精彩，Jason通过简练的语言将一个庞大的系统架构描述出来，让我对游戏引擎有一个整体的概览和认识，具体细节由上面的图片可以有一个清晰的认识。
让我感触最深的是，图形，虽然是游戏中很重要的一个部分，但，远远远远不是全部。
凭着我对本节的认识，还有
`AI，网络，动画，物理，音乐，视频，资产(材质，纹理，骨骼，动画，shader)`
___


* #####P54 游戏开发中，常用的版本控制系统
 * **SCCS**,**RCS**
 * **CVS**
 * **Git**
 * **Subversion(SVN)**
 * **Perforce**
 * **NxN Alienbrain**
 * **ClearCase**
___


* #####P78 剖析工具与内存泄漏监测
了解了**[Pareto principle](http://wikipedia.org/wiki/Pareto_principle)**
 * 性能剖析工具： 
**统计式剖析器(statistical profiler)：**Intel的 **[VTune](http://software.intel.com/en-us/intel-vtune-amplifier-xe)**
**测控式剖析器(instrumental profiler):**IBM的 **[Rational Quantify](http://www.ibm.com/developerworks/rational/library/957.html)**
 * 内存检测工具：IBM的IBM的 **[Rational Purify](http://www.ibm.com/developerworks/rational/library/957.html)**
其实比较早就知道有类似**[valgrind](http://valgrind.org/downloads/current.html)**这类内存泄漏的监测工具，但是一直都没有使用过，出现了内存泄漏的问题都是不停的review，但是但是，人和猴子最大的区别不就是人会使用工具么。。。。

___

* #####P88 游戏中最常用的设计模式
 * **单例：**常用于构建引擎的各个子系统
 * **迭代器:**用于遍历游戏中大量的对象，需要十分高效的实现
 * **抽象工厂：**用于常见和管理游戏中的各类对象：玩家角色，AI，相机，游戏中的各类物体，资源如纹理，贴花，阴影等等，甚至是碰撞用的射线、包围盒等等
___


* #####P89 编码标准，为什么及该用多少
如果创建了就请始终如一的坚持标准，这是作为团队中一员开发最重要的“道德”。
另外，Json推荐了 [Making Wrong Code Look Wrong](http://www.joelonsoftware.com/articles/Wrong.html)
___


* #####P92 浮点计法
浮点数是被**量化**的，浮点数的组成，IEEE-754：**符号位(1bit)**|**指数(8bit)**|**尾数(23bit)**
`v = Sx2^(e-127)x(1+m)`
以前一直没有好好的理解浮点数这个基本的只是，借着本节好好复习和巩固了浮点数的构成，明白了其**范围和精度的取舍**，也名表了**32位机器的epsilon`(1.192x10^-7)`由来**，因此在游戏编程中，判断某对象的速度是否为零时：
应该是`if(v < epsilon)`而不是`if(v == 0.0f)`
___

* ##### P118 捕捉及处理错误
错误的捕捉和处理，一直是自己平时编码和写程序中，最最忽视掉的环节，然而，慢慢的，经过阅读了各种经典书籍后，才发现，错误的捕捉及处理，通常是一个软件项目中最重要的环节之一，固然，软件的算法效率是很重要的环节，新的牛X技术也是很重要的环节，然而，脱离了稳定性一切都是白搭，一个成熟的产品绝对不允许出现**在大多数情况下可以，偶尔会失效**的情况。
推荐文章，所有问题皆无银弹：[no silver bullet](http://en.wikipedia.org/wiki/No_Silver_Bullet)
* ######错误的大体分类：
 * **用户错误(user error)**：用户错误，根据语境在游戏中又可以分为
   * **玩家错误：**即输入错误，输入无效，对于玩家错误，需要尽可能的考虑周全，各种case都要面面俱到，关于这一点在今年网易游戏实习生笔试的时候印象太深刻了，其实三道题都不难，但是需要清晰地考虑各种边界条件，而这一点恰恰是自己在平时的编程过程中忽视掉的，以为主要的功能完成即可，确没有很深入的考虑异常情况的处理。
   * **开发者错误：**即开发中，A用了B提供的函数，但使用了错误的参数等情况，这一点需要在提供接口时具有清晰的描述。
 * **程序员错误(programmer error)**：代码本身存在bug，这种情况是绝对不允许的。
___

* ######错误监测及处理方法：
 * **错误返回值**
 最常见的错误处理方式，通过返回值监测，返回bool型表示成功和失败，或利用错误编码来指定具体错误类型，这里强烈建议使用Enum来表示具体错误而不是简单的编号如1,2,3,(对于C语言可以用宏定义)，另外，如果错误码是通过全局变量指定的，还需要考虑到多线程情况下的同步问题。
 * **异常**
 异常最大的优势就可以可以将程序的正常业务流程和错误处理完全分开，结构清晰，但是，异常通常伴随着庞大的系统开销，甚至是内存泄露等，Jason给出的建议是：
 >因此，在主机游戏引擎中，有颇充分的理由去**完全关闭**掉异常处理，然而在PC中可以安然的使用异常，也贴出了相应的文章：
 [Exceptions](http://www.joelonsoftware.com/items/2003/10/13.html)
 [Exceptions2](http://www.joelonsoftware.com/items/2003/10/15.html)
 [Exceptions vs. status returns](http://nedbatchelder.com/text/exceptions-vs-status.html)
 * **断言**
 断言通常在调试期使用，最终的发行版将去掉所有的断言，有个很秒的比喻，**断言通常是为bug准备的地雷**，然而，一定要牢记，断言通常只用来捕获程序bug，而不是用户错误，因为断言失败的结果一定是**程序异常退出**。
 断言通常通过宏定义实现，然而利用宏实现断言时一定要注意红展开时的一些问题，举个例子：
```cpp
	#define ASSERT(expr) if(!(expr)) {reportAssertionFailure(#expr, __FILE__, __LINE__)}
    void f()
    {
        if (a < 5)
            ASSERT(a >= 0);
        else //展开后对应了错误的if
            doSomething();
    }
```
```cpp
#if ASSERTIONS_ENABLED
		#define debugBreak() asm {int 3;}
    	#define ASSERT(expr)\
		if (expr) {} \ //正确的做法
        else \
		{ \
        	reportAssertionFailure(#expr, __FILE__, __LINE__); \
			debugBreak(); \
        }
#else
		#define ASSERT(expr)
#endif
```