---
layout:     post
title:      DIY宿舍蓝牙门锁
subtitle:   从此告别出门带钥匙的繁琐
date:       2016-05-12 13:23:17 +0800
author:     Shao Guoji
header-img: img/post-bg-bluedoor.jpg
catalog:    true
tag:
    - 硬件
    - 单片机
    - 蓝牙门锁开发笔记
---

**断断续续搞了半年，蓝牙门锁开发基本完成，现决定分享制作过程及开源程序。个人原创作品，仅供交流及分享，未经同意请勿用于任何商业用途。**

## 一、前言

由于要把蓝牙门锁拿去参加“仲园学术科技主题展览”，正好借着这次机会继续改进门锁、并回顾记录整个制作过程，也同时给想制作的朋友一点参考。其实很有必要把自己当初“一时冲动”做出来的东西重新整理一下（早就想这样干了，一直拖着），而这次展览，就是一个“逼自己”的好机会，不然这个带有一堆缺陷的“半成品”真的就这样over了~~~

——2016-04-16

重大更新，蓝牙门锁配套APP正式发布！详见[”蓝牙门锁“APP正式发布-]({% post_url 2016-05-27-BuleDoor-APP-lauch %})

——2016-05-27

**声明：“本作品属业余纯手工制作、结构简单，仅限于功能的简易实现，只针对特定型号门锁，且尚处于测试阶段，安全性有限，仅限于小型场合使用。不适用于高级别安防需求。”**

<br/>

---

## 二、功能介绍及原理实现

智能家居曾经也是火了一把，市场上的蓝牙门锁动辄几百上千，装在宿舍有点不现实，也也没必要。而自己做一个玩玩，仅仅几十块的成本就能体验一把智能家居，更能享受DIY的乐趣，说干就干。

### 主要功能

* 手机App蓝牙免钥匙遥控开门
* 关门状态实时监测
* 内置时钟，可断电走时
* 忘关门提醒
* 防盗报警
* 串口字符串命令系统


宿舍蓝牙门锁通过把舵机嵌入门锁内部机械机构，从而实现电动开门。配合外部单片机控制电路、蓝牙模块、指示灯及门锁状态开关，更可用手机APP免钥匙蓝牙遥控开门，实时监测门锁状态。而时钟芯片的引入、增加了忘关门提醒、防盗报警等人性化功能，可通过串口发送字符串命令对门锁进行设置、获取状态信息等，乃居家旅行、杀人越货、装逼撩妹必备神器，那么，就让我们开始门锁的制作吧。

**注：本制作需一定动手、焊接能力，工科屌丝男可自行摸索，妹纸请找男票帮忙……**

<br/>

---

## 三、材料准备

![prepare]({{site.baseurl}}/img/bluedoor/post-bluedoor-prepare.JPG)

| 元件                                   | 数量          | 单价（参考）  |
|--------------------------------------- | ------------- | ------------- |
| 宿舍门锁                               | 1             | ￥20          |
| STC12C4052AD单片机                     | 1             | ￥5           |
| 20脚芯片座                             | 2             | ￥0.14        |
| STC单片机下载器                        | 1             | ￥4           |
| 12M晶振                                | 1             | ￥0.2         |
| 30pF瓷片电容                           | 2             | ￥0.01        |
| HC-05蓝牙模块                          | 1             | ￥16          |
| SG90 9g舵机                            | 1             | ￥6           |
| 有源蜂鸣器                             | 1             | ￥0.5         |
| 5mmLED（颜色你喜欢）                   | 1             | ￥0.05        |
| DS1302时钟芯片                         | 1             | ￥0.5         |
| 8脚芯片座                              | 1             | ￥0.05        |
| 圆柱型3*8mm 32.768MHz晶振              | 1             | ￥1.5         |
| CR2032纽扣电池（带焊脚）               | 1             | ￥1           |
| micro-USB转接板                        | 1             | ￥1           |
| 排针                                   | 若干          | ￥0.1         |
| 5CM*7CM洞洞板                          | 1             | ￥0.2         |
| 塑胶外壳                               | 1             | ￥1.20        |
| USB充电线                              | 1             | ￥7           |
| 母对母杜邦线                           | 1             | ￥3.9         |
| 易拉罐                                 | 1             | ---           |
| 订书钉                                 | 2             | ---           |
| 螺丝                                   | 2             | ---           |
| 导线、锡线、烙铁、钳子、热熔胶枪等工具 | 1             | ---           |
| Keil软件、STC-ISP软件                  | 1             | ---           |
| 对电子制作热情的心                     | 1             | ---           |
|                                        | 总计          | ￥68.35       |

