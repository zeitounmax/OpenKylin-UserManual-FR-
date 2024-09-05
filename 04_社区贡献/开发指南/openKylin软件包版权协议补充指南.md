---
title: openKylin软件包版权协议补充指南
date: 2022-09-16 14:36:45
tags:
---

# openKylin软件包版权协议补充指南

openKylin 对待软件包版权和许可证信息的态度十分严格，在 openKylin 中的所有软件包都应包含相应的版权信息:
1. 保证每一个代码源文件的头部，有相关license的简要声明:  
    1.1 **针对引用的其他开源项目的文件，确保原文件的copyright保留，如有修改，只能新增copyright，不能删除原copyright**  
    1.2 针对自己新增的代码文件，按相应license的要求，在头部加入license简要声明；  
2. 在软件包的一级目录下创建以“LICENSE”或“COPYING”为名的文件，放入整个项目的许可证信息；
3. 在debian/copyright 文件中提供项目所有源文件版权信息的摘要；

 **推荐新的开源项目使用[Mulan PSL v2协议](http://license.coscl.org.cn/MulanPSL2)** 。Mulan PSL v2与BSD类许可证类似，有很好的兼容性：BSD、MIT类宽松许可证兼容Mulan PSL v2协议；Mulan PSL v2协议兼容Apache License v2.0、L/GPLv2、L/GPLv3等许可证。即，许可在BSD、MIT类许可证下的代码可以贡献到使用Mulan PSL v2协议的项目中；使用Mulan PSL v2协议的代码可以贡献到Apache License V2.0、L/GPLv2或L/GPLv3等项目中。

## 头部注释

针对项目内的.c/.cpp/.h/.py/.java 等源文件，在头部注释添加相应版权声明，相应的声明可在许可证文本信息中找到，也可以参考遵循相同许可证的项目：

以项目ukui-foo，许可证“Mulan PSL v2”，copyright主体 Zhang Three 为例，需要在每个源文件头部注释中插入：

```
Copyright (c) 2022 Zhang Three
ukui-foo is licensed under Mulan PSL v2.
You can use this software according to the terms and conditions of the Mulan PSL v2.
You may obtain a copy of Mulan PSL v2 at:
         http://license.coscl.org.cn/MulanPSL2
THIS SOFTWARE IS PROVIDED ON AN "AS IS" BASIS, WITHOUT WARRANTIES OF ANY KIND,
EITHER EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO NON-INFRINGEMENT,
MERCHANTABILITY OR FIT FOR A PARTICULAR PURPOSE.
See the Mulan PSL v2 for more details.
```

以项目ukui-foo，许可证GPL-3.0+，copyright主体 Zhang Three 为例:
```
Copyright (C) 2022 Zhang Three

This program is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 3, or (at your option)
any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, see <http://www.gnu.org/licenses/>.
```

> 企业用户使用企业copyright

## 项目license文件
依据项目遵循的许可证情况，需要将相应的许可证完整内容（openKylin可从/usr/share/common-license目录下获取, Mulan PSL v2协议从[官网](http://license.coscl.org.cn/index.html)获取），拷贝到项目的一级目录下，重命名为"LICENSE"或“COPYING”。
以项目ukui-foo，许可证GPL-3为例：
```
cd ukui-foo
cp /usr/share/common-license/GPL-3 COPYING
```
## debian/copyright文件
### 文件语法

debian/copyright 文件必需要传达所有规定的上游信息、版权声明和许可细节，该文件由两个或更多段落组成。至少，该文件必须包括一个标题段和一个文件段。

### 段落

有三种类型的段落。文件中的第一个段落被称为标题段。其他每一段都是 Files 段或者是独立的 License 段。

#### 标题段

以下字段可以出现在标题段中

  - Format: 必选
  - Upstream-Name: 可选.(建议)
  - Upstream-Contact: 可选.(建议)
  - Source: 可选.(建议)
  - Disclaimer: 可选.
  - Comment: 可选.(建议)
  - License: 可选.
  - Copyright: 可选.

标题段中的 Copyright 和 License 可以补充但不能取代 Files 段。它们可以用来总结整个软件包的贡献和再分配条款，例如，当一个作品结合了允许性和自由复制的许可时，或者记录一个汇编的版权和许可。在标题段中可以只使用 License，单独使用 Copyright 就没有意义了。

#### Files 段(可重复)

对文件的版权和许可证的声明是在一个或多个段落中进行的。在最简单的情况下，可以使用一个段落，它适用于所有文件，并列出所有适用的版权和许可证。

以下字段可以出现在一个文件段落中。

  - Files: 必选
  - Copyright: 必选
  - License: 必选
  - Comment: 可选.

#### 独立的 License 段落（可选，可重复）。

当一组文件多重许可，或同一许可多次出现时，你可以使用单行许可字段和独立的许可段落来扩展许可简称。

以下字段可以出现在独立的许可段落中。

  - License：必选
  - Comment：可选

### 字段

以下字段是定义在 debian/copyright 中使用的。

#### Format

类型：单行值；

该文件格式遵循规范的地址，如：

- Format: https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/。

#### Upstream-Name

类型：单行值；

上游使用的软件名

#### Upstream-Contact

类型：基于行的列表;

到达上游项目的首选地址。可能是自由格式的文本，但按照惯例，通常会写成 RFC5322 地址或 URI 的列表。

#### Source

类型：格式化文本，无概要；

上游来源的解释。通常这将是一个 URL，但它可能是一个自由格式的解释。如果上游源已被修改以删除非自由部分，则应在此字段中进行说明。

#### Disclaimer

类型：格式化文本，无概要；

此字段可用于非免费和贡献包(non-free)的情况

#### Comment

类型：格式化文本，无概要；

此字段可以提供附加信息。

#### License

类型：格式化的文本，带有简述；

在标题段中，这个字段给出了整个软件包的许可信息，它可能不同于每个文件许可信息的所有组合或是它们的简化。在 Files 段中，这个字段给出了该段的文件字段中所列文件的许可条款。在一个独立的许可段落中，它给出了那些引用它的段落的许可条款。

第一行：许可证的缩写名称，或提供替代方案的表达方式。[标准缩写](https://dep-team.pages.debian.net/deps/dep5/#license-short-name)。

剩下的几行：如果这里留空，文件必须包括一个独立的许可证段落，与第一行列出的每个许可证短名称相匹配。否则，这个字段应该包括许可证的全文，或者包括一个指向/usr/share/comm-licenses下的许可证文件的指针。这个字段应该包括所有需要的文本，以满足 Debian Policy 对包含软件发行许可的要求，以及任何许可要求，即在二进制软件包中包含保修免责声明或其他声明。

#### Copyright

类型：有格式的文本，无概要；

一个或多个自由格式的版权声明。任何格式都是允许的；
请看下面的例子，以了解如何组织这个字段使其更容易阅读。在标题段中，这个字段给出了整个软件包的版权信息，它可能不同于或简化于所有每个文件的版权信息的组合。在 Files 段落中，它给出了适用于文件模式所匹配的文件的版权信息。

版权字段收集了本段文件的所有相关版权声明。并非所有的版权声明都适用于每一个单独的文件，一个版权人的出版年份可能会被收集在一起。例如，如果文件A有：

 - Copyright 2008 John Smith
 - Copyright 2009 Angela Watts

和文件B有：

 - Copyright 2010 Angela Watts

涵盖文件A和文件B的版权字段只需包含；

 - Copyright 2008 John Smith
 - Copyright 2009, 2010 Angela Watts

版权字段可以包含完全复制的原始版权声明（包括 " Copyright "一词），也可以缩短文本，只要不牺牲信息即可。

#### Files

类型：以空格分隔的列表；

表示本段规定的许可证和版权所涵盖的文件的模式列表。

文件字段中的文件名模式是使用简化的shell glob语法指定的。模式由空格隔开。

  - 只有通配符 * 和 ? 适用；前者匹配任何数量的字符（包括没有），后者匹配单个字符。两者都匹配斜线(/)。

  - 匹配从源码开始的路径名。因此，"Makefile.in " 只匹配源码根模流的文件，但 "\*/Makefile.in" 匹配所有目录层级的文件。


### License 规范
#### 语法

License名称是不区分大小写的，并且不能包含空格。

在多重许可的情况下，当用户可以在不同的许可之间进行选择时，以及当作品的使用必须同时遵守多个许可的条款时，许可的短名称以 "or" 来分隔。

例如，这是一个简单的“GPL 版本 2 或更高版本”字段：

License: GPL-2+

这是双重许可的 GPL/Artistic 作品，例如 Perl：

License: GPL-1+ or Artistic

这适用于其中包含 GPL 和经典 BSD 代码的文件：

License: GPL-2+ and BSD

对于最复杂的情况，逗号用于消除 ors 和 ands 的优先级，并且优先级高于 or，除非前面有逗号。例如：

A or B and C 意味着 A or (B and C).

A or B, and C 意味着 (A or B), and C.

这是一个包含 Perl 代码和经典 BSD 代码的文件：

License: GPL-2+ or Artistic-2.0, and BSD

一个有OpenSSL例外的GPL-2+作品实际上是一个双重许可的作品，它既可以根据GPL-2+重新发布，也可以根据GPL-2+和OpenSSL例外重新发布。因此，它被表述为：

License: GPL-2+ with OpenSSL exception

debian/copyright可以参考[示例](https://gitee.com/openkylin/ukui-session-manager/blob/openkylin/yangtze/debian/copyright "示例")

## 如何检查 copyright

社区上游已经有程序能够帮助我们检查 copyright 的写法，这些程序目前都遵循 DEP-5 的规则。

#### licensecheck 

维护者可以通过 licensecheck 命令对源码文件头部进行检查

 - $ licensecheck * -r

#### cme

使用 cme命令根据源码文件头部自动更新 debian/copyright 文件

 - $ cme update dpkg-copyright
