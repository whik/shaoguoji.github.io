---
layout:     post
title:      基于接口的设计、文学程序、风格、效率
subtitle:	 《C 语言接口与实现》读书笔记 —— 前言、第一章
date:       2017-11-28
author:     Shao Guoji
header-img: img/post-bg-c-interface-implementatios.jpg
catalog:    true
tag:
    - C语言
    - 读书笔记
---

### 我又开始写读书笔记了

过去一年来也读过几本书，但读书笔记这事是一拖再拖。难得这段时间能静下心来啃书，决定恶补一下基础知识和提高一下 C 语言，并记录下读书过程中许多自认为有价值的东西。由于是笔记，会更倾向于「给自己看」，所以写出来的东西东拼西凑比较零散，难免会缺乏条理逻辑，但也会尽量考虑读者阅读体验（毕竟写东西的初衷还是给「未来的自己」看）。

---

### 关于本书

![图1 书籍封面](http://odaps2f9v.bkt.clouddn.com/17-11-29/94639556.jpg)

[《C 语言接口与实现》（豆瓣评分 9.1）](https://book.douban.com/subject/6801697/)，乍听书名个人一种莫名其妙的感觉，实际作者就是想告诉人们，C 语言虽然古老又低级，但并不是只能用来写操作系统、驱动等底层程序。相反，只要驾驭熟练，还是可以写出像 C++ 等面向对象语言般的稍微大型点的程序。

所以就有亚马逊读者评论此书是「从 C 语言新手变成高手的必读之作」。对于一直玩单片机写裸机程序的我而言，能够进一步了解 C 语言的其他应用也不是一件坏事。除此之外，本书的习题也是设计的不错，在每篇文章最后都会附上我不自量力的解答，并提交 github。

英文原版 PDF：[David_R_Hanson-C_Interfaces_and_Implementations-EN](http://www.r-5.org/files/books/computers/languages/c/mod/David_R_Hanson-C_Interfaces_and_Implementations-EN.pdf)

配套代码下载：[GitHub - drh/cii: C Interfaces and Implementations](https://github.com/drh/cii)

习题个人解答：[GitHub - shaoguoji/cii-exercises-solutions](https://github.com/shaoguoji/cii-exercises-solutions)

---

### 前言 —— 书籍主旨

#### 内容提炼

1. 提倡基于接口及其实现的设计方法
2. 书中的实现代码并不是玩具代码，而是产品级使用
3. 基于接口的设计跟具体语言无关
4. 本书重点在算法工程，而不在数据结构算法本身

#### 个人理解

1. 作者的核心思想是**可复用**，即通过设计通用的**接口及其实现**打造出许多可重用的模块，然后基于这些模块进行开发。这样一来只需在程序模块的基础上进行「搭积木」式的拼接就能快速完成产品的开发，可以说是一种开发方式，书中原话：

> 一旦掌握了基于接口的设计方法，就能够在服务于众多应用程序的通用接口基础上建立应用程序，从而加快开发速度。在一些 C++ 环境中的基础类库就体现了这种效果。增加对现有软件（接口实现库）的重用，能够降低初始开发成本，同时耗能降低维护成本，因为应用程序的更多部分都建立在通用接口的实现之上，而这些实现无不经过了良好的测试。【P IV】

感觉有点像在用 C 语言实现类似 C++ STL（标准模板类库）那套东西（反正我不会 C++）……

2. 强调了这书里面的代码不只是让你用来装逼和炫技用的（虽然装逼的确很好用），而是可以真枪实弹可以直接用在产品上的。书中使用的一些数据结构及算法都是相当成熟的实现，甚至大部分内容都是从其他一些著作中借鉴过来的，所以几乎所有接口的实现性能都是 OK 的（与问题规模 N 成线性关系），伸手党放心用吧。

3. 第 1 点也讲了这是一种开发方式，所以与语言无关，但用 C 语言实现略为麻烦，并且对于 C 语言的各种坑不能掉以轻心，书中原话：

> C 编程语言基本不支持基于接口的设计方法，而 C++ 和 Modula-3 这样的面向对象的语言则鼓励将接口与实现分离。基于接口的设计跟具体语言无关，但是它要求程序员对 C 一样的语言有更强的驾驭能力和更高的警惕性，因为这类语言很容易破坏带有隐含实现信息的接口，反之亦然。【P IV】

最后一句话「因为……」这里不太理解，隐含实现？反之亦然？什么鬼……

4、可以理解为「基于接口的设计」是对数据结构与算法的「包装」，并不深挖包装盒里的东西。当然，但内容与包装要搭。书中原话：

> 书中提供的一些接口时针对数据结构的，但本书不是介绍数据结构的，本书的侧重点在算法工程（包装数据结构以供程序使用），而不再数据结构算法本身。然而，接口设计的好坏总是取决于数据结构与算法是否合适，因此，本书可算是传统数据结构和算法教材（如 Robert sedgewick 所著的 Algorithms in C）的有益补充。【P V】

从未好好看过算法，有空找《Algorithms in C》来看下。

#### 教学使用建议

预备知识：

1. 了解 C 语言
2. 了解基本数据结构

书中原话是「在大学介绍性的编程课程中了解了 C 语言」，这话放到国内基本没法玩（国外的「介绍性的编程课程」这么吊的么……），只限于大学那点 C 语言皮毛要肯这书有点难度。至少要把指针玩熟了，看到代码中的下标、星号、指针偏移能在脑子里放映「内存动画片」就差不多了。

*其实写到这里时我已经看了前 3 章了，所以对于我来说这是一本难得符合「i + 1」输入原则的书，个人自认为 C 语言基础还行，基本数据结构在校学的也蛮认真。正当我急需提升 C 语言相关知识时，这本书无疑给我从「舒适区」里拉了一把。希望大家也能根据自己能力水平选到好书。*

---

### 第一章 引言 —— 造轮子是为了不再造轮子

#### 个人总结

因为现有的库大都不好用，所以程序员经常自己来编写特定于应用程序的代码，此外，一些库过于复杂、学习成本大，也是一些程序员不使用（懒得学）的原因，但其实自己实现也不会轻松多少。所以作者建议，你不是要写么，那就用我这「基于接口的设计」套路造轮子、写一个更好用、更通用、可重用的库，造福广大码农啊！于是就开始了本书的旅程……

---

### 文学程序（literate program）

#### 内容提炼

1. 文学程序是一种不受限与特定语法，而是为文字表述服务的一种代码表达方式
2. 代码与正文交织在一起，文章与代码相辅相成，浑然一体。
3. 文学程序通过「代码块标签」给出代码，并且各代码块顺序不定，且内容可随时增加
4. 用「代码块标签」替换实际代码，程序表达更简洁清晰

#### 个人总结

如果说 MarkDown 是「像写代码一样写文章」，那文学编程就是「像写文章一样写代码」了，其强调的核心思想是，代码与其说明并不是独立存在的，而是结合起来形成可读性良好的文学作品（书籍），所以网上有人说文学编程就是往代码里写多点注释就明显不合理，因为这时代码是代码，注释是注释。

说白了文学编程就是写书用的，代码这是要为作者的表述服务，其表现在代码的出场次序上，「代码块可以按最适于理解的顺序给出」，所以不必遵循编程语言强加的顺序（如 C 语言的「先声明后使用」）。

举个简单例子吧，假如要写一个 Hello World 程序，文学编程给人的感觉是这样的：

先**定义根代码块**（`hello.c` 表示文件名），这代表程序的整体组成顺序（代码整体还是有顺序的）：

```c
<hello.c 3> ≡
  <includes 4>
  <data 4>
  <protorypes 5>
  <functions 3>
```

C 语言中程序都是从主函数 `mail()` 开始执行的，在书本第 5 页（尖括号后的数字表示代码所在页数）的 `protorypes` 块中可以看到函数原型，

```c
<functions 3> ≡ 
  int main()
  {
      hello_print();
      return 0;
  }
```

`≡` 可以简单理解为等于号， `hello_print()` 函数实现打印功能，定义如下：

```c
<functions 3> +≡ 
  void hello_print(void)
  {
      <print string in console 5>
  }
```

`+≡` 符号表示往代码块中继续补充内容。函数的核心功能就是打印字符串了，这里调用库函数 `printf()` 实现：

```c
<print string in console 5> ≡
  printf("%s\n", str);
```

其中 `str` 为全局变量，在文件前面定义，表示要打印的字符串：

```c
<data 4> ≡ 
  char str[] = "Hello World!";
```

把函数声明一下：

```c
<protorypes 5> ≡ 
  void hello_print(void);
```

对了我们还要包含相应的头文件：

```c
<includes 4> ≡ 
  #include <stdio.h>
```

这样一个用文学程序描述的 Hello World，就完成了。可以发现 `hello_print()` 函数我是先写了使用在写声明，全局变量 `str` 也是先用再补充，这就摆脱了语法的限制，使得对程序的说明可以先开门见山说重点再慢慢补充细枝末节，而不必因为语法规则的限定先写一大堆用不上的代码。让写书时逻辑非常清晰。

---

### 习题（完整代码见[Github](https://github.com/shaoguoji/cii-exercises-solutions)）

**1.1 在一个单词结束于换行符时，getword 在 <scan forward to a nonspace or EOF 5\> 代码块中将 linenum 加 1，而不是在 <copy the word into buf[0..size-1] 5\> 代码块之后。解释这样做的原因。如果在本例中，linenum 的加 1 操作是在 <copy the word into buf[0..size-1] 5\> 代码块之后进行，会发生什么情况？**

答：getword 在 <scan forward to a nonspace or EOF 5\> 代码块中将 linenum 加 1，可以统计文件开始处的空行并将空白字符读走。如果 linenum 的加 1 操作在之后进行，就会丢失对连续空白符情况下换行符的计数（因为没用循环），导致行数不准确。

**1.2 当 double 在输入中发现三个或更多的相同单词时会显示什么？修改 double 来改掉这个「特性」。**

答：当输入中有三个或更多（如 n 个）相同单词时，程序会显示 n-1 次此单词（因为只要符合「相邻相同」都会触发），可做如下修改使之只显示一次：

在程序中增加 `lastword` 数组存储上一次找到的重复单词，只有在「本次重复单词与上次重复单词不同」的情况下才打印结果，避免了同一单词连续出现多次带来的影响。

修改 `doubleword()` 函数，增加全局变量及输出结果判断条件：

```c
void doubleword(char *name, FILE *fp)
{
  char prev[128], word[128], lastword[128];  // 增加 lastword 数组
  
  linenum = 1;
  prev[0] = '\0';
  lastword[0] = '\0';  // 增加 lastword 数组初始化
  while (getword(fp, word, sizeof word))
  {
    if (isalpha(word[0]) && strcmp(prev, word) == 0 
        && strcmp(lastword, word) != 0)  // 增加条件：本次结果与上次结果不同
    {
      if (name)
      {
        printf("%s:", name);
      }
      printf("%d: %s\n", linenum, word);
      
      strcpy(lastword, word);  // 更新 lastword 数组为本次结果
    }
    strcpy(prev, word);
  }
}
```

**1.3 许多有经验的 C 程序员会在 strcpy 的循环中加入一个显式的比较操作：**

```c
char *strcpy(char *dst, const char *src)
{
    char *s = dst;

    while ((*dst++ == *src++) != '\0')
        ;
    return s;
}
```

**显式比较表明赋值操作并非笔误。一些 C 编译器和相关工具，如 Gimpel Software 的 PC-Lint 和 LCLint[Evants, 1996]，在发现赋值操作的结果用作条件表达式时会发出警告，因为这种用法是一个常见的错误来源。如果读者有 PC-Lint 或 LCLint，可以在一些「测试」过的程序上进行试验。**

答：验证性题目，待做（等我装了这俩玩意儿再说……）。