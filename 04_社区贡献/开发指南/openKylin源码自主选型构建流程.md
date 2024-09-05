# openKylin源码自主选型构建流程

# 一、软件包版本号选型

## 1. 选型策略

### 1.1 软件项目选型策略

![输入图片说明](assets/openKylin%E6%BA%90%E7%A0%81%E8%87%AA%E4%B8%BB%E9%80%89%E5%9E%8B%E6%9E%84%E5%BB%BA%E6%B5%81%E7%A8%8B/%E8%BD%AF%E4%BB%B6%E9%A1%B9%E7%9B%AE%E9%80%89%E5%9E%8B%E7%AD%96%E7%95%A5.png)
- 持续积极引入全“A”软件(版本)

- 鼓励引入仅含”A"和”B”软件(版本)tory-view-113483.html

- 谨慎引入含"C“软件(版本)

- 拒绝引入/及时删除含“D”或者“C >= 3”的软件(版本)

### 1.2 软件版本选型策略

- 长期支持版本(暂时openKylin还没定长期支持版本，1.0目前来说不是长期支持版)
  - 兼顾质量与稳定，优先选择主流OS广泛应用的版本。
  - 升级后不影响兼容性列表。
  - 同一个版本生命周期内不做大版本变动，仅采取特性、补丁回合的方式解决质量问题。

- 社区开发版本
  - 满足基本合规问题下，尽可能集成更多软件，优先选择开源组件当前稳定分支的最新版本。
  - 及时跟进上游社区动态，合并开源软件新特性版本。

### 1.3 软件分级与兼容性原则

| **级别** | **范围**                                | **原则**                                                     |
| -------- | --------------------------------------- | ------------------------------------------------------------ |
| 一级     | kernel、glibc、gcc、zlib等系统核心组件  | API和ABI主版本的生命周期内保持稳定，并且在接下来的一个主版本中也尽量保持稳定 |
| 二级     | dbus、openssl、bzip2、systemd等基础软件 | API和ABI在单个主版本的生存周期内保持稳定，依赖的应用程序在单个主版本生命周期内原则上无需重大修改 |
| 三级     | 其他应用软件等                          | API和ABI在单个主版本的生存周期内不强制保持稳定，存在依赖关系的应用程序保持联动升级 |

## 2. 具体流程

1. 依据“软件项目选型策略"挑选需要引入的新项目

2. 依据"软件版本选型策略"挑选该项目需要引入的某一个版本

3. 输出“项目选型报告”，包含新项目的合规性、兼容性、重要程度、活跃度、质量、安全性具体情况，以及选中版本的新特性、兼容性、影响域、其他发行版中版本情况对比等内容；

4. 技术委员会通过后按以下流程开展具体工作

### 2.1 获取软件包的项目地址

获取项目地址用于查看软件包当前的项目详情、开发状态、编译安装、使用说明等相关内容

可以从以下几个方法去获取软件包的项目地址：

#### 2.1.1 debian社区的软件包追踪平台

