# 客户端间Unicode文本交换


> 2000年1月4日 Juliusz Chroboczek <jch@xfree86.org>

> 草案!


摘要：提出了一种新的属性类型和选择目标

UTF8字符串是携带UTF-8编码的Unicode文本。这个文档只处理UTF8字符串的定义;任何相关的API扩展应在将来的文件中加以说明。

这份文件目前没有任何官方权威;
然而,它是相当稳定的。


## 简介及背景


Unicode（UNICODE2, Unicode）是一个带有野心的编码字符集，适用于所有已知文本数据的交换当前和历史的脚本。Unicode字符表是由称为代码点的无符号整数索引。按照惯例，我们为编码点89AB(十六进制)的Unicode字符编写U+89AB。

Unicode字符集的码点对码点完全相同于ISO10646。主要区别在于Unicode定义了文本处理算法，而ISO10646则没有。

UTF-8是一种将Unicode文本编码为8位字节字符流的技术。UTF-8编码具有以下优点和属性:

- 它兼容的7位ASCII，在该映射编码为8位字节的任意ASCII字符字符串UTF-8不需要任何转换;

- 它是无状态的：在任何时候都可以开始解释一个UTF-8编码的字符串;

- UTF-8和16位或32位Unicode值流之间的转换在计算上是微不足道的。

加上Unicode的通用性，这些属性构成了UTF-8纯文本的理想交换格式。特别是，UTF-8允许客户端无缝交换选择ICCCM包含独立于语言环境的多语言文本。

多语言文本目前首选的X11交换格式是复合文本（CTEXT），它是ISO2022的一个大子集。因此，它需要有状态解析，并且是人为在所有用途相同的字符之间的区别，但碰巧属于不同的字符集。

本文提出了一个新的属性类型和选择目标UTF8字符串，携带UTF-8编码的Unicode文本。这个文档只处理UTF8_STRING的定义;API扩展制作更方便的UTF8_STRING操作将在以后的文档，并且不在本文档的范围内。


## 规范的一部分


本文档建议原子名UTF8字符串（UTF8_STRING）应该是在X.Org注册。这个原子的主要用途是性质类型和选目标，尽管其用于其他目的是不排除在外。

如果属性类型为UTF8_STRING，它应该携带8位数据(即指定格式8)。它应该被解释为编码的字符串根据Unicode标准定义的UTF-8，版本2.0或任何更新的版本。数据仅由UTF-8字符串组成;特别地，它不携带签名，也没有零字节。

类型的属性中的数据没有更多的暗示UTF8字符串。特别是，它不需要在规范的形式，并可能
包含任何Unicode控制字符，包括但不限于，C0和C1控制字符，段落和换行，方向标记，或平面14语言标记。

如果选择请求指定目标UTF8字符串，则选择holder应该使选择作为UTF-8字符串可用，带
输入UTF8字符串，格式为8。

为了互操作性的利益，选择的语义目标文本不改变;特别是，用a来回答选择UTF8字符串类型的类型到指定TEXT的请求明确不允许。


## 客户端指引


内部使用文本编码的客户端可以很容易地映射到鼓励使用UTF8作为首选交换格式。我们鼓励这些应用程序遵守下面的指导方针。这些指导方针都不应被视为规范。

## 组合字符


预计在短期内，将会有一些应用程序不能正确处理组合字符。这也是期望任何可以接受组合字符的应用程序将能够正确地解释预先编写的形式。对于这个因此，建议客户端在只要有可能，它们的预先组合形式。

当然，在实际可行的情况下，我们也鼓励客户端接受组合字符。

## 行和段落分隔符


传统上，X客户端使用C0字符0x0A换行符来分离行文本。我们建议通过这个公约转换为UTF8字符串属性类型。

客户端应该提供UTF8_STRING类型的选择单独的线和一个U + 000换行字符。段落应该由两个或更多次出现的序列分隔U+ 000a进线。

