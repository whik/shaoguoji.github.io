---
layout:     post
title:      焊接那点事儿
subtitle:   PCB贴片元器件手工焊接技巧及要点
date:       2017-03-14 10:46:36 +0800
author:     Shao Guoji
header-img: img/post-bg-welding-skill.jpg
catalog:    true
tag:
    - 硬件
---

![板子](http://odaps2f9v.bkt.clouddn.com/17-3-25/36127354-file_1490415861247_6b4f.jpg)

### 内容简介

以下内容都是我最近焊贴片所收获的知识和技巧，现在无私分享给你（如果我写成这样你都能看懂的话~(～￣▽￣)～）。但我想你应该知道，焊接本来就不是三两下就能学会的，还是要多练，熟能生巧。

---

<br/>

### 我真的会焊贴片么？

最近公司新产品出炉，一直在帮忙焊板子，才发现自己的焊接水平有待提高。一直自认为焊接技术“还可以”，但事实证明，能焊一点插件就“自我感觉良好”的我实在是太年轻了，在洞洞板焊直插这种随便找个电子专业的童鞋都完全没压力好吧。

在PCB上焊贴片元器件才是真正考验技术的工作，遇到密密麻麻的芯片引脚和比沙子还小的电容电阻，虚焊、短路等问题频频出现且极难发现，对手法以及耐心的要求都非常高。但更重要的是一些小技巧和要注意的细节，有些要点是我之前一直忽略的。

> 焊接对各位硬件老司机来说是轻车熟路了，我只是个水平一般的初学者，还请多多指教，见笑了。

---

<br/>

### 关于烙铁

一个可调温的焊台似乎是必不可少的，不然怎么知道烫到你的烙铁是多少度的呢哈哈~~~有了焊台，刀头烙铁也成了标配（反正我用尖头烙铁焊贴片很蛋疼），再加上焊锡、海绵、助焊剂，焊接大家族算是齐人了。

![焊台](http://odaps2f9v.bkt.clouddn.com/17-3-22/34902681-file_1490174592400_4d35.jpg)

关于烙铁的使用要注意几点：

* 烙铁头易损，随时保持烙铁头挂锡
* 避免烙铁头与硬物敲击，防止变形
* 焊贴片时温度调到350°C左右就好
* 避免烙铁久置加热，否则容易烧死（长时间不用时把温度调低）
* 海绵别加太多水，拧干点保持柔软
* 烙铁别烫到奇怪的东西，不然会有奇怪的味道→_→

---

<br/>

### 贴片电阻电容值的表示

简单讲下贴片元件的标识，要知道，看不懂电阻电容值的表示，你连元件都找不到！！！

#### 直标法

> 直标法：用数字和单位符号在贴片电阻器表面标出阻值，其允许误差直接用百分数表示，若贴片电阻上未注偏差，则均为±20%。直标法中可用单位符号代替小数点直标法一目了然，但只适用于较大体积元件，且国际上不能通用。

也就是直接写出数值和单位，比如我们常说的100Ω、4K7(4.7K)、220uF等。

#### 数码法

> 数码法：在贴片电阻器上用三位数码表示标称值的标志方法。数码从左到右，第一、二位为有效值，第三位为指数，即零的个数。偏差通常采用文字符号表示。

比如 103 的电阻，就是 10 后加三个零，所以就是 10000 ，单位Ω，即 10K 电阻，同理， 331 电阻就是 330 欧啦。而对于电容，单位是 pF，如 104 电容，就是 100000pF = 100nF = 0.1uF。

*特别要注意电容的单位换算，pF最小，到 nF，再到 uF，最后F。1F=10^6uF=10^9nF=10^12pF*

---

<br/>

下面就我最近接触的一些元器件来说明一下不同元件焊接的注意事项。

### 芯片

#### QFP封装

![QFP封装](http://odaps2f9v.bkt.clouddn.com/17-3-22/11162348-file_1490174733615_1add.jpg)

SOP封装的芯片和引脚间距都较大的QFP，基本上直接用刀头加锡往外刮两下就完事，问题不大。这里重点讲下引脚密集的QFP封装的焊接。先固定芯片，步骤如下：

* 把芯片平置于 PCB 上，焊盘不用加锡，否则芯片放不平，极易虚焊
* 注意芯片一脚方向与PCB对应（一脚通常在小圆圈或正看丝印左下角位置）
* 对齐芯片四边引脚与焊盘，要兼顾四边是个非常虐心工作
* 烙铁加锡固定的一边的边缘几个引脚，查看四边对齐情况，没对准还有挽救机会——用烙铁重新调整芯片位置
* 烙铁加锡固定对边的几个引脚

![对齐引脚](http://odaps2f9v.bkt.clouddn.com/17-3-22/23092392-file_1490175550216_18515.jpg)

![固定芯片](http://odaps2f9v.bkt.clouddn.com/17-3-22/98095134-file_1490175630948_e8.jpg)

这样一来芯片就被固定好了，接下来就开始焊了（很多元件都是“先固定，再焊接”的套路），焊的时候也需留个心眼：

1. 选择没有焊锡固定的一边，开始焊接
2. 用烙铁蘸助焊剂涂抹一边引脚与焊盘
3. 从一边加锡，拼命加到形成锡球
4. 用烙铁往另一边引流焊锡，被锡润湿过的引脚自然就焊上了
5. 用烙铁头的粘性把多余的锡移除。
6. 如法炮制剩下的三边
7. 四边都焊好后检查引脚是否黏连、短路，必要时补焊。

![蘸助焊剂](http://odaps2f9v.bkt.clouddn.com/17-3-22/70146837-file_1490176096998_15091.jpg)

![涂助焊剂](http://odaps2f9v.bkt.clouddn.com/17-3-22/45272077-file_1490176097656_138e7.jpg)

![加锡](http://odaps2f9v.bkt.clouddn.com/17-3-22/19161667-file_1490176097986_cfdb.jpg)

![拖焊](http://odaps2f9v.bkt.clouddn.com/17-3-22/42810473-file_1490176098337_ab20.jpg)

![余锡](http://odaps2f9v.bkt.clouddn.com/17-3-22/71210082-file_1490176098690_1a53.jpg)

![除去多余焊锡](http://odaps2f9v.bkt.clouddn.com/17-3-22/56953133-file_1490176099035_df47.jpg)

![焊好一边](http://odaps2f9v.bkt.clouddn.com/17-3-22/42479811-file_1490176099389_9edd.jpg)

![焊好四边](http://odaps2f9v.bkt.clouddn.com/17-3-22/69591083-file_1490176099723_12d92.jpg)

![焊好三个芯片](http://odaps2f9v.bkt.clouddn.com/17-3-22/34876156-file_1490176100145_75e0.jpg)

总之焊芯片只要记住两点：**1、焊的时候先多加锡，引脚连起来也无所谓，焊好后再慢慢分开。2、多加助焊剂**，助焊剂能让焊锡流动性更好，而不是像一坨粘粘的shi一样。

油管上有个视频专教焊QFP芯片的：[Professional SMT Soldering: Hand Soldering Techniques - Surface Mount](https://www.youtube.com/watch?v=5uiroWBkdFY)。

#### QFN封装

![ QFN 封装芯片](http://odaps2f9v.bkt.clouddn.com/17-3-22/72556083-file_1490176986794_f5f8.png)

![4 脚贴片晶振](http://odaps2f9v.bkt.clouddn.com/17-3-22/76683028-file_1490177234685_1008f.jpg)

对于QFN这种没有引脚外露的芯片（如ESP8266），或者接触点在底面的 4 脚贴片晶振。用烙铁直接焊较困难，且不可靠。正确的方法应该是用热风枪吹（第一次拿热风枪的我好激动啊~(～￣▽￣)～），那要怎么吹呢？

1. 所有引脚焊盘、中间大焊盘上薄锡
2. 用镊子夹着芯片，用底面抹一下助焊剂
3. 把芯片放到焊盘上，大概对一下位置
4. 风枪温度也是调350°C左右，风速调最小
5. 垂直于 PCB 握住风枪，高度 8CM 左右，先加热芯片外围，最后对准芯片吹
6. 助焊剂和焊锡融化，在液体张力下芯片自动吸附到正确位置
7. 关掉风枪，检查芯片连接情况，小心 PCB 烫
8. 必要时用烙铁补焊四周

![QFN芯片](http://odaps2f9v.bkt.clouddn.com/17-3-22/89452125-file_1490177407398_163ff.jpg)

![焊盘加锡](http://odaps2f9v.bkt.clouddn.com/17-3-22/36052140-file_1490177493721_cfa1.jpg)

![芯片抹助焊剂](http://odaps2f9v.bkt.clouddn.com/17-3-22/25584344-file_1490177543482_a198.jpg)

![放置芯片](http://odaps2f9v.bkt.clouddn.com/17-3-22/83355050-file_1490177600956_bed.jpg)

![热风枪](http://odaps2f9v.bkt.clouddn.com/17-3-22/72859496-file_1490177707208_3ee3.jpg)

![吹芯片](http://odaps2f9v.bkt.clouddn.com/17-3-22/39251423-file_1490177707578_15b9b.jpg)

![风枪芯片距离](http://odaps2f9v.bkt.clouddn.com/17-3-22/650677-file_1490177707954_dbea.jpg)

![烙铁补焊](http://odaps2f9v.bkt.clouddn.com/17-3-22/48699595-file_1490177708841_e87a.jpg)

![焊好的 芯片](http://odaps2f9v.bkt.clouddn.com/17-3-22/30750030-file_1490177709202_c0cd.jpg)

注意事项：

1. 风速不可太大，高度要适宜，不然会把芯片或者外围小电容电阻吹飞的
2. 若芯片难以自动吸附，可用镊子辅助调整，或按压一下
3. 切记中间固定焊盘不可加太多锡，不然芯片下去会把锡向外挤出造成引脚短路。
4. 热风枪真的很烫很烫很烫烫烫烫烫烫烫烫烫烫烫

---

<br/>


### 电容电阻、二极管

电容电阻等二端元件是最常见的了，比较[常用的焊法](https://wenku.baidu.com/view/a617f21b59eef8c75fbfb379.html)是：先给一边焊盘上锡，用镊子焊上一边固定，再焊另一边。这种方法最大的好处就是易于调整元件位置（因为有镊子辅助），所以焊好的元件看起来整齐美观。缺点也很明显——效率太低（也是因为要拿镊子）。尤其在面对有大量元件的PCB时，一个一个脚的焊似乎有点慢……

现在我会用一种“烙铁粘元件”的快速焊接方法，牺牲元件排列美观换更高的效率。这种方法也是公司的大神工程师教我的，不需要镊子就可以同时焊好元件的两个脚，效率加倍，步骤如下：

1. 两边焊盘加锡
2. 甩掉烙铁多余的锡，保持烙铁头有一层薄锡覆盖（烙铁头有锡才有粘性）
3. 将元件平放，烙铁头从侧面接触把元件水平粘起
4. 把带有元件的烙铁至于焊盘位置，同时加热两端，用烙铁头微调元件位置
5. 顺着元件方向移除烙铁
6. 并排的元件从左往右逐个焊接

![焊盘加锡](http://odaps2f9v.bkt.clouddn.com/17-3-22/26938436-file_1490177946846_e2f1.jpg)

![烙铁粘元件](http://odaps2f9v.bkt.clouddn.com/17-3-22/82287233-file_1490178076064_16fcd.jpg)

![粘起元件](http://odaps2f9v.bkt.clouddn.com/17-3-22/9639846-file_1490178076285_132c8.jpg)

![移动元件](http://odaps2f9v.bkt.clouddn.com/17-3-22/11733463-file_1490178274271_169e4.jpg)

![放置元件](http://odaps2f9v.bkt.clouddn.com/17-3-22/31987878-file_1490178274662_10ab9.jpg)

![移除烙铁](http://odaps2f9v.bkt.clouddn.com/17-3-22/45325094-file_1490178275086_e695.jpg)

其中用烙铁去“粘”元件是关键的一步，技巧是慢慢用烙铁靠近元件侧边，轻轻地接触。要注意的是烙铁头有锡才有粘性，但锡不能过多，否则液体的表面张力会让元件难以控制。如何让烙铁头覆上薄锡呢？先加锡，再沿着烙铁方向抖几下即可（想象一下扔飞镖的手部动作），实际焊接中也是很少用海绵的，多余的锡都是直接抖掉，所以这个“抖”的动作很重要（海绵会把锡擦得太干净反而不好，有脏东西时才用，直接抖掉就能很自然地留下一层薄锡）。

> 用镊子反而容易造成虚焊，而用锡把元件扶正可以大大减少虚焊的概率。

用这种焊法时不能一味求快，在追求速度的同时也要确保质量，尽可能把元件焊整齐，千万千万别虚焊。此外锡量也要控制好，两边成球状那种已经算多了，不多不少刚刚好的状态应该像这样（ IPC 标准）：

![IPC 标准](http://odaps2f9v.bkt.clouddn.com/%E4%B8%8B%E8%BD%BD.png)

一些比较大比较高的贴片电解电容和二极管，烙铁不能同时加热两端，要注意焊盘不能上太多锡，否则会出现元件一边高一边低的尴尬局面。

---

<br/>

### 插座类

像 USB、SD 卡、SIM 卡等插座，都要*先焊引脚，再焊固定脚*，因为先固定插座的话位置不准就调不了了，注意别焊歪。对于有固定孔的插座，像 Micro-USB ，在焊好引脚后要把板子翻过来，在固定孔反面加锡， 让焊锡一直流到元件一面固定。原因在于某些 Micro-USB 座不完全封闭，**在元件旁焊接固定时很容易把锡弄到插孔里堵住，这样插头就插不进去了**（一句话，有洞的脚在背面焊）。

![焊USB座引脚](http://odaps2f9v.bkt.clouddn.com/17-3-22/58930214-file_1490178500931_15755.jpg)

![焊USB座固定脚](http://odaps2f9v.bkt.clouddn.com/17-3-22/59310902-file_1490178500627_11669.jpg)

![焊好的USB座](http://odaps2f9v.bkt.clouddn.com/17-3-22/66546237-file_1490178500380_12127.jpg)

还有一种更恶心的排线插座叫“ FPC 插座”，引脚非常密，而且粘锡很严重，需要用大量松香才能搞定，一般的那种黏糊糊的助焊剂也不太行，反正我焊坏了不少。

![FPC座](http://odaps2f9v.bkt.clouddn.com/17-3-22/16014751-file_1490178647713_9a27.jpg)

![焊接FPC座](http://odaps2f9v.bkt.clouddn.com/17-3-22/12623826-file_1490178650935_c994.jpg)

最后要说的一点是，由于插座类元件需要经常被插拔，所以一定要焊牢，**焊固定脚的时候可把烙铁温度稍调高，焊久一点**，一些大的元件（或与焊盘接触面大）也要延长加热时间和提高温度，确保焊稳焊牢。

---

<br/>

### 焊接的先后次序

要想更高效、可靠地焊好一块板子，是要遵循一定的原则（如“先小后大”）的，不可乱来，更不是看哪个元件顺眼就焊哪个。一般我拿到一块板子后的处理流程是：

1. 打印 PCB 封装图（即板子上印的图案），根据电路原理图用红笔在纸上标出各元件值的大小、芯片型号等（为了更快地找元件，小板子、元件少的话可略过此步）
3. 焊接电源部分、包括各种稳压、转换电路，焊好后用万用表检查各点电位是否正常（小板子可略过）
4. 焊接主要的芯片（MCU、Flash 芯片、设备驱动芯片等）
5. 焊接小的电容电阻二极管等元件
6. 焊接较大的电容、二极管等元件
7. 焊接板子外围的开关、插座、天线等元件
8. 通电测试
9. 洗板水 + 无尘布清洗 PCB 

![红笔标元件值](http://odaps2f9v.bkt.clouddn.com/17-3-26/92372911-file_1490458721626_150de.jpg)

**PS：对于电路比较复杂的 PCB ，应该先焊接电源电路，测试各点电压值正常后再焊接数字电路部分，要以模块为单位，边焊边测，及时排除问题，保证电路的正确连接。**

当然不一定要完全这样来焊，具体要根据元件和PCB的差异自行摸索出最适合的次序。

---

<br/>

### 焊接常见问题

#### 焊点不光滑

初学者常常存在焊点不光滑的问题，不仅不美观，还会造成虚焊、拉尖。焊点不光滑主要有两个原因，一是烙铁温度不够，不能完全融化焊锡，把烙铁温度调高即可。二是焊接时间过长，导致助焊剂完全挥发，焊锡便会失去光泽和流动性，解决方法就是先加锡，再把多余的锡用烙铁去掉。

#### 引脚黏连

在焊接引脚较密集的元件比如芯片时，时常会把两个或多个引脚焊一起，很难分开。这是由于焊接时间过长导致焊锡中的助焊剂都挥发掉了，焊锡失去了流动性。一般的锡线都夹带有助焊剂，这时可以再加锡，并在短时间内把多余的锡移除。更直接的方法就是直接加松香或其他助焊剂，让焊锡变得润滑起来。

---

<br/>

### 焊锡堵孔的疏通

插件的焊孔被焊锡堵住着实很让人头疼，用吸锡器吸半天也吸不出来？或者用东西钻好久也没通？其实还有更简单的方法，那就是——敲！利用液体的流动性和张力，加上伟大的牛顿第一定律（惯性），便可完美解决问题，不妨尝试一下：

1. 往焊孔焊盘一面加锡（没错，越堵越加！）
2. 用烙铁持续加热堵住焊孔的那坨锡
3. 左手拿板子，右手保持烙铁继续加热
4. 左右手同时抬起移动，把板子抬起
5. 双手同时移动，把板子对准桌子边缘迅速往下敲击，板子与桌子碰撞前烙铁止住，让板子狠狠砸在桌缘
6. 融化的焊锡就会因为撞击而被从焊孔中“甩出”

注意动作不要过于暴力，不然很容易把板子敲坏。要知道，没有什么焊孔是敲一次通不了的，如果有，就加锡多敲几次。PS: 过大的 PCB 不适合用这招

这种方法的一个应用就是拆插件，以往都是用吸锡器一个脚一个脚地拆，现在可以**直接加一大坨锡全部一起加热，然后趁热把元件整个拿下，再把焊孔敲通**，方便快捷。

油管上还有个视频专教拆元件的：[Desoldering Techniques](https://www.youtube.com/watch?v=77JgIqraX_I)。

---

<br/>

### 一些小细节

1. 注意元件与焊盘封装大小对应，别把0805的元件焊到0603的焊盘了
2. 为防止 PCB 上不必要的地方（如公司Logo）粘上锡，焊接之前可用纸胶布贴住
3. 可先把常用元件拿一堆放在固定位置，不用每次去翻元件盒
4. 在元件盒中取小贴片时，用右手小指指腹往下一摁能粘上好多哦，比用镊子一个个夹快多了
5. 暂时没想到别的，想到再补充……

---

<br/>

### 写在最后

最后要感谢工程师敢把产品样板交给我焊，看来只有在严格要求下才能发现自己的不足并提高（所以各位要实战啊不能一直焊着玩啊）。虽然接触了许多新的元器件，但缤纷多彩的电子世界里还有更多东西等着我去发现。

**不说了工头叫我焊板子去了……**

---

<br/>
<br/>

>参考文章： 
> 
> * [贴片电阻阻值的4大表示方法](http://www.xcy99.com/Article/tpdzzzd4dbsff_1.html)
> * [如何读取贴片电阻的阻值和贴片电容的容](http://jingyan.baidu.com/article/08b6a591ba928614a809229d.html)
> * [贴片电阻焊接步骤](https://wenku.baidu.com/view/a617f21b59eef8c75fbfb379.html)