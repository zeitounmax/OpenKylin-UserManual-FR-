# 移植GNU Hello到openKylin

> 本文是openKylin软件包打包移植的示例文档

## 一、GNU Hello 项目介绍

GNU Hello项目是 GNU 推出的示例软件，此项目将入门的 hello world 程序，以正规的 GNU 规范来实现，作为 GNU 编码标准和 GNU 维护者实践的参考范例。俗话说得好，麻雀虽小，五脏俱全，GNU Hello 虽然只是一个 hello world 程序，却包含了如下几项技术：

- Automake 和 Autoconf：生成编译配置脚本
- Gnulib：程序的基本函数库
- Gettext：国际化支持
- getopt：命令行参数支持
- help2man：用程序的--help选项输出生成manpage
- Texinfo：编写程序文档

GNU项目中其他更复杂的软件系统，只是具体的业务逻辑复杂度更高，其组织结构与GNU Hello项目展示的规范保持一致。

## 二、对上游源码进行打包

> **openKylin 使用 apt 作为包管理器，因此将上游软件引入 openKylin 时，需要把上游源代码打包成符合 Debian 规范的源码包。**

### 1. 获取上游源码

GNU Hello 项目的官方网站为：[https://www.gnu.org/software/hello/](https://www.gnu.org/software/hello/)

在官网介绍中获取最新版(2.12.1)的下载地址：[https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz](https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz)

随后下载并解压源码包：

```shell
$ wget https://ftp.gnu.org/gnu/hello/hello-2.12.1.tar.gz
$ tar -xaf hello-2.12.1.tar.gz
```

### 2. 使用 debmake 产生模板文件

进入源码目录，执行`debmake`

```shell
$ cd hello-2.12.1
$ debmake
I: set parameters
I: sanity check of parameters
I: pkg="hello", ver="2.12.1", rev="1"
I: *** start packaging in "hello-2.12.1". ***
I: provide hello_2.12.1.orig.tar.gz for non-native Debian package
I: pwd = "/data2/test/hello"
I: $ ln -sf hello-2.12.1.tar.gz hello_2.12.1.orig.tar.gz
I: pwd = "/data2/test/hello/hello-2.12.1"
I: parse binary package settings: 
I: binary package=hello Type=bin / Arch=any M-A=foreign
I: analyze the source tree
I: build_type = Autotools with autoreconf
I: scan source for copyright+license text and file extensions
I:  43 %, ext = c
I:  27 %, ext = m4
I:  11 %, ext = gmo
I:  11 %, ext = po
I:   1 %, ext = gperf
I:   1 %, ext = mk
I:   1 %, ext = in
I:   1 %, ext = texi
I:   0 %, ext = sin
I:   0 %, ext = sed
I:   0 %, ext = header
I:   0 %, ext = text
I:   0 %, ext = O
I:   0 %, ext = ac
I:   0 %, ext = am
I:   0 %, ext = 1
I:   0 %, ext = pot
I:   0 %, ext = x
I:   0 %, ext = valgrind
I:   0 %, ext = info
I:   0 %, ext = tex
I:   0 %, ext = sub
I:   0 %, ext = rpath
I:   0 %, ext = sh
I:   0 %, ext = guess
I: check_all_licenses
......
I: check_all_licenses completed for 447 files.
I: bunch_all_licenses
I: format_all_licenses
I: make debian/* template files
I: single binary package
I: debmake -x "1" ...
I: creating => debian/control
I: creating => debian/copyright
I: substituting => /usr/share/debmake/extra0/changelog
I: creating => debian/changelog
I: substituting => /usr/share/debmake/extra0/rules
I: creating => debian/rules
I: substituting => /usr/share/debmake/extra1/compat
I: creating => debian/compat
I: substituting => /usr/share/debmake/extra1/watch
I: creating => debian/watch
I: substituting => /usr/share/debmake/extra1/README.Debian
I: creating => debian/README.Debian
I: substituting => /usr/share/debmake/extra1source/local-options
I: creating => debian/source/local-options
I: substituting => /usr/share/debmake/extra1source/format
I: creating => debian/source/format
I: substituting => /usr/share/debmake/extra1patches/series
I: creating => debian/patches/series
I: run "debmake -x2" to get more template files
I: $ wrap-and-sort
```

运行`debmake`后生成了新的 debian 目录和模板文件，查看此时的源码树

```shell
$ cd ..
$ tree
├── hello-2.12.1
│......
│   ├── debian
│   │   ├── changelog
│   │   ├── compat
│   │   ├── control
│   │   ├── copyright
│   │   ├── patches
│   │   │   └── series
│   │   ├── README.Debian
│   │   ├── rules
│   │   ├── source
│   │   │   ├── format
│   │   │   └── local-options
│   │   └── watch
│......
├── hello_2.12.1.orig.tar.gz -> hello-2.12.1.tar.gz
└── hello-2.12.1.tar.gz
```

### 3. 编辑 debian 目录模板文件

#### (1) 检查 debian/source 文件夹

##### [1] 确保软件包格式为 quilt

`debian/source/format`文件中定义了源码包的格式，通常分为`3.0 (native)`和`3.0 (quilt)`，根据 openKylin 软件包构建工作流的要求，源码包格式必须为`3.0 (quilt)`。

```shell
$ cat debian/source/format 
3.0 (quilt)
```

##### [2] local-options 文件一般不需要，可以去掉

```shell
rm debian/source/local-options
```

#### (2) 编辑 debian/changelog 文件

`debian/changelog`是软件包的更新日志，记录内容包括软件包包名，版本号，版本信息描述等。
我们先查看`debian/changlog`文件的默认的初始化内容：

```text
hello (2.12.1-1) UNRELEASED; urgency=low

  * Initial release. Closes: #nnnn
    <nnnn is the bug number of your ITP>

 -- your name <your email address>  Wed, 16 Nov 2022 09:40:47 +0800
```

第一行最左边是软件包名称，其右侧括号内的是软件包版本号，软件包版本号格式为：`<upstream_version>-<revisions>`，upstream_version是上游版本号，在执行`debmake`时，已经自动识别上游版本号为2.12.1；revisions是修订版本，默认是1，对于 openKylin 软件包，使用`ok1`作为第一次revisions。

版本号右侧的UNRELEASED需要修改为yangtze，中间的描述部分通常写为：Build for openKylin。

最后一行中的`your name`为个人署名，建议使用英文，`your email address`为个人电子邮箱，这两处都需要填写，不可缺少。

修改完成后的`debian/changlog`文件内容应参考如下所示：

```text
hello (2.12.1-ok1) yangtze; urgency=medium

  * Build for openKylin.

 -- zhangsan <zhangsan@email.com>  Tue, 15 Nov 2022 10:13:02 +0800
```

#### (3) 编辑 debian/control 文件

`debian/control`定义了源码包和二进制包的信息，包括编译依赖、安装依赖等，是必不可少的一个文件，根据上游软件展出的信息等进行编写即可，修改完成后如下所示。

```text
Source: hello
Section: devel
Priority: optional
Maintainer: openKylin Developers <developers@lists.openkylin.top>
Standards-Version: 4.3.0
Build-Depends: debhelper-compat (= 9)
Homepage: http://www.gnu.org/software/hello/
Rules-Requires-Root: no

Package: hello
Architecture: any
Depends: ${shlibs:Depends}, ${misc:Depends}
Conflicts: hello-traditional
Replaces: hello-traditional, hello-debhelper (<< 2.9)
Breaks: hello-debhelper (<< 2.9)
Description: example package based on GNU hello
 The GNU hello program produces a familiar, friendly greeting.  It
 allows non-programmers to use a classic computer science tool which
 would otherwise be unavailable to them.
 .
 Seriously, though: this is an example of how to do a Debian package.
 It is the Debian version of the GNU Project's `hello world' program
 (which is itself an example for the GNU Project).
```

#### (4) 编辑 debian/rules 文件

`debian/rules`是应当由软件包维护者提供的构建脚本，这个文件事实上是另一个 Makefile，需要根据具体上游软件的编译方法，编写相应的内容。Debian格式的源码包在编译时，默认读取debian/rules文件中的内容作为编译规则，此文件同样不可或缺。

Hello是一个标准的使用`Autotools`构建的软件包，其编译安装方法为：

```shell
$ ./configure 
$ make
$ sudo make install
```

根据上述编译方法，可以编写 debian/rules 文件内容如下：

```Makefile
#!/usr/bin/make -f
%:
    dh $@

override_dh_auto_clean:
    [ ! -f Makefile ] || $(MAKE) distclean

override_dh_installdocs:
    dh_installdocs NEWS
```

#### (5) 处理非必需文件

debian/compat 和 debian/README.Debian 不是必需的文件，对于此软件包可以去掉。

```shell
$ rm debian/compat
$ rm debian/README.Debian
```

### 4. 打包并测试编译

在源码目录顶层执行`dpkg-source -b .`，即可将上游源码打包为 Debian 格式的源码包，打包成功后会生成`.debian.tar.xz`和`.dsc`结尾的两个新文件。

```shell
$ dpkg-source -b .
dpkg-source: info: using source format '3.0 (quilt)'
dpkg-source: info: building hello using existing ./hello_2.12.1.orig.tar.gz
dpkg-source: info: building hello in hello_2.12.1-ok1.debian.tar.xz
dpkg-source: info: building hello in hello_2.12.1-ok1.dsc

$ tree -L 1 ../
../
├── hello-2.12.1
├── hello_2.12.1-ok1.debian.tar.xz
├── hello_2.12.1-ok1.dsc
├── hello_2.12.1.orig.tar.gz -> hello-2.12.1.tar.gz
└── hello-2.12.1.tar.gz

1 directory, 4 files
```

在把源码包上传至Git仓库之前，我们还可以先在本地进行编译测试，确认 debian/rules 等文件是否编写正确。每次打包时，都建议先完成本地编译测试，再上传到Git仓库。Debian格式的源码包在编译时使用的是`dpkg-buildpackage`工具。编译成功时，如下所示：

```shell
$ dpkg-buildpackage -b -uc
dpkg-buildpackage: info: 源码包 hello
dpkg-buildpackage: info: 源码版本 2.12.1-ok1
dpkg-buildpackage: info: source distribution yangtze
dpkg-buildpackage: info: 源码修改者 your name <your email address>
dpkg-buildpackage: info: 主机架构 amd64
......
dpkg-deb: 正在 '../hello_2.12.1-ok1_amd64.deb' 中构建软件包 'hello'。
dpkg-deb: 正在 'debian/.debhelper/scratch-space/build-hello/hello-dbgsym_2.12.1-ok1_amd64.deb' 中构建软件包 'hello-dbgsym'。
        Renaming hello-dbgsym_2.12.1-ok1_amd64.deb to hello-dbgsym_2.12.1-ok1_amd64.ddeb
 dpkg-genbuildinfo --build=binary
 dpkg-genchanges --build=binary >../hello_2.12.1-ok1_amd64.changes
dpkg-genchanges: info: binary-only upload (no source code included)
 dpkg-source --after-build .
dpkg-buildpackage: info: binary-only upload (no source included)

$ tree -L 1 ../  
../
├── hello-2.12.1
├── hello_2.12.1-ok1_amd64.buildinfo
├── hello_2.12.1-ok1_amd64.changes
├── hello_2.12.1-ok1_amd64.deb
├── hello_2.12.1-ok1.debian.tar.xz
├── hello_2.12.1-ok1.dsc
├── hello_2.12.1.orig.tar.gz -> hello-2.12.1.tar.gz
├── hello-2.12.1.tar.gz
└── hello-dbgsym_2.12.1-ok1_amd64.ddeb

1 directory, 8 files
```

## 三、将源码包上传至Gitee平台

### 1. 创建对应的Git仓库

#### (1) fork openkylin/community 仓库到个人仓库中

在openKylin组织中创建新的代码仓库，需要用到openkylin/community仓库：[https://gitee.com/openkylin/community](https://gitee.com/openkylin/community)

在浏览器网页端forkopenkylin/community仓库到个人账号，会生成新的仓库地址：`https://gitee.com/<username>/community`，将这个新仓库clone到本地。

```shell
$ git clone git@gitee.com:<username>/community.git
```

后续流程从维护者角色来说，分为 **单独的包维护者** 和 **SIG组维护者** 两种。

#### (2) 作为单独的包维护者

##### [1] 创建软件包维护信息

进入到一级目录packages里面，创建`hello.yaml`文件，添加以下内容：
(更多填写示例可以浏览：[https://gitee.com/openkylin/community/tree/master/packages](https://gitee.com/openkylin/community/tree/master/packages))

```yaml
name: hello
path: hello
maintainers:
- name: 你的Gitee ID
  openkylinid: 你的openkylinid用户名
  displayname: 你的姓名或昵称
  email: 你的联系邮箱
```

##### [2] 将改动推送到个人远端仓库中，并向openkylin/community仓库提交pr

```shell
$ git add packages
$ git commit -m "添加hello软件包维护信息"
$ git push origin master
```

推送到个人仓库之后，在web页面端发起`Pull Requests`请求，对应的社区管理人员审核通过并接受pr之后，openkylin平台内部会根据软件包维护信息中描述的内容自动创建相应openkylin仓库。

例如，上面的`hello`软件包的pr请求合并之后，会创建出名为`hello`的openkylin仓库，可以在浏览器中访问该仓库，这里`hello`软件包对应的仓库地址为：[https://gitee.com/openkylin/hello](https://gitee.com/openkylin/hello)

#### (3) 作为SIG组维护者

##### [1] 创建软件包维护信息

这一步需要在对应的sig组的sig.yaml文件中添加源码包名称，这里以Packaging SIG组举例，操作示例如下：

```shell
$ echo "- hello" >> sig/packaging/sig.yaml
```

##### [2] 将改动推送到个人远端仓库中，并向openkylin/community仓库提交pr

```shell
$ git add sig/packaging/sig.yaml
$ git commit -m "新增hello软件包"
$ git push origin master
```

推送到个人仓库之后，在web页面端发起`Pull Requests`请求，对应的SIG组维护者审核通过并接受pr之后，openkylin平台内部会根据软件包维护信息中描述的内容自动创建相应openkylin仓库。

例如，上面的`hello`软件包的pr请求合并之后，会创建出名为`hello`的openkylin仓库，可以在浏览器中访问该仓库，这里`hello`软件包对应的仓库地址为：[https://gitee.com/openkylin/hello](https://gitee.com/openkylin/hello)

### 2. 构建本地Git仓库

获取构建工具：[https://gitee.com/openkylin/packaging-tools](https://gitee.com/openkylin/packaging-tools)

```shell
$ path/to/source-packing.py import-to-git --dsc-file hello_2.12.1-ok1.dsc --packaging-branch openkylin/yangtze
```

### 3. 推送至远程Git仓库

```shell
$ cd hello
$ git remote add origin git@gitee.com:openkylin/hello.git
$ git push --all && git push --tags
```

此时可以在浏览器中打开Gitee平台的仓库链接([https://gitee.com/openkylin/hello](https://gitee.com/openkylin/hello))，查看源代码是否推送成功。