Unicode引入了两个新的控制字符，用于分隔行和段落，U+2029段落分隔符(PS)和U+2028行分隔符。我们建议这些字符不应该被使用选择供应商。然而,我们强烈建议选择请求者接受并解释这两个特征;一个建议策略是将U+2028行分隔符解释为单个U+000A行FEED和U+2029段落分隔符作为两次出现的序列U+000A线进给。

Unicode BIDI算法与U+000A的交互换行将
需要澄清。现在，我们建议使用单个U+000A线
提要不应导致BIDI算法被重置，而序列
两次或两次以上出现U+000A换行应做到。

## C0和C1控制字符


Unicode包含两个范围的控制字符，分别是C0和C1，与ISO8859系列中的控制字符范围的编码。

除了U+000A换行符外，这些字符的使用应该避免选择提供者，因为它们没有很好的定义的语义。然而，选择请求者应该是准备接受任意控制字符。特别是，U+000C形式进料(FF)和U+000B水平制表(HT)是可能的由运行在X11下的应用程序使用，特别是终端模拟器。我们建议解释如下。

U+000C FORM FEED (FF)导致分页。它不应该导致要重置的BIDI算法的状态。简单地把它当作U+000A换行是一个合适的策略。U+000B水平表(HT)应被视为应用程序相关的线性空白的数量。只是对待它像U+0020空间是一个合理的策略。

任何其他的C0或C1控制字符都应该是简单的被选择请求者丢弃。


## 选择所有者的指导方针


希望进行选择的客户端，称为选择所有者以文本形式提供，应:

1. 响应类型为TARGETS的转换请求(至少)
TEXT, STRING, UTF8_STRING，可能还有COMPOUND_TEXT。

2. 响应带有目标UTF8字符串的转换请求没有初始签名和结束NULL的UTF-8编码字符串字节存储在类型为UTF8_STRING的属性中。为了最大化互操作性方面，这个字符串最好是预先组合好的形式，但客户端可以自由地使用任意的Unicode字符串当不需要转换为预合成形式时。生产这样的属性涉及生成合适的Unicode控制字符，例如U+000A LINE FEED，以及可能的方向标记和面14语言标签。

3. 来响应带有目标STRING的转换请求选择ISO 8859-1，并以属性表示结果字符串类型。不映射到ISO 8859-1的字符应该是以特定于应用程序的方式替换，或者通过重新映射到类似ISO 8859-1字符(例如，映射Unicode引用)标记为ASCII引号)，或替换为易于识别的ASCII
字符，如' ?'或' #'。

4. 可选地用target响应转换请求COMPOUND_TEXT，具有特定于应用程序和语言环境的转换
将数据转换成复合文本[CTEXT]。确切的语义转换在本文档中没有描述。

5. 用多态目标TEXT响应转换请求检查所选文本是否可以精确地表示为ISO8859-1字符串。如果是这种情况，选择所有者应该按照第3点进行;否则，按照第4点继续进行。


## 请求者的指导方针


客户端，称为请求者，希望使用一个可能
以文本形式提供，应该:

1. 使用目标TARGETS进行转换请求，并检查目标UTF8_STRING、STRING和optional的可用性COMPOUND_TEXT和TEXT。

2. 如果找到了目标UTF8_STRING，请求者应该发出一个目标UTF8_STRING的转换请求。如果这个转换如果成功，请求者应该处理结果的UTF-8编码字符串以特定于应用程序的方式。字符串应该是解释为没有签名或终止NULL字节。在特别地，一个初始的U+FEFF不破零宽度空格或一个finalU+0000 NULL不应该被丢弃。

请求者应该健壮地处理不正确或过长的UTF-8序列。它应该能够任意解释或丢弃Unicode控制字符，包括U+000A LINE FEED，方向标记，以及平面14语言标记。