<br/>

---

## 四、单片机程序的下载

额一开始是先写硬件部分的，但发现舵机调试需要单片机输出控制信号，无奈又回来强插一段软件内容。烧写程序，学过单片机的同学一定很熟悉，那么首先你将需要：

* USB下载器及驱动程序（[下载地址](http://www.doyoung.net/DOC/CP210x_VCP_Win_XP_S2K3_Vista_7.zip)）
* STC-ISP下载软件（[下载地址](http://www.doyoung.net/DOC/STC-ISP-V488.EXE)）
* 门锁HEX文件([下载地址]({{site.baseurl}}/img/bluedoor/bluedoor.hex))

把下载器先插上电脑，装好驱动后在系统的“设备管理器”中就能看到串口设备，记住所用的COM口:

![设备管理器](http://img1.buy.ijinshan.com/weibo_img/2016/5/10/12/44/r1462855464156769197971.png)

然后把单片机和下载器按图示连接（**注意电源别接反！× 3**）：

![单片机连接下载器](http://www.doyoung.net/tool/STC_ISP_USB/pic/USB_ISP_S.jpg)

*图片来源：杜洋工作室*

打开STC-ISP软件，按图示步骤操作，记得选择刚刚的COM口。点击下载后给单片机重新上电（把电源断开再连接）,等待几秒，程序下载完成。

![烧写程序](http://img1.buy.ijinshan.com/weibo_img/2016/5/10/13/13/r1462857223329735126857.png)

<br/>

---

## 五、焊接电路板

### 1、电路板焊接

出于成本考虑（其实因为我不会画PCB）,电路板采用洞洞板直接焊接元件的方式，以达到电路稳定工作的目的（PS：之前一直用面包板一堆线的十分不稳定），所以对焊接技术有一定要求。

根据外壳大小，对洞洞板进行裁剪，我用的外壳较小，把洞洞板剪成11\*12孔（55\*27mm）大小勉强放得下除纽扣电池外所有元件并恰好装进外壳中。（[塑胶外壳外壳购买链接](https://detail.tmall.com/item.htm?id=522854650882&spm=a1z09.2.0.0.Fv74dz&_u=8oh9n953390)，记得叫卖家给广告费我！！！）

然后根据下面洞洞板布线图进行焊接，如果你有更好的布线方案就更好咯……

![洞洞板布线图](http://img1.buy.ijinshan.com/weibo_img/2016/5/8/17/3/r1462698239195052943142.png)

**元件解释**

* IC1为20脚单片机芯片座，缺口朝上，注意芯片座内焊有两个30pF电容
* IC2为蓝牙模块插座，由于排母太高，这里用钳子从另一个20芯片座上“拆下”了一个单排6脚芯片座。引脚定义（从上往下）：EN、5V、GND、TXD、RXD、STATE
* IC3为8脚DS1302芯片座，缺口朝上
* IC4为micro-USB转接板，用排针焊接
* 右上角棕色圆圈是蜂鸣器焊点
* 其他各色的小圆圈是排针引出：左下角红色是DS1302备用电源接口，右侧是舵机接口（红-VCC，黑-GND，橙-信号），右下蓝色：LED指示灯接口，黑色：门锁状态开关接口
* 左下角二极管D1是我用来隔离手动拉线开门亮灯电源用的，可以去掉


**注意上图是正面元件图，在反面锡接过线时需自行脑补走线图。**焊接过程要细心耐心，注意防止短路。焊接完成后会像这样：

![洞洞板焊接完成图正面]({{site.baseurl}}/img/bluedoor/P60429-000434.jpg)

![洞洞板焊接完成图背面]({{site.baseurl}}/img/bluedoor/P60429-000354.jpg)

很遗憾的是这里用了四根跳线，因为跳线越少电路越稳定，越美观。所以我们要尽量防止跳线的使用，就算跳也要跳的漂亮一点（请原谅我布线技术有限）。**另外注意元件之间的位置摆放、确保所有器件都能放得下。**

确保正确焊接完成后把元件插上，蓝牙模块需用钳子把排针掰成与模块垂直并修剪排针长度再插，上电看看蓝牙模块指示灯是否正常闪烁。 把板子放进外壳试一下，刚刚好。

![插上元件]({{site.baseurl}}/img/bluedoor/P60429-005100.jpg)

![板子放进外壳]({{site.baseurl}}/img/bluedoor/P60429-005143.jpg)

### 2、备用电池的焊接、外壳开孔

要实现时钟芯片断电走时，需外加备用电池，这里选用带焊脚的CR2032纽扣电池。把一根母对母杜邦线中间剪短，与电池引脚焊接，再插到刚刚预留的时钟芯片备用电源排针上即可（**注意正负**）。由于要排针（备用电源的除外）外插杜邦线，所以先把排针向外弯曲，然后对排针、USB口在外壳上对应的位置进行开孔，**注意开孔位置的准确性**。

![备用电池]({{site.baseurl}}/img/bluedoor/P60508-174422.jpg)

![外壳开孔]({{site.baseurl}}/img/bluedoor/P60508-174726.jpg)

为了插线方便，可用纸胶带在开孔处标明USB、舵机、指示灯及门锁状态开关的引脚定义：

![USB]({{site.baseurl}}/img/bluedoor/P60508-175222.jpg)

![舵机]({{site.baseurl}}/img/bluedoor/P60508-175026.jpg)

![指示灯及门锁状态开关]({{site.baseurl}}/img/bluedoor/P60508-175039.jpg)

最后再把电路板和备用电池装进外壳,排针、USB接口对准外壳开孔，如果能顺利把盖子合上，那么电路板及外壳这一部分就算是制作完成了。

![所有元件装进外壳]({{site.baseurl}}/img/bluedoor/P60508-174222.jpg)

<br/>

---

## 六、配置蓝牙模块

要让买来的蓝牙模块按照我们的意愿去工作、收发数据，要先对模块进行配置。配置包括设备名称、连接密码、通讯波特率……不过用担心，之前我已经专门写了一篇关于蓝牙模块配置的文章，所以这里就不啰嗦了，请戳——>[HC-05蓝牙模块的参数设置与使用-蓝牙门锁开发笔记]({% post_url 2016-04-16-usage-of-bluetooth-model %})

<br/>

---

## 七、手机控制端——“蓝牙串口”App

### 下载、安装、运行app

配置好蓝牙模块之后，就可以和它开心的“聊天”（通讯）啦，和PC端的“串口助手”一样，这里我们使用的是安卓手机端的“串口助手”：蓝牙串口（[下载地址](http://app.mi.com/download/35616)）。即实现手机与蓝牙模块之间的通讯。

下载安装好软件后打开，简洁但算不上清爽界面，但功能齐全，使用方便。

![蓝牙模块主界面]({{site.baseurl}}/img/bluedoor/S60511-153740.jpg)

首次运行默认“聊天视图”，滑到最右边的“键盘视图”：

![键盘视图]({{site.baseurl}}/img/bluedoor/S60511-153747.jpg)

### 添加按键定义

这款App拥有强大的按键定义功能，可把发送数据封装成按键操作。长按按键，可弹出设置框，添加“开锁”（命令：\*unlock#）和“松锁”（命令：\*lock#）两个按键，**注意所有命令以星号开始井号结束**。

![开锁按键]({{site.baseurl}}/img/bluedoor/S60511-155037.jpg)

![松锁按键]({{site.baseurl}}/img/bluedoor/S60511-155142.jpg)

![设置完成]({{site.baseurl}}/img/bluedoor/S60511-155155.jpg)

更多详细命令配置请见[宿舍蓝牙门锁命令详解-蓝牙门锁开发笔记]({% post_url 2016-05-12-bluedoor-command %})

### 连接蓝牙模块并发送消息

电路板插上蓝牙模块，上电，去到手机系统设置的“蓝牙”，搜索设备，选择你的蓝牙模块（设置蓝牙模块时的设备名），输入密码进行配对：

![系统蓝牙设置]({{site.baseurl}}/img/bluedoor/S60511-155409.jpg)

配对完成后**重启“蓝牙串口”App**，在右上角的“连接”选项中能看到我们刚刚配对好的蓝牙模块名称，点击连接：

![app蓝牙设备]({{site.baseurl}}/img/bluedoor/S60511-155430.jpg)

连接成功后左上角会显示当前设备名。把舵机插上电路板，在App点击“开锁”和“松锁”按键，舵机便会做出相应的旋转动作。

![连接成功]({{site.baseurl}}/img/bluedoor/S60511-155441.jpg)

### 配置改善

通过设置App的“自动连接”和“默认视图”能够让我们的使用更方便：

![设置App]({{site.baseurl}}/img/bluedoor/S60511-155254.jpg)

*话说手机截图怎么辣么大张~~~*

<br/>

---

## 八、门锁机械部分改装

接下来就是重头戏了，也是比较难搞、需多次调试而且很难用语言描述清楚的一部分——门锁的改装，其实就是把舵机加进去。由于学校宿舍用的是老式的弹簧门锁，所以我的改装也是专门针对此类门锁，至于其他类型的门锁，应该大同小异，你们可以自己尝试拆开摸索改装一下。

### 1、门锁的拆除

用螺丝刀趁舍管不在的时候将门锁拆下来，打开后盖，你将看到门锁内部机械结构全貌：

![拆门锁]({{site.baseurl}}/img/bluedoor/IMG_20160416_174102.jpg)

### 2、舵机安装

最烦人的时候来了。要实现电动开门就必须使用舵机，这是因为舵机的旋转角度可通过PWM波的占空比精确控制，能让舵机提供动力、“拉开”门锁（比其他各种电磁铁、电机靠谱多了有木有啊！）。红色框框部分就是舵机的安装位置，有一个不太人性化的地方：**粘死舵机后，反锁的旋钮（就是卡住不让开门那个东东）就用不了**了，所以我干脆把它拆了……还有，**舵机只能控制锁舌，而不能反锁，这也是个小缺陷。**

![舵机安装位置]({{site.baseurl}}/img/bluedoor/IMG_20160416_174130.jpg)

舵机下面需用橡皮擦、硬纸板或厚塑料片随便什么你能想到的东西垫高一下下，并把舵机一旁的螺丝固定孔剪掉，用透明胶固定一下导线，再放入舵机，导线可从旁边的孔穿出：

![剪掉螺丝固定孔]({{site.baseurl}}/img/bluedoor/1970-01-01_08-10-12_1D57D7.jpg)

![放置舵机]({{site.baseurl}}/img/bluedoor/IMG_20160417_232759.jpg)

#### 舵机传动轴的修剪

把随舵机配套的传动轴的“只有一边”的那种拿出来，用剪刀修剪传动轴的长度，一次不要剪太多，剪一点再装上舵机放到门锁上量一下。只要保证不会因为舵机位置太高合不上门锁后盖，也不会因为传动轴太低而碰到门锁外壳影响舵机转动就行。

![修剪传动轴]({{site.baseurl}}/img/bluedoor/IMG_20160417_231623_1460906494692.jpg)

![效果]({{site.baseurl}}/img/bluedoor/IMG_20160418_005718.jpg)

#### 舵机传动曲轴的制作

由于航模舵机只有一根传动轴，与我们开锁方向不一致，为实现“转动变推拉”的传动方向改变，我们要手动增加一根传动曲轴，而这个传动曲轴的材料选择也是十分麻烦、我是用反锁旋钮拆下的弹簧硬铁丝剪一小段，弯成U字形，插入传动轴的两个孔中，再用钳子缠绕而成。憋说话，看图：

![硬铁丝]({{site.baseurl}}/img/bluedoor/IMG_20160417_231952_1460906487420.jpg)

![缠绕成传动曲轴]({{site.baseurl}}/img/bluedoor/IMG_20160417_232058_1460906475090.jpg)

#### 传动轴位置的调整

再把舵机的三根线接到电路板上，上电。发送指令让舵机产生“开锁”和“松锁”的转动，不断调整传动轴与齿轮的角度，确保“松锁”状态下传动轴与舵机的“长”垂直，“开锁”状态下传动轴与舵机的“长”平行（可能转不了那么大角度，差不多就行）。貌似很难理解，来个图吧：

![舵机位置示意图](http://img1.buy.ijinshan.com/weibo_img/2016/5/8/23/35/r1462721718968094325285.png)


#### 舵机位置的调整

把弄好传动曲轴的舵机放到门锁上，比对位置，红色圈圈的地方就是舵机传动曲轴与门锁接触传动的位置，把门锁稍微拉开一点，就能把传动曲轴放入：

![舵机传动曲轴位置]({{site.baseurl}}/img/bluedoor/IMG_20160416_174307.jpg)

把舵机连接电路发送开锁及松锁命令，**注意舵机运动时用手大力按住固定舵机**，保证舵机的转动能有效对门锁进行控制。不断移动舵机调整前后左右位置，增加或减少铺垫材料调整上下位置，必要时可把传动轴拆出来调整角度再装回去。总之就是让舵机的力量充分发挥，达到能把门锁拉开和复位的较稳定状态，**实在难以调整的话，可根据具体门锁结构修改单片机程序调整舵机转动幅度**。

![舵机位置调整]({{site.baseurl}}/img/bluedoor/IMG_20160418_005808.jpg)

#### 传动轴的最终固定

当你觉得舵机位置基本调整好了以后，记得把舵机拿出来，**在传动轴与舵机连接齿轮处涂上502胶水固定传动轴**（最好先向外松一下齿轮，涂上502后再按紧），否则转几次就飞出来了，当然此时传动轴已经固定死在也拿不下来了。等待胶水完全风干，。最后再进行开锁和松锁的测试，确保舵机正常旋转、力道十足。

#### 舵机的最终固定

当你觉得舵机的位置非常完美的时候，就可以用热熔胶进行舵机的最终固定了。规则只有一个：**粘紧就好**。没错，就是那么简单粗暴！！！上图：

![舵机的固定]({{site.baseurl}}/img/bluedoor/IMG_19700101_113610.jpg)

注意别粘到左边的反锁装置（红色框），不然反锁功能又没了……

![注意反锁装置]({{site.baseurl}}/img/bluedoor/IMG_19700101_114021.jpg)

等待热熔胶硬了之后再把门锁后盖盖上、螺丝扭上。如果舵机与后盖之间有空隙就塞点什么弄紧点。如果舵机位置太高后盖盖不上，那就用力点吧……（之前都说了注意舵机位置不要太高的……）**最后再输出信号开锁松锁试一下是否正常**……

![盖上后盖]({{site.baseurl}}/img/bluedoor/IMG_19700101_114244.jpg)

如果门锁能正常开关，那么恭喜你，最难的门锁改装部分已经完成，你离成功也不远了……

<br/>

---

## 九、门锁整体安装

### 1、门外LED指示灯的安装

其实这个LED指示灯是可有可无的，当初还是手动拉绳开门时，外面的人不知道门开了，每次都要我大喊一声：”推~~~推~~~推（没人鸟我）。。。你妹推门啊！“所以就想到弄个LED灯提示一下，然后就一直保留到现在……额好像扯远了。

那么其实LED指示灯的安装也很简单，门外固定一个LED，在LED正负极焊上两根长长的导线，然后从内侧门缝由外而内穿过，导线末端焊上母头杜邦线即可。你也可以像我一样加个“灯亮请推门”之类的提示语：

![门外LED]({{site.baseurl}}/img/bluedoor/P60511-235545.jpg)  

![门内走线]({{site.baseurl}}/img/bluedoor/P60511-235708.jpg)

![杜邦线母头]({{site.baseurl}}/img/bluedoor/P60511-235850.jpg)

### 2、门锁状态开关的制作与安装

增加“门锁状态开关”是很有必要的，单片机通过状态输入来获取当前门锁的状态，门有没有关（准确来讲有没有关好），这是实现“开门自动复位门锁”、“忘关门提醒”和“防盗报警”功能的前提。我采用的制作方法很简单：两枚订书钉+一小片铝片。

#### 开关触点的制作

在门侧边上用两枚订书钉作为开关触点。直接用掰开的订书机在木门侧边订两枚订书钉(暴力一点应该能钉进去)，**开孔间隔5mm,距门内侧边沿10mm**，位置像这样：

![订书钉位置]({{site.baseurl}}/img/bluedoor/P60512-000233.jpg)

然后用烙铁在订书钉另一头弯折处焊公对母头杜邦线（我这里焊了双公头的面包板线再用双母头杜邦线作延长），不影响门的正常关闭、不会掉就行：

![触点杜邦线]({{site.baseurl}}/img/bluedoor/P60512-000115.jpg)

#### 开关触片的制作

从垃圾桶捡来一个易拉罐（有时捡垃圾比捡钱还爽），用剪刀把罐体展开成平面，剪下大约12*55mm的条状铝片，亮面朝外对折两次：

![易拉罐剪裁]({{site.baseurl}}/img/bluedoor/P60512-112125.jpg)

![铝片对折]({{site.baseurl}}/img/bluedoor/P60512-112219.jpg)

把门关上，在触点对应的门框位置做标记，把折起来的铝条用胶带固定两端在门框上：

![门框触片]({{site.baseurl}}/img/bluedoor/P60512-000309.jpg)

![关门]({{site.baseurl}}/img/bluedoor/P60512-000722.jpg)

#### 接触面打磨

由于订书钉和易拉罐表面有涂层，会影响导通性，所以最后一定记得**用砂纸对接触面进行打磨**。

### 3、门锁的安装

把改造好的门锁装回门上，记得把舵机线引出。用螺丝把外壳电路固定在门上，插接好舵机线、LED线、门锁状态开关线，插上USB电源线供电，打开APP，万事俱备，芝麻开门！

![安装门锁]({{site.baseurl}}/img/bluedoor/P60512-000737.jpg)

![安装电路]({{site.baseurl}}/img/bluedoor/P60512-000757.jpg)

<br/>

---

## 十、最后的废话

当初急急忙忙把展览模型做出来，然后就一直不断改进，加上DS1302实时时钟、忘关门提醒及防盗报警功能，现在舍友正在编写我们自己安卓APP控制软件（我们IT文化节APP大赛的作品，本来想自己写，但不会安卓~~~），门锁功能基本定型了，是时候汇报一下了。

今晚把门锁程序最后一点代码注释整理好，如释重负，但舍友的APP依然需要努力。

半年前，致力实现“偷懒开门”的我尝试写了舵机的控制程序，也就是门锁最初版本的程序，一直断断续续增删功能，没有高大上的编程技巧，全凭热情，维护着这个小小的个人项目。直到把DS1302时钟芯片加上，把“命令系统”重写一遍，代码才规范许多，看起来舒服许多。单片机部分算告一段落了，接下来，看舍友咯——尽情期待我们的专属App！

<br/>

---

## 十一、总结

这里一般都会写一些什么“经过这次制作学到很多，收货很多……”之类的废话，但我想说，这次制作并没有什么太大技术含量，也没有没有学到什么，单片机、串口、PWM等都是之前就知道的（能做出来一切都好像在意料之中），不过舵机和蓝牙模块倒是新知识，但这些技术不是重点（比我这个渣渣牛逼的大神一抓一大把），重点是**从生活的实际需求出发去制作**。都不是什么高深莫测的东西，而是用来解决生活中的实际问题的。或许对于别人来说，写不出一个程序没什么关系，但我写不出的话连门都进不了啊！压力山大有木有。

##### 所做的一切，不是装逼、不是无病呻吟，只是想尽量用有限的知识让生活更美好、更有趣好玩，仅此而已。

——20160512

<br/>

---

项目github地址：[https://github.com/shaoguoji/bluedoor](https://github.com/shaoguoji/bluedoor)

<br/>

---

有什么问题可以评论留言。对了，有成功做出来的朋友也可以留个言，毕竟我对这随手做的东西没抱太大希望……

<br/>

<br/>

> 参考文章
> 
> [HC-05蓝牙模块的参数设置与使用-蓝牙门锁开发笔记]({% post_url 2016-04-16-usage-of-bluetooth-model %})
> 
> [宿舍蓝牙门锁命令详解-蓝牙门锁开发笔记]({% post_url 2016-05-12-bluedoor-command %})