基本上绝大部分的软件包都可以在debian社区的软件包追踪平台（[tracker.debian.org](https://tracker.debian.org/)）上找到

例如：查找`libnftnl`软件包

![输入图片说明](assets/openKylin%E6%BA%90%E7%A0%81%E8%87%AA%E4%B8%BB%E9%80%89%E5%9E%8B%E6%9E%84%E5%BB%BA%E6%B5%81%E7%A8%8B/debian%E7%A4%BE%E5%8C%BA%E7%9A%84%E8%BD%AF%E4%BB%B6%E5%8C%85%E8%BF%BD%E8%B8%AA%E5%B9%B3%E5%8F%B0%E7%A4%BA%E4%BE%8B.png)

如上图，可以通过右上角的homepage信息去获取软件包的项目地址；

#### 2.1.2 dsc文件

可以在debian社区（[tracker.debian.org](https://tracker.debian.org/)）、ubuntu（https://launchpad.net）等平台查看软件包的dsc文件，dsc文件中可能有描述当前软件包的项目地址信息。

如：`libnftnl`在debian社区查看软件包[dsc文件](https://deb.debian.org/debian/pool/main/libn/libnftnl)（以下以[1.1.2-2版本](https://deb.debian.org/debian/pool/main/libn/libnftnl/libnftnl_1.1.2-2.dsc)为例）：

```Bash
# 去除gpg签名信息，方便查看
Format: 3.0 (quilt)
Source: libnftnl
Binary: libnftnl11, libnftnl-dev
Architecture: linux-any
Version: 1.1.2-2
Maintainer: Debian Netfilter Packaging Team <pkg-netfilter-team@lists.alioth.debian.org>
Uploaders: Arturo Borrero Gonzalez <arturo@debian.org>
Homepage: https://git.netfilter.org/libnftnl
Standards-Version: 4.2.1
Vcs-Browser: https://salsa.debian.org/pkg-netfilter-team/pkg-libnftnl
Vcs-Git: https://salsa.debian.org/pkg-netfilter-team/pkg-libnftnl
Build-Depends: debhelper (>= 11), libmnl-dev, libtool, pkg-config
Package-List:
 libnftnl-dev deb libdevel optional arch=linux-any
 libnftnl11 deb libs optional arch=linux-any
Checksums-Sha1:
 c0f880588fabaa845a88fb5a2bdad0dea7481857 366014 libnftnl_1.1.2.orig.tar.bz2
 0a9063e79191955fbf59482d75c7bfb62aded816 912 libnftnl_1.1.2.orig.tar.bz2.asc
 505f8b44e3adb574dc3cf657580e9d73f0b0a853 8772 libnftnl_1.1.2-2.debian.tar.xz
Checksums-Sha256:
 a5c7b7a6c13c9c5898b13fcb1126fefce2015d5a96d7c354b19aaa40b6aece5d 366014 libnftnl_1.1.2.orig.tar.bz2
 4178552d7ad210e9f17df3c079ac6de032f62c72774d59bfac404a023d48950b 912 libnftnl_1.1.2.orig.tar.bz2.asc
 4ef80a0c344b23624d24b7dd4be1b73a7b96d86089813c545705f8093067ce13 8772 libnftnl_1.1.2-2.debian.tar.xz
Files:
 14093a238d5025d4a452e6d1cef88c58 366014 libnftnl_1.1.2.orig.tar.bz2
 e10dcb9138261d92e7569bd72b809a28 912 libnftnl_1.1.2.orig.tar.bz2.asc
 518ff0360f67c8031a31792a257a5c3b 8772 libnftnl_1.1.2-2.debian.tar.xz
```

如高亮处：

- **Homepage** ：项目地址

- **Vcs-Browser** ：版本控制系统浏览平台（类似于github、gitee、gitlab等平台）

- **Vcs-Git** ：git仓库地址

#### 2.1.3 debian/control文件

首先先从debian社区、ubuntu社区（launchpad.net）任意下载源码包，并解压缩。

```Bash
# 从ubuntu社区（launchpad.net）平台下载源码包
# 也可以通过添加deb-src源的方式使用apt-source来下载源码
# 使用dget需要安装devscripts
dget -x https://launchpad.net/ubuntu/+archive/primary/+sourcefiles/libnftnl/1.2.3-1/libnftnl_1.2.3-1.dsc

# 进入到解压缩目录
cd libnftnl-1.2.3
```

查看`debian/control`文件，查找是否有`Homepage`、`Vcs-Git`、`Vcs-Browser`等相关字段

如：

```Bash
$ grep -E "^(Homepage|Vcs-*)" debian/control
Homepage: https://git.netfilter.org/libnftnl
Vcs-Git: https://salsa.debian.org/pkg-netfilter-team/pkg-libnftnl.git
Vcs-Browser: https://salsa.debian.org/pkg-netfilter-team/pkg-libnftnl
```

上述高亮处各字段的说明与2.1.2小节的内容一致

#### 2.1.4 debian/watch文件

与2.1.3小节一样，需要先下载源码。这次查看的是`debian/watch`文件

**debian/watch** 文件主要是用来记录上游源码的下载地址（该文件不是必须的，因此在某些源码包中可能没有该文件）

```Bash
$ cat debian/watch  
version=3
opts=pgpsigurlmangle=s/$/.sig/ http://ftp.netfilter.org/pub/libnftnl/libnftnl-(\S+).tar.bz2
```

通过该文件记录的上游源码下载地址，可以找到其对应的项目地址

如：

```Bash
# 其下载地址一般是ftp或git release下载地址
# 如果其下载地址是由别的平台托管的那么可能就无法获悉其项目地址
http://ftp.netfilter.org/pub/libnftnl

# 通过上述地址猜测可能的项目地址是
http://netfilter.org/pub/libnftnl
```

### 2.2 获取软件包的版本情况

通过查看项目地址获悉当前项目的最新稳定版本列表。

可以通过以下几种方式查看：

- debian/watch 文件

在2.1.4小节中如果软件包的有debian/watch文件，可以查看该项目的上游软件包压缩包下载地址。

```
！其下载地址中可能有beta版本，其名字类似于xxxx-beta.tar.gz。一般这种beta版本的源码不应该被选择。
```

- git仓库

如果是git仓库，可以查看其对应的是否有发布 git release：

1. 如果有，那么可以在git release页面获取其版本列表

2. 如果没有，查看其对应的`branch(分支)`、`tag(标记)`是否有相关版本列表信息



# 二、构建源码包

从第一章节中选出待构建软件包的版本以及下载地址。

如果远端仓库已有项目，需要变更upstream版本，那么请查看第4小节的内容。

## 1. 下载源码并修改源码目录格式

下载源码包到本地，并解压缩

```Bash
# 下载源码
wget https://www.netfilter.org/pub/libnftnl/libnftnl-1.2.3.tar.bz2

# 解压缩
tar -xaf libnftnl-1.2.3.tar.bz2
```

修改源码目录，使其符合**<source_name>-<source_version>**格式。如：

```Bash
$ ls -al
drwxr-x---@    - luzp 10 8月  02:24 libnftnl-1.2.3
.rw-rw----@ 395k luzp 10 8月  02:24 libnftnl-1.2.3.tar.bz2
```

## 2. 构建出debian打包目录

### 2.1 upstream源码包含debian目录

```
！注意：如果上游源码目录中包含debian目录，此时我们需要手动对上游的debian目录进行改名，避免我们再重新生成debian目录时引起别的问题
```

- 备份原有的debian目录

```Bash
mv debian debian-orig
```

- 重新构建debian目录

```Bash
debmake -u <upstream版本号> -r <修订版本号> -t
```

此时，会在源码的同级目录新创建个目录，其格式为<原来的源码目录名称>-<upstream版本号>

假如：

源码压缩包是`amdgcn-tools-13.tar.gz`

```Bash
# 解压源码
# 解压后的源码目录为amdgcn-tools-13
tar -xaf amdgcn-tools-13.tar.gz

# 进入源码目录，并备份原来的debian目录
cd amdgcn-tools-13
mv debian debian-orig

# 重新构建debian目录
debmake -u13 -rok1 -t

# 执行完成之后，会在amdgcn-tools-13同级目录生成一个新的目录
# <原来的源码目录>-<upstream版本号>,也就是下面的目录
ls amdgcn-tools-13-13
```

### 2.2 直接构建debian目录

如果上游源码是纯源码，那么直接构建debian目录即可。

```Bash
# 构建debian目录
debmake -z tar.bz2
```

- -z 指定源码的压缩包格式
  - libnftnl-1.2.3.tar.bz2 就指定为 -z tar.bz2

  - 默认的源码压缩格式是tar.gz

- -b 如果是perl或python的源码包可以指定该参数，为debmake创建出更多配套对应编程语言的文件

```Bash
# 如果是python
debmake -b":python3"

# perl
debmake -b":perl"
```

- -t 重新生成上游源码包（即xxx.orig.tar.gz）

更详细的debmake用法可以查看其man手册

```Bash
man debmake
```

## 3. 修改构建规则

主要是修改debian目录中的文件。

### 3.1 control

`control`文件涉及两个部分的内容：

#### 3.1.1 源码包部分

描述当前这个源码包的简要信息，以及构建该源码包的编译依赖等

其中有几个字段需要注意：

- **Section** 

描述软件包的类型，用于软件包的分类。比如`mysql-8`应属于`database`

可选的内容有：

admin, cli-mono, comm, database, debug, devel, doc, editors, education, electronics, embedded, fonts, games, gnome, gnu-r, gnustep, graphics, hamradio, haskell, httpd, interpreters, introspection, java, javascript, kde, kernel, libdevel, libs, lisp, localization, mail, math, metapackages, misc, net, news, ocaml, oldlibs, otherosfs, perl, php, python, ruby, rust, science, shells, sound, tasks, tex, text, utils, vcs, video, web, x11, xfce, zope

- **Priority**
  -  软件包的优先级

  - required 系统必须的包
  - important 重要的软件包，系统非必须的包，但是希望系统中最好能预装的
  - standard 标准相对独立的软件包
  - optional 这是默认优先级选项，除非这个软件包是系统必须的包，否则应该使用该选项

- **Maintainer** 

需要写成当前openkylin负责该软件包的sig组，如`packaging sig`组的写成`Openkylin Developers <packaging@lists.openkylin.top>`

- **Vcs-Browser**:

写成对应的gitee仓库地址， 如 https://gitee.com/openkylin/autoconf

- **Vcs-Git**

写成对应的gitee仓库git地址 ，如 https://gitee.com/openkylin/autoconf.git

- **Build-Depends和**

编译体系结构相关代码所需要的编译依赖

- **Build-Depends-Indep**

编译体系结构无关的代码所需要的编译依赖

对于**Build-Depends**和**Build-Depends-Indep**可以从项目地址中查找是否有相关编译依赖说明。

---
**注意：** 如果control文件参考了其他OS发行版的规则，需要去除或替换其他OS版本中的版本号规则，例如：

```
Build-Depends: 
  pkg1 (>= 1.0.0-0ubuntu1), 
  pkg1 (< 1.2.0-1debian2),
  pkg2 (> 1.0.0.build),
  pkg3 (> 2.0ubuntu19)
```

首先，需要明确对于quilt格式的版本号其一般带有"-"，格式为`<upstream_version>-<revision>`，而native格式的源码包一般不包含"-"

修改流程可以分两步进行

**第一步：**

对于quilt格式的版本，pkg1中的版本号 1.0.0-0ubuntu1 和 1.2.0-1debian2 其包含特定OS发行版的版本号，因此要修改。也就变成：

```
pkg1 (>= 1.0.0), 
pkg1 (< 1.2.0),
```

对于native格式的版本号，首先需要确定其upstream版本，对于pkg2和pkg3：

（1）尝试找到pkg2的1.0.0版本，如果能找到，那么1.0.0就是upstream版本，其编译依赖改为：pkg2 (> 1.0.0)；同理pkg3

（2）如果找不到pkg2的upstream版本，此外当前版本号没有包含其他OS发行版名称，那么可以保留（因为使用source-package工具重打包源码，其upstream版本号也就是当前的版本号）；但是对于pkg3，如果pkg3是OS自研的（其源码名称一般也包含OS名称，例如ubuntu-keyring），那么此时可以将此包重新改名，由openkylin重新维护，对应源码名称改为openkylin-keyring，版本号也重新制定，例如可以制定为1:1.0.0；如果不是OS自研包，最好我们也重新制定版本号，按我们自己的版本号去维护，可以内部讨论，例如这里也指定为1:1.0.0。**注意这里，最好要使用:格式的版本号，其表明带有:的版本号总会比不带的版本号高。**

那么，对于pkg2，pkg3，其修改为：

```
pkg2 (> 1.0.0.build),
pkg3 (> 1:1.0.0)
```

**第二步（native格式的不用处理）：**

第二步是在第一步的基础上改进。总体上我们要明确其他OS发行版中-横线后面的修订版本的内容。

对于<upstream_version>-<revision>，一般来说每个revision是修复了某个bug、安全漏洞等。对于ubuntu，每个revision都是由几个patch组成。

因此对于pkg1 (>= 1.0.0-0ubuntu1)，其明确规定是0ubuntu1修订版本之后的才能使用，也就意味着使用pkg1的1.0.0版本是存在风险的，最直接的现象就是编译不通过。

因此，这就需要结合我们实际的pkg1的版本情况来：

（1）如果仓库存在pkg1，其版本为1.0.0-ok1。为了保证上述编译能通过，需要集成ubuntu的1.0.0-0ubuntu1中的patch。如果已经集成相关patch，那么编译依赖就变成：

```
pkg1 (>= 1.0.0-ok1), 
pkg1 (< 1.2.0),
```

集成了patch之后仓库中的pkg1版本变为1.0.0-ok2。那么在编译依赖的地方就变成：

```
pkg1 (>= 1.0.0-ok2), 
pkg1 (< 1.2.0),
```

（2）如果仓库不存在pkg1，那么我们首次自主选型构建时，选型的版本最好是在1.0.0~1.2.0 之间选择，保证能满足各个依赖包的编译依赖。

上面所说，都是基于编译依赖来处理。同理二进制包的安装依赖字段如果也指定了版本号，那么处理方式也是和上面的一样。

#### 3.1.2 二进制包部分

描述当前源码包可以被分为哪些二进制包程序。

常见的分包类型为：

- lib

共享库，其二进制包包名一般为 `lib-xxx`

- dev

共享库头文件及相关开发文件，其二进制包名一般为`lib-xxx-dev`

- doc

软件包的文档

如果其文档对应的是某个可执行程序的文档，其二进制包包名一般为`xxx-doc`

如果其文档是某个开发库的文档，其二进制包包名一般为`lib-xxx-doc`

- bin

软件包的二进制程序，如编译型语言（比如c/c++）的二进制程序，以及解释型编程语言（比如python、perl）的可执行程序，其二进制包包名一般为其对应的二进制程序名称

这个分包时，可以结合`debmake`在构建`debian`打包目录时一起使用，如：

```Bash
# 软件包名称为 foo ， 其提供二进制程序与二进制程序的文档
debmake -b"foo:bin,foo-doc:doc"

# 软件包名称为 libfoo ， 是一个开发库源码，其提供开发库与开发库的文档
debmake -b"libfoo:lib,libfoo-doc:doc"

# 执行完之后，会自动将各个二进制包信息模板写入到debian/control文件中
```

### 3.2 rules

软件包的编译构建规则

### 3.3 changelog

修改软件包的版本号，格式为：<upstream_version>-<revisions>

一般来说，对于openkylin社区，使用`ok1`作为第一次revisions，如：

```Bash
autoconf (2.71-ok1) yangtze; urgency=low

  * Build for openKylin.

 -- Lu zhiping <luzhiping@kylinos.cn>  Fri, 26 Aug 2022 15:25:09 +0800
```

### 3.4 source/format

必须将源码包的格式设为`quilt`格式

```Bash
echo "3.0 (quilt)" > debian/source/format
```

```Bash
echo "3.0 (quilt)" > debian/source/format
```

### 3.5 其他文件

xxx.install

xxx.doc

xxx.symbols

xxx.manpages

### 3.6 应用其他OS发行版的补丁

如果其他OS发行版有该软件包的补丁，那么我们需要分析考虑是否应用他们的补丁。

可以先从以下方面去考虑是否应用其他OS发行版的补丁：

- 补丁的修复内容是什么？补丁没能被上游社区吸收的原因，是否是无关紧要的补丁？

- 是否是以前的旧版本才有的补丁，新版本是否已经集成？

- 是否是重大漏洞修复补丁？

- 是否是特定OS发行版才需要的补丁？

- 如果不应用补丁是否出现编译问题，安装是否有问题？

```
而且，应用上游社区的patches最好是在构建出git仓库之后再应用patches，这样能在git commit中保留上游社区patch的信息
```

下面的内容是在第五章节构建出git仓库之后应用patch的步骤：

如果需要应用上游社区的patch，最好是保留他们patch信息的内容与作者信息等。可以通过git工具直接应用上游社区的patch：

- git am应用patch

git am应用patch会自动保留patch的信息

```Bash
git am xxxx.patch
```

- git apply应用patch

但是有些patch的信息格式不是完全适用于git am，那么可以使用git apply导入patch，然后再进行commit添加patch信息

```Bash
# 导入patch信息
git apply xxx.patch

# 添加改动
git add .

# 在编辑框中填写patch的相关信息，如
# patch的作者，patch的内容等
git commit
```

导入patch之后，想对新导入patch的内容进行编译测试，那么可以使用gbp工具来生成新的源码（新的dsc文件）

```Bash
# 修改changelog新增版本号
vim debian/changelog

# 编译源码，会生成新的dsc文件
gbp buildpackage -S -sa -nc --git-prisine-tar --git-ignore-branch
```

## 4. 导入上游upstream源码

如果远端仓库已有项目，需要变更upstream版本。

（1）从上游社区下载源码压缩包

（2）对源码压缩包重新加压缩使其符合`<package-name>_<pacakge-version>.orig.tar.[gz|xz|bz2]`格式

```Bash
#例如：
# 从上游源码社区获取的源码压缩包为 requests-2.27.1.tar.gz
# 首先解压源码
tar -xaf requests-2.27.1.tar.gz

# 修改源码目录为 <package-name>-<package-version>
# 假如从上游社区的源码压缩包解压出来的目录是requests
mv requests requests-2.27.1

# 重新压缩,使其符合 <package-name>_<pacakge-version>.orig.tar.[gz|xz|bz2] 
tar -czf requests-2.27.1 requests-2.27.1.orig.tar.gz
```

（3）克隆gitee远端仓库源码到本地，进入源码目录

```Bash
# 克隆远端仓库
git clone git@gitee.com:openkylin/requests.git

# 进入源码目录
cd requests
```

（4）变更upstream源码

参考[openKylin源码包git工作流](openKylin源码包git工作流.md) 的第六章节《更新upstream版本》

- a. 导入上游upstream版本（第二步生成的压缩包）

- - ```Bash
    gbp import-orig requests-2.27.1.orig.tar.gz --no-merge --pristine-tar --upstream-branch=upstream
    git checkout upstream
    git push
    git push --tags
    ```

- b. 筛选旧版本patch （结合[openKylin源码包git工作流](openKylin源码包git工作流.md) 6.3.3小节进行）

- - 如果patch是研发的patch，那么需要与相关研发确认该patch在新的upstream源码中是否还需要
  - 如果patch是其他OS发行版的patch，那么需要结合OS发行版的最新版本源码中确认是否还需要该patch

-  c. 引入其他OS发行版的patch

- - 可以参考3.6小节《应用其他OS发行版的补丁》。

  - 使用git am或git apply 或其他方式导入其他OS发行版的patch

- d. 修改debian目录的其他编译构建文件

- - debian/control
  - debian/changelog 需保留原有的changelog信息
  - debian/rules
  - deiban/其他文件

修改完成之后，可以按第5小节，先本地编译测试，测试无问题后再推送到远端仓库上。

## 5. 编译源码包

完成对debian目录构建规则的完善之后，先基于此编译一轮源码包。会生成dsc文件

```Bash
# 切到开发分支上
git checkout openkylin/yangtze
# 在源码目录执行
dpkg-source -b .
```



# 三、编译

根据上一章节的内容构建出源码包之后，进行编译。

可以下载一个openkylin的chroot环境，在chroot环境中进行编译调试。

x86的chroot：http://files.build.openkylin.top/51159/chroot-openkylin-yangtze-amd64.tar.gz

riscv的chroot: http://files.build.openkylin.top/51197/chroot-openkylin-yangtze-riscv64.tar.gz

在这一阶段会对debian打包目录中的打包构建规则进行不断的修改和优化，直至打包出一个完整可安装的二进制包。



# 四、安装测试

简单测试编译出的二进制程序是否可安装，安装之后系统是否正常等



# 五、构建git仓库

## 1. 创建gitee仓库

fork openkylin的community仓库

```Bash
# fork到个人仓库之后，克隆个人仓库到本地
git clone git@gitee.com:<username>/community.git

# 然后在对应的sig组sig.yaml文件中添加源码包名称
# 例如为packaging sig新增软件包，source-name 需写实际的软件包名称
echo "- <source-name>" >> sig/packaging/sig.yaml

# 然后提交改动，推送到个人仓库
git add sig/packaging/sig.yaml
git commit -m "新增xxx软件包"
git push origin master
```

在gitee网页端创建PR，然后由community项目负责人审查，审查通过后CI平台会自动创建对应的git仓库

## 2. 构建git仓库

对第二章节构建出来的源码包进行git仓库的构建。

- 下载工具

按照[source-packaging](https://gitee.com/openkylin/packaging-tools#源码反向构建工具-source-packing)工具的使用方法下载工具和相关依赖的安装

- 构建git仓库

```Bash
./source-packing import-to-git --dsc-file sqlite3_3.31.1-ok1.dsc --packaging-branch openkylin/yangtze
```

更详细的git工作流可以参考[openKylin源码包git工作流](openKylin源码包git工作流.md) 

## 3. 关联远端仓库，并推送到gitee

需要包在第三章节编译、第四章节安装测试没问题了之后，才推送到gitee仓库上

```Bash
# 进入到第二步构建出的源码仓库
cd sqlite3
# 关联远端仓库并推送到远端仓库
git remote add origin git@gitee.com:openkylin/sqlte3.git
git push --all && git push --tags
```



# 六、其他内容

1. gitee的ci流程简要描述：

```Bash
1. 软件包第一次推送：
maintainer首次将包通过push的方式推送到gitee的仓库上，此时ci平台会将其打包发送到okbs编译平台进行编译，当前的编译地址是（https://build.openkylin.top/~cibot/+archive/openkylin/citest/+packages）

2. 后续的修改贡献：
开发者需要fork仓库到个人仓库中，然后在个人仓库上进行开发修改，源码修改完毕后再修改changelog新增版本号，然后提交PR到原始仓库。

maintainer审查PR，PR审查通过后，CI平台自动对当前的PR进行打包发送到OKBS编译平台进行编译，编译结果将会以评论的方式添加到对应的PR中，maintainer根据编译结果以及CI平台的门禁检查结果来考虑是否接收该PR
```

2. 各发行版源地址，以gvfs为例：

Debian 11: https://tracker.debian.org/pkg/gvfs

Ubuntu 22.04: https://launchpad.net/ubuntu/+source/gvfs

Fedora 36: https://src.fedoraproject.org/rpms/gvfs

Arch: https://archlinux.org/packages/extra/x86_64/gvfs/

Deepin: https://ftp.sjtu.edu.cn/deepin/pool/

openEuler: https://gitee.com/src-openeuler/gvfs
