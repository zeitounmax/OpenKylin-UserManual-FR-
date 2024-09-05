# 关于 ***UTF8_STRING*** 

> 该页面编写于2000年，当时正在初步部署 ***UTF8_STRING*** 。此后，对 ***UTF8_STRING*** 的支持已经得到普及。特别是关于XFree86的任何声明仍适用于X.Org，并且所有最新的工具包都对 ***UTF8_STRING*** 有完全支持。

> 据我所知，下面提供的草案仍然是 ***UTF8_STRING*** 唯一权威规范。

 ***UTF8_STRING*** 是一个由X.Org项目正在标准化的新的X11编码方案。 ***UTF8_STRING*** 允许在 X11 客户端之间无缝交换国际文本数据；其主要应用是选择交换（剪切和粘贴）。

 ***UTF8_STRING*** 经过精心设计，保持了与现有X11客户端的兼容性；启用 ***UTF8_STRING*** 功能的客户端将使用 ***UTF8_STRING*** 与其他新客户端进行交换，在与旧客户端通信时则回退到复合文本（COMPOUND_TEXT，是一种多字符集数据（如多语言文本）的编码格式）。

 ***UTF8_STRING*** 支持应由您正在使用的用户界面库提供，因此对程序员来说应该是完全透明的。仅在你编写使用原始 Xlib库<sup>[1]</sup> 或 Xt库<sup>[2]</sup> 的函数访问选择的新部件时，才需要注意 ***UTF8_STRING*** 。

当前版本的 ***UTF8_STRING*** 的文档可以在这个 ***UTF8_STRING*** 草案中找到。

## XFree86库中的 ***UTF8_STRING*** 支持

自4.2版本起， XFree86库 <sup>[3]</sup>中包含了对 ***UTF8_STRING*** 的大量支持（其中大部分由Bruno Haible完成）。由于这些扩展，只需几行代码即可将 ***UTF8_STRING*** 支持添加到现有客户端中。

值得注意的改变包括：

- 增加了 ***XICCEncodingStyle*** 枚举类型，其中包括一个表示 ***UTF8_STRING*** 的成员 ***XUTF8StringStyle*** ；

- 这些实用程序能够正确理解和解析包含 ***UTF8_STRING*** 的文本属性数据；包括 ***XTextPropertyToStringList***、***XmbTextPropertyToTextList***和 ***XwcTextPropertyToTextList***。

- 所有用于构造文本属性的实用程序都接受 ***XUTF8StringStyle*** 作为属性样式；包括 ***XmbTextListToTextProperty*** 和 ***XwcTextListToTextProperty*** 。

此外，还包含了一些与现有API类似的UTF-8类型；这些包括 ***Xutf8SetWMProperties***、 ***Xutf8TextListToTextProperty*** 、 ***Xutf8TextPropertyToTextList***，以及 ***Xutf8TextEscapement*** 、 ***Xutf8TextExtents*** 、 ***Xutf8TextPerCharExtents*** 、 ***Xutf8DrawText*** 、 ***Xutf8DrawString*** 、 ***Xutf8DrawImageString*** 、 ***Xutf8ResetIC和Xutf8LookupString*** 。

## 客户端中的 ***UTF8_STRING*** 支持

许多X11客户端完全支持 ***UTF8_STRING*** 进行选择交换；其中包括：

- XTerm的XFree86版本（还请参阅手册页）；
- Xaw文本部件的XFree86版本（因而也适用于xedit工具）；
- KDE 2应用程序；
- 当前版本的Gtk+工具包，因此也适用于当前版本的Gnome；
- Mozilla的X11版本及其变体
- 当前版本的Tk工具包，因此也适用于使用Tcl/Tk编写的应用程序。

为了方便地将任意BMP字符插入到所有正确的国际化X11客户端中（包括在一定程度上不支持 ***UTF8_STRING*** 的客户端），你可能想要使用ucm实用程序。

## 更多信息

更多有关在自由Unix-like操作系统下的Unicode支持的详细信息，请参阅Markus Kuhn的Unicode FAQ。


### 注释

> 1：X11协议的C语言接口库，用于与X11服务器进行通信

> 2：X Toolkit Intrinsics，X桌面工具包

> 3：XFree86是一个自由开源的X11实现，早期广泛用于Unix-like操作系统上的图形界面系统

> 翻译：DSOE1024 校对：DSOE1024