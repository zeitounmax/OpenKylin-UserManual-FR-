---
title: openKylin打包指南
description: 
published: true
date: 2022-06-23T06:51:53.339Z
tags: 
editor: markdown
dateCreated: 2022-05-17T07:44:07.346Z
---

# **openKylin打包指南**


### **关于本文档**


本文档为一个介绍DEB包制作的指导性文档，重点指导你完成第一个DEB包的制作，具体DEB包的制作细则则会分散在各个具体规范中，我们将不断更新和完善此文档。

写作本文档时，遵循下列指导原则：

-   仅提供概览，而忽略边界情况。（**Big Picture 原则**）

-   保持文字简短紧凑。（**KISS 原则**）

-   不重复造轮子。（使用链接指向**已有参考**）

-   专注于使用非图形的工具和控制台。（使用 **shell 例子**）

-   保持客观。（使用 [popcon](http://popcon.debian.org/) 等等）

### **1. 软件打包**


一个 openKylin 下的软件包通常包含一系列文件的集合，它们定义了应用程序或者库文件可以如何通过包管理器（比如apt，yum等）进行发行部署。作为一种替代源码编译安装的方式，打包，即制作DEB软件包，将会把应用程序的二进制文件，配置文档，man/info帮助页面等文件合并打包在一个文件中，从而使软件的安装变得简单起来。通过软件包管理器，用户可完成获取，安装，卸载，查询等一系列操作。

**1.1. 总体规则**

 openKylin 试图规范化多种多样的开源项目到一个连贯的系统。因此 openKylin 制定此打包指导来规范制作DEB的动作。

-    openKylin 遵守一般的[Linux基础标准(LSB)](https://gitee.com/link?target=http://www.linuxbase.org/)。该标准致力于减少各个发行版间的不同。

-    openKylin 也遵守[Linux文件系统层级标准(FHS)](https://gitee.com/link?target=http://www.pathname.com/fhs/)。该标准是关于如何管理
    Linux 文件系统层级的参考。

-   除了遵守这些一般Linux发行版都会遵守的一般规则，本文档规范化了为 openKylin 社区版打包的实际细节问题。

**1.2. 打包基础知识**

运用此文档创建DEB包之前，建议你先熟悉下面知识点，该两项是如何创建一个高质量的软件包所必须的，提供了诸多详细的细节。

|   | skill             |      | links                                                                   |
|---|-------------------|------|-------------------------------------------------------------------------|
| 1 | Debian政策文档    | 必选 | https://www.debian.org/doc/debian-policy/                               |
| 2 | Debian 维护者指南 | 必选 | https://www.debian.org/doc/manuals/debmake-doc/index.zh-cn.html |

**1.3. 关联文档**

如果你计划将软件引入到 openKylin 官方软件仓库，请参考
[ openKylin 贡献攻略](https://gitee.com/openkylin/docs/blob/master/%E7%A4%BE%E5%8C%BA%E5%8F%82%E4%B8%8E%E6%8C%87%E5%8D%97/openKylin%E8%B4%A1%E7%8C%AE%E6%94%BB%E7%95%A5.md)。

**1.4. 适用性**

一般来说，这些准则适用于 openKylin 的所有版本，包括非生命周期版本、生命周期版本以及开发版本。

指导方针也在一定程度上涵盖了进入 openKylin 的所有类型和交付场景的软件包。 openKylin 是一个社区版本，因此不能保证所有的规则是一成不变，当前其最核心最重要的基本原则，在可预期的未来是不会有大的变动。

**1.5. 技术提醒**

这里给出一些技术上的建议，参考行事可以让您与其他维护者共同维护软件包时变得更加轻松有效，从而让
 openKylin  项目的输出成果最大化。

-   让您的软件包容易除错（debug）。

-   保持您的软件包简单易懂。

-   不要对软件包过度设计。

-   让您的软件包拥有良好的文档记录。

-   使用可读的代码风格。

-   在代码中写注释。

-   格式化代码使其风格一致。

-   维护软件包的 git 仓库。

### **2. 打包规则**


每个操作系统都自成体系，彼此之间除了技术路线、里程碑不同之外，软件包的组织方式也有所不同。

其主要的区别集中在下面几个方面：

（1）不同的包管理器（fedora、openSUSE使用rpm、debian使用deb等）。

（2）维护不同的软件包列表，包括不同的软件版本。

（3）互相独立的软件包拆分规则。

（4）基于不同拆分规则，而自然形成的软件依赖关系图。

**2.1. 软件包管理器**

openKylin 不打算重复造轮子，使用DEB作为底座，附以apt、dpkg等来管理软件包。
DEB 格式是Debian 系统（包含 Debian 和 Ubuntu）专属安装包格式，配合 apt 软件管理系统，
成为了当前在 Linux下非常流行的一种安装包。
也许在不久的将来，如果DEB，apt等工具不能满足需求， openKylin 会考虑发起新的项目。

**2.2. 软件列表、软件选型**

 openKylin 有自己的软件列表集合，当前已经集成1000+软件包，还在继续丰富和完善。

 openKylin 的软件代码来源，是直接取自软件原生社区的稳定版本，同时按照此打包规范编写debian/\*规则文件打包并集成。

 openKylin 遵循Upstream First原则。

**2.3. 软件拆分规则**

对于下面这样的上游源代码示例，我们在这里给出使用 **debmake** 处理时一些典型的
multiarch 软件包拆分的场景和做法：

-   一个软件库源码 *libfoo-1.0*\*\*.tar.gz\*\*

-   一个软件工具源码 *bar-1.0*\*\*.tar.gz\*\*，软件由编译型语言编写

-   一个软件工具源码 *baz-1.0*\*\*.tar.gz\*\*，软件由解释型语言编写

| 二进制软件包   | 类型   | Architecture: | Multi-Arch: | 软件包内容                             |
|----------------|--------|---------------|-------------|----------------------------------------|
| lib foo1       | lib \* | any           | same        | 共享库，可共同安装                     |
| lib foo -dev   | dev \* | any           | same        | 共享库头文件及相关开发文件，可共同安装 |
| lib foo -tools | bin \* | any           | foreign     | 运行时支持程序，不可共同安装           |
| lib foo -doc   | doc \* | all           | foreign     | 共享库文档                             |
| bar            | bin \* | any           | foreign     | 编译好的程序文件，不可共同安装         |
| bar -doc       | doc \* | all           | foreign     | 程序的配套文档文件                     |
| baz            | script | all           | foreign     | 解释型程序文件                         |

### **3. 打包验证**


（1）你必须测试你的软件包看是否存在安装问题。debi命令可以帮助你测试所有生成的二进制软件包。

（2） 使用lintian检查你的 .changes
文件。lintian命令会运行很多测试脚本来检查常见的打包错误。
当然，要替换你自己软件包所生成的 .changes
文件的文件名。lintian命令的输出常带有以下几种标记：

-   E: 代表错误，确定违反了 Debian Policy 或是一个肯定的打包错误。

-   W: 代表警告，可能违反了 Debian Policy 或是一个可能的打包错误。

-   I: 代表信息，对于特定打包类别的信息。

-   N: 代表注释，帮助你调试的详细信息。

-   O: 代表已覆盖，一个被 lintian-overrides 文件覆盖的信息，但由于使用
    \--show-overrides 选项而显示。

（3）查看软件选型打包后的二进制包是否可以正常使用apt命令集install、remove、purge
和 upgrade。 整个测试过程应按照以下操作序列完成：

-   如果可能，安装前一个版本的软件包；

-   从前一个版本升级软件包；

-   降级软件包到前一个版本(可选)；

-   彻底删除该软件包；

-   全新安装该软件包；

-   卸载该软件包；

-   再次安装该软件包。

-   彻底删除该软件包；

如果这是你的第一个软件包，你应该使用其他版本号创建一个测试用的软件包来完成升级测试，这样可以避免将来的问题。

（4）安装升级后，验证：1）服务类的，验证start/stop/restart/reload；2）命令类的，至少要验证基本功能可用。

（5）软件包源码中自带tests，不能随意注释、删除、禁用，需要确保代码提交时，门禁中make
check自测用例通过。

（6）特别是软件选型升级后，对其他软件包的影响，很难独立判断，需要做集成测试。

### **4. 打包规范**


规则和规范是一个逐步完善的过程，需要确保已有的规则得到遵循。

**4.1. 来源可靠**

-   不要内嵌预编译好的二进制文件或库文件，软件包中包含的所有二进制文件或库文件都必须是从源代码包中被编译的。二进制固件类的文件可豁免。如果需要引入二进制，经由TC讨论后决定。

-   软件包应该尽量避免多个、分离的、上游的项目被捆绑到一个软件包中，力争做到一个软件包来源一个社区。

-   软件**应该**是开源软件，开源软件的定义参考[https://opensource.org/osd.html](https://gitee.com/link?target=https://opensource.org/osd.html)
    。如果非开源软件，经由TC讨论后决定。

-   集成没有法律风险的开源软件，[开源许可名录](https://gitee.com/link?target=https://opensource.org/licenses/alphabetical)。

-   debian目录文件内容要适配 openKylin ，做到正确、准确、清晰、简洁。如果引用了其他发行版内容，或来自原生社区，必须顶部说明。

-   存在于**黑名单**的软件**必须不能**引入。

-   每一个软件的引入决定，都作为案例，作为后续类似软件引入决策的参考。Technical
    Committ对软件引入原则的一致性负责。

**4.2. 架构支持**

-   软件包维护者应尽量保证在aarch64和x86_64，MIPS等几种架构上能编译成功，后续随 openKylin 对其它体系架构的支持，可能会增加构建的要求。

**4.3. 软件拆分**

-   软件的拆分需要按照 openKylin 软件拆分规则实施。

-   为所有二进制软件包在 **debian/control** 文件中创建对应的二进制软件包条目。

-   在对应的 **debian/二进制软件包名.install** 文件中列出所有文件的路径

**4.4. 命名规则**

-   软件包的名称由**软件包名称-软件包版本号**组成。如果所获取上游源代码的形式为
    **hello-0.9.12.tar.gz**，您可以将 **hello** 作为上游源代码名称，并将
    **0.9.12** 作为上游版本号。

-   组成 openKylin 
    软件包名称的字符选取存在一定的限制。最明显的限制应当是软件包名称中禁止出现大写字母。这里给出正则表达式形式的规则总结：

    上游软件包名称（**-p**）：[-+.a-z0-9]{2,}

    二进制软件包名称（**-b**）：[-+.a-z0-9]{2,}

    上游版本号（**-u**）：[0-9][-+.:\~a-z0-9A-Z]\*

     openKylin 修订版本（**-r**）： [0-9][+.\~a-z0-9A-Z]\*

-   版本比较的规则可以归纳如下：

    （1）字符串按照起始到末尾的顺序进行比较。

    （2）字符比数字大。

    （3）数字按照整数顺序进行比较。

    （4）字符按照 ASCII 编码的顺序进行比较。

**4.5. 格式规范**

使用 **debmake**
可以为软件包维护者创建符合deb包规范的必要模板文件。在生成的文件中，注释行以
**\#**
开始，其中包含一些教程性文字。您在将软件包上传至 openKylin 仓库之前必须删除或者修改这样的注释行。

在生成的模板文件中，以下文件是非常重要的：

-   **debian/changelog**
    文件记录了软件包的历史并在其第一行定义了上游软件包的版本和 openKylin 
    修订版本。所有改变的内容应当以明确、正式而简明的语言风格进行记录。

-   **debian/control**
    文件描述了当前软件包的会编译出的二进制包、编译依赖和安装依赖等。

-   **debian/rules** 文件描述如何编译这个软件包

**4.6. 依赖关系**

-   要保证软件包的编译依赖和安装依赖已经存在于 openKylin 仓库中。如果没有，需要一并打包引入。编译依赖和安装依赖需要自行确认，确保完整。尽量避免循环依赖。

-   **Depends**

该字段描述此软件包依赖的所有软件包集合，只有当它依赖的软件包均已安装后才可以安装。此处请写明所有你的软件包必须依赖的软件包，如果依赖的软件包没能安装那么该软件包有可能不能正常运行。

-   **Recommends**

这项中的软件包不是严格意义上必须安装才可以保证程序运行。当用户安装软件包时，所有前端软件包管理工具都会询问是否要安装这些推荐的软件包。**aptitude**
和 **apt-get**
会在安装你的软件包的时候自动安装推荐的软件包(用户可以禁用这个默认行为)。**dpkg**
则会忽略此项。

-   **Suggests**

此项中的软件包可以和本程序更好地协同工作，但不是必须的。当用户安装程序时，所有的前端程序可能不会询问是否安装建议的软件包。**aptitude**
可以被配置为安装软件时自动安装建议的软件包，但这不是默认。**dpkg** 和
**apt-get** 将忽略此项。

-   **Pre-Depends**

此项中的依赖强于 **Depends** 项。软件包仅在预依赖的软件包已经安装并且 *正确配置*
后才可以正常安装。在使用此项时应 *非常慎重*，仅当在 TC
讨论后才能使用。记住：根本就不要用这项。 :-)

-   **Conflicts**

仅当所有冲突的软件包都已经删除后此软件包才可以安装。当程序在某些特定软件包存在的情况下根本无法运行或存在严重问题时使用此项。

-   **Breaks**

此软件包安装后列出的软件包将会受到损坏。通常 **Breaks**
要附带一个“版本号小于多少”的说明。这样，软件包管理工具将会选择升级被损坏的特定版本的软件包作为解决方案。

-   **Provides**

某些类型的软件包会定义有多个备用的虚拟名称。如果你的程序提供了某个已存在的虚拟软件包的功能则使用此项。

-   **Replaces**

当你的程序要替换其他软件包的某些文件，或是完全地替换另一个软件包(与
**Conflicts** 一起使用)。列出的软件包中的某些文件会被你软件包中的文件所覆盖。

**4.7. 编译构建**

-   **debian/rules**
    脚本重新封装了上游的构建系统，用来编译源码文件，将文件安装至
    \*\*\$(DESTDIR)，\*\*并将生成的文件存入各个 **deb** 二进制包文件中的目的。

-   通过添加合适的 **override_dh\_**\*
    目标（target）并编写对应的规则，可以实现对 **debian/rules** 脚本的灵活定制。

-   如果需要在 **dh** 命令调用某些特定的 \*\*dh_\*\**foo*
    命令时采取某些特别的操作，则任何自动执行的操作均可以被 **debian/rules**
    中额外添加的 \*\*override_dh_\*\**foo* 这样的 Makefile 目标所覆写。

-   不要嵌入基于系统时间的时间戳。

-   在 **debian/rules** 中使用“**dh \$@**”以应用最新的 **debhelper** 特性。

-   在构建环境中导出环境变量“**LC_ALL=C.UTF-8**”。

-   对上游源代码中使用的时间戳，使用 debhelper 提供的环境变量
    **\$SOURCE_DATE_EPOCH** 的值。

### **5. 一个例子**


**5.1. 总体流程**

从上游源码压缩包 **debhello-0.0.tar.gz** 构建单个非原生 Debian
软件包的大致流程可以总结如下：

-   维护者获取上游源码压缩包 **debhello-0.0.tar.gz** 并将其内容解压缩至
    **debhello-0.0** 目录中。

-   **debmake** 命令对上游源码树进行 debian
    化（debianize），具体来说，是创建一个 **debian**
    目录并仅向该目录中添加各类模板文件。

-   名为 **debhello_0.0.orig.tar.gz** 的符号链接被创建并指向
    **debhello-0.0.tar.gz** 文件。

-   维护者须自行编辑修改模板文件。

-   **debuild** 命令基于已 debian 化的源码树构建二进制软件包。

-   **debhello-0.0-1.debian.tar.xz** 将被创建，它包含了 **debian** 目录。

**软件包构建的大致流程.**

\$ tar -xzmf debhello-0.0.tar.gz

\$ cd debhello-0.0

\$ debmake

... manual customization

\$ debuild

...

**5.2. 模板文件**

其中debmake用于生成debian化的模板文件，具体结构如下所示：

**基本 debmake 命令运行后的源码树。**

\$ tree

├── debhello-0.0

│ ├── LICENSE

│ ├── Makefile

│ ├── debian

│ │ ├── README.Debian

│ │ ├── changelog

│ │ ├── control

│ │ ├── copyright

│ │ ├── patches

│ │ │ └── series

│ │ ├── rules

│ │ ├── source

│ │ │ ├── control

│ │ │ ├── format

│ │ │ ├── local-options

│ │ │ ├── options

│ │ │ └── patch-header

│ │ ├── tests

│ │ │ └── control

│ │ ├── upstream

│ │ │ └── metadata

│ │ └── watch

│ └── src

│ └── hello.c

├── debhello-0.0.tar.gz

└── debhello_0.0.orig.tar.gz -\> debhello-0.0.tar.gz

7 directories, 19 files

这里的 **debian/rules** 文件是应当由软件包维护者提供的构建脚本。此时该文件是由
**debmake** 命令产生的模板文件。

**5.3. 编辑模板文件**

作为维护者，要制作一个合适的 Debian 软件包当然需要对模板内容进行一些手工的调整。

为了将安装文件变成系统文件的一部分，**Makefile** 文件中 **\$(prefix)** 默认的
**/usr/local** 的值需要被覆盖为 **/usr**。要做到这点，可以按照下面给出的方法，在
**debian/rules** 文件中添加名为 **override_dh_auto_install**
的目标，在其中设置“**prefix=/usr**”。

**debian/rules（维护者版本）：.**


```
$ vim debhello-0.0/debian/rules
 ... hack, hack, hack, ...
$ cat debhello-0.0/debian/rules
#!/usr/bin/make -f
export DH_VERBOSE = 1
export DEB_BUILD_MAINT_OPTIONS = hardening=+all
export DEB_CFLAGS_MAINT_APPEND  = -Wall -pedantic
export DEB_LDFLAGS_MAINT_APPEND = -Wl,--as-needed

%:
        dh $@

override_dh_auto_install:
        dh_auto_install -- prefix=/usr
```


如上在 **debian/rules** 文件中导出=**DH_VERBOSE** 环境变量可以强制 **debhelper**
工具输出细粒度的构建报告。

如上导出 **DEB_BUILD_MAINT_OPTION** 变量可以如 **dpkg-buildflags**
手册页中“FEATURE
AREAS/ENVIRONMENT”部分所说，对加固选项进行设置。

如上导出 **DEB_CFLAGS_MAINT_APPEND** 可以强制 C 编译器给出所有类型的警告内容。

如上导出 **DEB_LDFLAGS_MAINT_APPEND**
可以强制链接器只对真正需要的库进行链接。

对基于 Makefile 的构建系统来说，**dh_auto_install**
命令所做的基本上就是“**\$(MAKE) install DESTDIR=debian/debhello**”。这里创建的
**override_dh_auto_install** 目标将其行为修改为“**\$(MAKE) install
DESTDIR=debian/debhello prefix=/usr**”。

这里是维护者版本的 **debian/control** 和 **debian/copyright** 文件。

**debian/control（维护者版本）：.**


```
$ vim debhello-0.0/debian/control
 ... hack, hack, hack, ...
$ cat debhello-0.0/debian/control
Source: debhello
Section: devel
Priority: optional
Maintainer: Osamu Aoki <osamu@debian.org>
Build-Depends: debhelper-compat (= 13)
Standards-Version: 4.5.1
Homepage: https://salsa.debian.org/debian/debmake-doc
Rules-Requires-Root: no

Package: debhello
Architecture: any
Multi-Arch: foreign
Depends: ${misc:Depends}, ${shlibs:Depends}
Description: Simple packaging example for debmake
 This Debian binary package is an example package.
 (This is an example only)
```


在 **debian/**
目录下还有一些其它的模板文件。根据具体场景，它们也需要进行更新。之后可以开始对软件包进行构建。

**5.4. 使用 debuild 构建软件包**

您可以使用 **debuild** 或者等效的命令工具，在这个源码树内构建一个非原生 Debian
软件包。

**debhello 0.0 版使用 debuild 命令产生的文件：.**


```
$ cd ..
$ tree -FL 1
.
├── debhello-0.0/
├── debhello-0.0.tar.gz
├── debhello-dbgsym_0.0-1_amd64.deb
├── debhello_0.0-1.debian.tar.xz
├── debhello_0.0-1.dsc
├── debhello_0.0-1_amd64.build
├── debhello_0.0-1_amd64.buildinfo
├── debhello_0.0-1_amd64.changes
├── debhello_0.0-1_amd64.deb
└── debhello_0.0.orig.tar.gz -> debhello-0.0.tar.gz
1 directory, 9 files
```


您可以看见生成的全部文件。

-   **debhello_0.0.orig.tar.gz** 是指向上游源码压缩包的符号链接。

-   **debhello_0.0-1.debian.tar.xz** 包含了维护者生成的内容。

-   **debhello_0.0-1.dsc** 是 Debian 源码包的元数据文件。

-   **debhello_0.0-1_amd64.deb** 是 Debian 二进制软件包。

-   **debhello-dbgsym_0.0-1_amd64.deb** 是 Debian 的调试符号二进制软件包。

-   **debhello_0.0-1_amd64.build** 是构建日志文件。

-   **debhello_0.0-1_amd64.buildinfo** 是 **dpkg-genbuildinfo**
    生成的元数据文件。

-   **debhello_0.0-1_amd64.changes** 是 Debian 二进制软件包的元数据文件。
