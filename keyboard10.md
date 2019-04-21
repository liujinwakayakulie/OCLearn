# 从零开始DIY机械键盘-零代码版

## 机械键盘原理概述图

## 整体采用方案如下：

主控硬件：arduino pro micro版本

主控软件：qmk固件方案

连接方案：40%pcb

外壳方案：定位板弯折

轴：拆机樱桃黑轴

键帽：垃圾包键帽

## 搜集相关经验帖如下（如果打不开，请翻墙）

[http://tieba.baidu.com/p/5448123499?see\_lz=1&pn=2](http://tieba.baidu.com/p/5448123499?see_lz=1&pn=2)

[http://sync.sh/\#33](http://sync.sh/#33)

[http://tieba.baidu.com/p/4592747695?see\_lz=1](http://tieba.baidu.com/p/4592747695?see_lz=1)

[https://www.jianshu.com/p/ea3d2ce1e691](https://www.jianshu.com/p/ea3d2ce1e691)

[http://dy.163.com/v2/article/detail/DK51DVE40511D7HR.html](http://dy.163.com/v2/article/detail/DK51DVE40511D7HR.html)

[https://forum.51nb.com/thread-1650426-1-1.html](https://forum.51nb.com/thread-1650426-1-1.html)

## 文中提到的工具类网站如下（如果打不开，请翻墙）

[http://www.keyboard-layout-editor.com](http://www.keyboard-layout-editor.com)键盘佩列网站

[http://builder.swillkb.com/](http://builder.swillkb.com/)定位板网站

[https://kbfirmware.com](https://kbfirmware.com) qmk对接佩列生成固件文件网站

[https://github.com/qmk/qmk\_toolbox/releases](https://github.com/qmk/qmk_toolbox/releases) qmk固件刷入工具

[https://docs.qmk.fm/\#/](https://docs.qmk.fm/#/) qmk文档

[http://download.savannah.gnu.org/releases/avrdude/](http://download.savannah.gnu.org/releases/avrdude/) 刷入所需**avrdude**工具网站

[https://github.com/ruiqimao/keyboard-pcb-guide](https://github.com/ruiqimao/keyboard-pcb-guide) kicad学习网站

[http://haipeng.me/2018/05/24/kicad\_advanced\_tutorial\_1\_new\_component/](http://haipeng.me/2018/05/24/kicad_advanced_tutorial_1_new_component/)kicad中文教学

[https://github.com/Biacco42/ProMicroKiCad](https://github.com/Biacco42/ProMicroKiCad)主控元件库封装

## 特别感谢

院长 1800佩列优联pcb作者，QQ群号375379462

## 正文

### 1、主控

#### 1-1确认键盘佩列

[http://www.keyboard-layout-editor.com/\#/](http://www.keyboard-layout-editor.com/#/)

上方连接为确认佩列的网站，是一位大神做的开源项目。打开后可以看到下图，红色为我的一些标注

![](/assets/佩列网站介绍.png)

如上图所示，我们可以选择预设好的键盘佩列直接用，也可以在预设好的键盘佩列基础上改，基本用到的就是修改宽高XY这四个属性。下图为我的键盘佩列

![](/assets/我的佩列.png)

我们需要将Raw data的数据保存下来，生成定位板、生成固件、之后改进都需要用到。

我这边用键位除了1X还有一个2.75X和1.25X，制作这样的佩列有两个原因，一：键帽好配，二：可以有位置带上方向键。

#### 1-2制作固件

[https://kbfirmware.com/](https://kbfirmware.com/)

上述网站为QMK提供的固件生成网站，简单来说作用就是将1-1中生成的Raw data数据转化为能烧录到单片机中的程序，让单片机能实现我们设计的佩列。

如下图我们将Raw Data数据复制到输入框中，点击import  
![](/assets/固件-1.png)

点击之后会出现如下界面

![](/assets/固件-2.png)

总体概述，首先我们可以看到7个tab，这个就是我们生成固件的设置项。

##### 1-2-1键盘矩阵设置

由于引脚有限，键盘基本都是采用矩阵方案，也就是会有行列的概念，轴体一个引脚连接到行，另一个引脚连接到列。如上图，可以看到生成了一个4行12列的矩阵键盘，也就是说总共需要用到16个引脚，我采用的pro micro满足此需求。再往下是二极管的方向设置，默认是从列到行，也就是二极管正极指向列，负极指向行，电流从列流到行。至于为什么需要加装二极管，可以看我贴上的经验帖，里面有提到。

##### 1-2-2主控与引脚设置

切换到pins，我们可以看到主控和引脚的选择。鼠标移动到？可以看到常见的主控有alps64 arduino teensy gh60。我们根据自己购买的主控型号选择对用选项，然后再根据主控的引脚，设置对应的列与行的引脚。具体大家采用了哪个主控可以自行百度其引脚定义，[pro micro的引脚定义](https://cdn.sparkfun.com/datasheets/Dev/Arduino/Boards/ProMicro16MHzv1.pdf)。因此我的row定义为D1 D0 D4 C6，col定义为D7 E6 B4 B5 B6 B2 B3 B1 F7 F6 F5 F4。

![](/assets/固件-3.png)

##### 1-2-3键位设置

之前我们已经设置过佩列了，但是我们想对其进行修改或者增加映射层（层编号从0开始），可以直接点击Keymaps进行修改。如图，直接点击想修改的按钮，然后在键盘上直接敲击修改后的键位，就能直接修改了。

![](/assets/固件-4.png)

由于我这里设计的键盘键位本身过少，无法满足正常使用需求，所以需要增加键位映射。如下图在第0层时，点击想要映射功能键，选择fn，点击MO\(此功能键位的意思是在按压状态下开启映射，一旦松开按键就回到了第0层，具体的说明可以看[qmk官方文档](https://docs.qmk.fm/#/)。\)，最下方点击向上按钮增加开启的层。如果看到类似MO\(1\),说明设置成功，这个键位就变成了开启第1层的开关。

![](/assets/固件-7.png)

看下图我们注意红框内，将layer加到1，上方键位都是未设置状态，我们按自己需要设置就好了。同样的操作可以在第0层中再增加开启第2层、第3层的功能键。

![](/assets/固件-8.png)

由于我本身没有设置组合键所以MACROS这个tab没有设置

##### 1-2-4保存json文件

打开SETTRINGS tab点击红框内按钮，我们会保存一个.json的文件，如果我们在之后想更改设置，可以在打开网站时直接选择upload json文件，就可以在此前我们设置的基础上修改了。

![](/assets/固件-5.png)

##### 1-2-5生成固件文件

打开COMPILE tab点击download.hex按钮我们就可以生成固件文件下载到电脑中。此前我们所有的设置都在固件文件中。

![](/assets/固件-6.png)

#### 1-3烧录文件

由于我们之前所有的操作都是在pc端进行，然而程序需要运行在主控中，所以这中间有一个跨平台的概念，也就是程序编写与运行不在同一个平台，因此就需要一个工具将程序从PC端烧录到主控中。由于我们的固件生成借助了QMK，QMK也提供了对应的烧录工具

[https://github.com/qmk/qmk\_toolbox/releases](https://github.com/qmk/qmk_toolbox/releases)烧录工具网站

直接使用烧录工具会提示我们下载avrdude.exe我们可以在下面网站下载。

[http://download.savannah.gnu.org/releases/avrdude/](http://download.savannah.gnu.org/releases/avrdude/)刷入所需**avrdude**工具网站

烧录过程如下。

电脑连接arduino，电脑端会看到串口工具，此时arduino还不会被电脑是被键盘。我们打开烧录工具，选择此前下载的.hex文件，进行烧录即可，具体进度可以查看log。

我们可以借助此工具多次烧录，如果被识别为键盘是无法烧录的，需要短接GND与REST两次，arduino会在短时间内被识别成串口工具，我们就在此时间段内再次烧录即可。烧录成功后我们可以短接col和row，测试是否成功烧录了此前的设置键位。

至此主控就已经完成了。

### 2、PCB

绘制PCB我使用的kicad这个软件，之所以使用这个软件一方面是github上有使用此软件绘制键盘pcb的教程，另一方面是此软件免费。

以下为0基础需要用到的两个网站。

[https://github.com/ruiqimao/keyboard-pcb-guide](https://github.com/ruiqimao/keyboard-pcb-guide)    kicad学习网站

[http://haipeng.me/2018/05/24/kicad\_advanced\_tutorial\_1\_new\_component/](http://haipeng.me/2018/05/24/kicad_advanced_tutorial_1_new_component/)   kicad中文教学

具体的教程上面连接中说的很详细了，我这里就不班门弄斧了，只是有几处坑说一下

* 樱桃轴的封装库在第一个连接中有，需要手动添加
* promicro的封装库在[https://github.com/Biacco42/ProMicroKiCad](#)中有，需要手动添加
* 在绘制pcb的时候除了樱桃轴放在F面，其他元件一律放在B面
* 生产文件生成请参照第一个链接。

### 3、外壳制作

我们将1-1中的Raw data数据复制到[http://builder.swillkb.com/](http://builder.swillkb.com/)中。

我们主要修改的是edge Padding（四周填充）和custom polygons这两个。

我这里是什么都不改直接生成默认图形后，下载 .dxf文件，然后使用AutoCad进行简单修改。我也是边查资料边画，画的不专业，下面是我绘制的设计图。

至此主控、pcb、外壳三大件已经制作完成。余下的工作就是组装了，大家最好在全部焊接前分模块测试一下，包括主控是否正常工作，pcb是否有问题，二极管方向时候正确等等。