3.如果没有找到目标UTF8_STRING，或者在步骤中进行转换如果找到了目标COMPOUND_TEXT并且请求者愿意执行从COMPOUND_TEXT到其内部格式，请求者应发出转换请求与目标COMPOUND_TEXT。如果选择转换成功，则然后，请求者应该继续尽最大努力从将COMPOUND_TEXT格式转换为其内部格式。

虽然这一步是可选的，但强烈建议客户执行不要尝试它，因为COMPOUND_TEXT是目前唯一的广泛支持的选择目标和支持交换的属性类型超出ISO 8859-1表的文本。

4. 如果上述步骤2和步骤3都不成功，则目标如果发现字符串，客户端应该发出类型的转换请求字符串。生成的属性应该具有STRING类型并包含ISO 8859-1编码字符串。此字符串中的任何控制字符
应以特定应用程序的方式进行解释;在特别地，0x0 LINE FEED应该被解释为换行符。

方法进行转换选择多态目标TEXT，然后应该准备好接收属性使用任何文本编码，包括但不限于字符串和组合文本。


## 样例实现


指南上面的示例实现集成
使用Thomas Dickey的`XTerm `，补丁级别为108或更高。它是
可以从

http://www.clark.net/pub/dickey/

`XTerm `的这个实现有不同的版本
3.9.15及之后的XFree86。XFree86可以从

http://www.xfree86.org

## 当前实践

作者可用的客户端都不符合上述建议。许多客户端使用UTF-8字符串作为属性类型字符串(针对目标文本、字符串或两者兼而有之)，这违反了ICCCM约定(ICCCM)。有些客户端试图将Unicode字符串转换为复合文本，并使它们作为属性类型COMPOUND_TEXT可用，遵循ICCCM，但需要复杂的、有状态的转换哪一个不是一对一的，并且可能会被选择反过来请求者。已经发现其他客户端使用UTF-8编码字符串可以在依赖属性类型(如en_US.UTF-8)，它在运行的客户端之间进行选择传输在不同的地方非常困难或不可能。


## 与Unicode相关的其他格式


除了UTF-8, Unicode和ISO 10646都定义了16位格式称为utf - 16和32位格式称为utf - 32。ISO 10646此外，还定义了一种称为UCS-4的32位格式。

UTF-8和其他格式之间的转换是计算上的微不足道的。当限制为BMP时(Unicode字符集codepoints小于2 ^ 16),UTF-8携带最多50%的开销相比于UTF-16, 比UTF-32和UCS-4更紧凑。

出于这些原因，本文件不建议定义机制用于交换编码为UTF-16、UTF-32或UCS-4的数据，这样的机制只会导致混乱和互操作性问题，同时没有给用户带来明显的好处。似乎未来不太可能需要这种机制。


## 参考文献


- [ARBITRARY] Arbitrary Character Sets.   John McCarthy.   RFC 373, July
1972.

- [ASCII]    ISO/IEC 646:1991.   Information technology -- ISO 7-bit coded
character set for information interchange

- [CTEXT]    Compound Text Encoding, version 1.1.   Robert W Scheifler.

- [ICCCM]    David Rosenthal and Stuart W. Marks.   Inter-Client
Communication Conventions Manual, Version 2.0.

- [ISO10646] ISO/IEC 10646-1:1993.  Information technology -- Universal
Multiple-Octet Coded Character Set (UCS) -- Part 1:
Architecture and Basic Multilingual Plane.

- [ISO2022]  ISO/IEC 2022:1994 Information technology -- Character code
structure and extension techniques

- [UTF-8]    F. Yergeau.   UTF-8, a transformation format of ISO 10646.
RFC 2279, January 1998.

- [UNICODE2] The Unicode Consortium.   The Unicode Standard -- Version
2.0.   Addison-Wesley, 1996.   Supplemented by
The Unicode Standard -- Version 2.1, available from
http://www.unicode.org/unicode/reports/tr8.html

- [UNICODE]  The Unicode Consortium.   The Unicode Standard -- Version
3.0.   Addison-Wesley, 2000.