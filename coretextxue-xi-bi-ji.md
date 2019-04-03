# **CoreText学习笔记**

综合现有中文资料和苹果官方文档整理

##### 以下为引用文章地址

[xiaoxiaobukuang的专栏](https://blog.csdn.net/xiaoxiaobukuang)

[aron1992专栏](https://my.oschina.net/FEEDFACF)

[苏屹鸡专栏](https://blog.csdn.net/mangosnow/article/list/3?)

[Core Text官方文档](https://developer.apple.com/documentation/coretext?language=objc)

[Cocoa Text Architecture Guide](https://developer.apple.com/library/archive/documentation/TextFonts/Conceptual/CocoaTextArchitecture/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009459-CH1-SW1)

[Text Programming Guide for iOS](https://developer.apple.com/library/archive/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009542-CH1-SW1)

[Core Text Programming Guide](https://developer.apple.com/library/archive/documentation/StringsTextFonts/Conceptual/CoreText_Programming/Introduction/Introduction.html)

# 目录 

[文字在iOS的显示](#1)

### 1、文字在iOS的显示

![](/assets/textpg_intro_2x.png)先祭出一张神图镇楼

---

#### 1.知识准备

##### Characters and Glyphs

character是最小的具有意义的书面语言。character是一个抽象的概念。character必须在一定区域里以可识别的形状展现，这也就是Glyph的概念。，可识别就意味着一个character并不是和glyph一一对应的。一个character可以对应多个Glyphs.

![](/assets/glyph_a_2x.png)

这五个a就是不同的glyph，一个glyph也可以和多个character对应。连笔\(ligature\)造成了一个glyph对应多个character。

![](/assets/romanligatures_2x.png)



































