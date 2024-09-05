# openKylin源码包git工作流



# 1. git分支设置

| 分支            | 说明                                                         | 举例                                                         |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 上游分支        | 不包含debian目录的分支，保存软件包的上游源码。 修改包只需要一个上游分支，统一命名为upstream。 对于自研包，可能会多版本并行开发，所以可能会有多个上游分支，为了避免与打包分支混淆，上游分支名称里不能使用“/” | upstream master 1.0 2.0                                      |
| 开发分支  | 用于包维护者提交修改的分支，包含debian目录。 相对于上游分支修改的内容可以直接提交到这个分支。开发分支中源码包格式为native | 名称格式为：发行版代号/系列代号 openKylin 1.0版本开发分支命名为：openkylin/yangtze |
| 打包分支   | 修改包要求使用quilt格式进行打包，这个分支下，包维护者相对上游分支做的修改都以patch形式保存在debian/patches目录中，便于更新上游版本时进行取舍。仅能用于当前发行版的包可以使用native格式打包，此时不需创建打包分支。 CI平台自动完成开发分支到打包分支的同步。 | openkylin/yangtze对应的打包分支，在分支名前面加上packaging/： packaging/openkylin/yangtze |
| patch-queue分支 | CI构建环境中使用的临时分支，不会提交到源码管理平台，辅助将开发分支的修改内容生成patch保存到打包分支。 | patch-queue/packaging/openkylin/yangtze                      |



# 2. 源码包导入

源码包导入会自动创建上游分支和打包分支，对于quilt格式的源码包，需要将打包分支转化成native格式，便于开发者利用git管理文件修改历史。

![img](assets/openKylin%E6%BA%90%E7%A0%81%E5%8C%85git%E5%B7%A5%E4%BD%9C%E6%B5%81/%E6%BA%90%E7%A0%81%E5%8C%85%E5%AF%BC%E5%85%A5%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

git_init.sh脚本描述了具体操作步骤：

```
#!/bin/bash

set -e

[ ! -d $1 ] && echo "should specify source path"  exit 1;

cd $1

# 切换到打包分支
git checkout openkylin/yangtze 
git checkout -b packaging/openkylin/yangtze

# 创建临时的集成patch分支，会自动命名为 patch-queue/openkylin/yangtze
gbp pq import 

# fix patches date format
gbp pq export

git add debian
git commit -m "format patches" || true

git checkout openkylin/yangtze 

# 在打包分支集成patch
git merge patch-queue/packaging/openkylin/yangtze -m "apply patches"

# 删除临时分支
git branch -D patch-queue/packaging/openkylin/yangtze

# 删除debian/patches目录
rm -rf debian/patches

# 修改format格式
echo "3.0 (native)" > debian/source/format

#提交改动
git add debian
git commit -m "changed debian/source/format to native"

```

## 2.1 使用工具导入

可以使用`source-packing`工具进行导入：

主要分三步：

1. 对源码重新打包：

```Bash
./source-packing rebuild-source --dsc-file sqlite3_3.31.1-4kylin0.3.dsc --new-revisions ok1
```

此时会生成 `sqlite3_3.31.1-ok1.dsc`新的dsc文件

1. 反向构建出git仓库：

```Bash
./source-packing import-to-git --dsc-file sqlite3_3.31.1-ok1.dsc --packaging-branch openkylin/yangtze
```

1. 关联远端仓库并推送到远端仓库（如果远端仓库没有创建，那么需要先创建远端仓库）

```Bash
# 进入到第二步构建出的源码仓库
cd sqlite3
# 关联远端仓库并推送到远端仓库
git remote add origin git@gitee.com:openkylin/sqlte3.git
git push --tags
git push --all
```

详细使用方式如下项目文档描述：[源码反向构建工具-source-packing](https://gitee.com/openkylin/packaging-tools#源码反向构建工具-source-packing)

备注：可以使用手动的方式进行源码的导入，具体可以参考2.2，2.3小节（如使用2.1工具反向构建成功，2.2、2.3小节内容可跳过）

## 2.2 导入源码

```Bash
gbp import-dsc --pristine-tar --debian-branch=openkylin/yangtze --upstream-branch=upstream live-build_3.0~a57-1kylin38.20.04.2.dsc live-build
```

参数含义

| --pristine-tar     | 记录原始orig tar包的数据，避免重新用gbp打包后tar包内容发生变化 |
| ------------------ | ------------------------------------------------------------ |
| --debian-branch=   | 指定打包分支名称，打包分支命名参考 https://docs.qq.com/doc/DQ2tlZ1BYUHBIVGp4 |
| --upstream-branch= | 指定上游分支名称，修改包使用upstream，自研包使用master       |

除了我们指定的两个分支，gbp import-dsc命令还会自动创建两个tag：

- debian/3.0_a57-1kylin38.20.04.2 打包分支在3.0~a57-1kylin38.20.04.2版本的状态
- upstream/3.0_a57 本次导入的上游版本3.0~a57的状态，我们打包quilt格式的源码包时需要参考这个tag

### 2.2.1 当存在多个orig包的特殊情况处理

3.0 (quilt)格式的源码包支持多个orig包，有的软件包会将上游源码的主目录和个别子目录分别打包

https://manpages.debian.org/testing/git-buildpackage/gbp-buildpackage.1.en.html#git~6

例如sqlite3，3.31.1-4版本的源码包中有两个orig：sqlite3_3.31.1.orig.tar.xz，sqlite3_3.31.1.orig-www.tar.xz，编译时会先解压sqlite3_3.31.1.orig.tar.xz，然后将sqlite3_3.31.1.orig-www.tar.xz解压到www子目录。

这种情况的包，gbp导入时能够识别，但是执行gbp buildpackage时需要指定--git-component=www才能成功打包。

可以创建debian/gbp.conf，将--git-component选项固定下来

```Prolog
[buildpackage]
component=www
# 如有多个，要写成 component=['ft2docs', 'ft2demos']
```

不同的包component各不相同，需要分别创建debian/gbp.conf文件

还有个坑，假如orig和orig-component包的压缩方式不一样，一个是xz，一个是gz，gbp buildpackage时会报错：

```Plain%20Text
gbp:warning: Components specified, pristine-tar-commit not yet supported - disabling it.
gbp:info: Creating /home/xiewei/git/perl_5.30.0.orig.tar.xz
gbp:info: Creating /home/xiewei/git/perl_5.30.0.orig-regen-configure.tar.xz
gbp:error: Error creating perl_5.30.0.orig-regen-configure.tar.xz: Pristine-tar couldn't checkout "perl_5.30.0.orig-regen-configure.tar.xz": fatal: Path 'perl_5.30.0.orig-regen-configure.tar.xz.delta' does not exist in 'refs/heads/pristine-tar'
pristine-tar: git show refs/heads/pristine-tar:perl_5.30.0.orig-regen-configure.tar.xz.delta failed
```

修复方法：

1. 统一压缩格式，把perl_5.30.0.orig-regen-configure.tar.gz重新打包成perl_5.30.0.orig-regen-configure.tar.xz

```Apache
zcat perl_5.30.0.orig-regen-configure.tar.gz  | xz -c - > perl_5.30.0.orig-regen-configure.tar.xz
```

1. 导入pristine-tar信息

```Shell
gbp pristine-tar commit --component=regen-configure path-to/perl_5.30.0.orig-regen-configure.tar.xz
```

## 2.3 创建开发分支，集成patch（将源码转换为native格式，便于开发人员本地测试编译）

```Bash
git checkout openkylin/yangtze # 切换到打包分支
gbp pq import # 创建临时的集成patch分支，会自动命名为 patch-queue/openkylin/yangtze
gbp pq export # 自动格式化patch，回到openkylin/yangtze分支
git add debian
git commit -m 'format patch'
git branch packaging/openkylin/yangtze # 创建quilt的分支，用于正式打包
git merge patch-queue/openkylin/yangtze -m "apply patches" # 在打包分支集成patch
git branch -D patch-queue/openkylin/yangtze # 删除临时分支
然后删除debian/patches目录，将debian/source/format修改为“3.0 (native)”
rm -rf debian/pathes
echo "3.0 (native)" > debian/source/format
提交改动
```



# 3. 开发维护人员修改软件包（在开发分支如openkylin/yangtze下进行维护）

## 3.1. 产线代码修改

开发维护人员克隆正式版的git库到个人空间，然后直接在打包分支上修改源码文件和debian/changelog。

```Bash
git checkout openkylin/yangtze
# 修改源码文件
vim ...
# 修改changelog，按软件包版本号规范修改版本号，对于版本号中带‘-’符号的，不能修改'-'前面的部分
dch -R
git add .
git commit
```

再推送到个人gitee或gitlab仓库，提交PR给maintainer审核。

本地调试时可以直接用debuild进行编译测试。

## 3.2. 在打包分支修改后向上游提交patch

向上游提交改动需要先导出patch

首先执行git log，找到本次修改之前和之后的commit编号

```Bash
git log

commit 5ba63fabc689b225cd7823a81655e531f54eed46 (HEAD -> openkylin/yangtze)
Author: Xie Wei <xiewei@kylinos.cn>
Date:   Mon Apr 18 16:41:59 2022 +0800

    edit version and copying

commit 5c066ae094a7788c6507413891dab66db36cd07b
Author: Xie Wei <xiewei@kylinos.cn>
Date:   Mon Apr 18 16:40:56 2022 +0800

    edit COPYING

commit e5a25c0fba1b1b68e023062878962abbebf1f8ea
Author: Xie Wei <xiewei@kylinos.cn>
Date:   Mon Apr 18 16:40:31 2022 +0800

    edit version

commit 8c8ad046e88f2692bef8b193b517666d82ca6957
Author: Xie Wei <xiewei@kylinos.cn>
Date:   Mon Apr 18 16:35:37 2022 +0800

    native
```

如上所示，假设要把version和copying的改动作为一个完整的patch提交，那么应该对比倒数第1个commit和倒数第4个commit之间的改动。

```Apache
# 生成patch时忽略debian目录，较早的commit放在前面，较新的commit放在后面
git diff --binary -r 8c8ad046e88f2692bef8b193b517666d82ca6957 -r 5ba63fabc689b225cd7823a81655e531f54eed46 ':!debian' > /tmp/live-build-change-version-copying.patch
```

生成patch之后，就要看上游社区接受什么样的提交方式，直接发patch的此文不介绍，假设它支持在gitee或github提交PR，那么我们先在相应的托管平台fork自己的仓库，然后clone到本地，再导入patch

```Apache
# clone自己在托管平台fork的仓库
git clone git@gitee.com:xiewei/live-build.git live-build-upstream
cd live-build-upstream
# 切换到需要提交的分支，取决于patch针对的版本，以及上游社区的分支规定
git checkout master
git apply /tmp/live-build-change-version-copying.patch
git add .
# 提交修改说明
git commit
# push到自己fork的仓库
git push
```

然后就是在托管平台上操作，从自己的仓库提交一个PR到上游社区。

如果上游有更新，提交PR前要先同步上游改动，解决冲突后，再提交PR

```Bash
# 首先将上游仓库的地址添加到remote，clone下来后只需要做一次，因为我们只需要读取，添加https协议的地址就行了。
git remote add upstream https://gitee.com/upstream/live-build.git
# 同步上游更新，最后一个参数是要同步的分支名
git pull upstream master
# 如果有冲突，按照提示解决冲突，没有冲突则可以push到自己fork的仓库了
git push
```



# 4. 上传编译平台（CI平台自动实现）

上传编译平台时，需要尽量以quilt格式上传，充分重用orig.tar文件，节省带宽和服务器空间。

将native格式源码包转化成quilt格式的动作会通过CI平台自动完成。

![img](assets/openKylin%E6%BA%90%E7%A0%81%E5%8C%85git%E5%B7%A5%E4%BD%9C%E6%B5%81/native%E6%A0%BC%E5%BC%8F%E6%BA%90%E7%A0%81%E5%8C%85%E8%BD%AC%E5%8C%96%E6%88%90quilt%E6%A0%BC%E5%BC%8F-%E6%B5%81%E7%A8%8B%E5%9B%BE.png)

## 4.1 以quilt格式打包

在打包环境中clone git库，同步开发分支的代码到打包分支进行打包

native转换成quilt，主要差异在于要相对于upstream的代码改动形成补丁，而不是直接在源码文件上修改，我们利用gbp pq来生成补丁。

```Bash
# 切换到打包分支
git checkout packaging/openkylin/yangtze
# 创建临时patch-queue，自动切换到patch-queue/packaging/openkylin/yangtze
gbp pq import
# 在patch-queue中提交开发分支最新的改动
git diff --binary -r openkylin/yangtze -- ':!debian' | git apply -R
git add . -f
# 把开发分支最新的commit信息提交到patch-queue分支，用于导出patch
git log -r openkylin/yangtze -1 --pretty=format:"%s" | git commit -F -
# 导出patch，自动切换到packaging/openkylin/yangtze
gbp pq export
# 同步开发分支对于debian目录的改动，忽略patches和source/format
git diff --binary -r openkylin/yangtze -- debian ':!debian/patches' ':!debian/source/format' | git apply -R

git add debian
# 把开发分支最新的commit信息提交到打包分支，不需要人工输入
git log -r openkylin/yangtze -1 --pretty=format:"%s" | git commit -F -
# 打包源码，因为gbp默认要在master分支打包，所以要加 --git-ignore-branch 参数，--git-tag参数可以在打包成功后自动创建tag
gbp buildpackage --git-ignore-branch --git-tag --git-pristine-tar --git-pristine-tar-commit -S -nc
# packaging/openkylin/yangtze分支需要自动保存到gitee或者gitlab
git push
git push --tags
```

假如native分支涉及二进制文件修改，生成的patch也会有二进制信息，buildpackage时会提示`detected 1 unwanted binary file (add it in debian/source/include-binaries to allow its inclusion)`

debian推荐把要替换的二进制文件放在debian目录中，再修改debian/*.install或者debian/rules的install步骤，在编译时覆盖上游的二进制文件。debian中包含二进制文件，无论是图片还是patch，都需要将文件路径添加到debian/source/include-binaries中，或者是直接在debian/source/options里添加一行include-binaries，允许包含任何二进制文件。

https://www.debian.org/doc/manuals/debmake-doc/ch08.en.html

> If you wish to replace upstream provided PNG file **data/hello.png** with maintainer provided one **debian/hello.png**, editing **debian/install** isn’t enough. When you add **debian/hello.png**, you need to add a line "`include-binaries`" to **debian/source/options** since PNG is a binary file. See `dpkg-source`(1).

根据版本号获取upstream标签的方法： （在已有patch基础上进行比较，不再直接对比upstream）

```Python
from gbp.deb.git import DebianGitRepository
version = '3.0~a57-1kylin38.20.04.4.4'
upstream_version = version.split('-')[0]
# '3.0~a57'
upstream_tag = DebianGitRepository.version_to_tag('upstream/%(version)s', upstream_version)
# 'upstream/3.0_a57'
```

## 4.2 以native格式打包

native格式软件包可以在开发分支直接打包源码，上传编译平台。

```Bash
# 切换到开发分支
git checkout openkylin/yangtze
# 打包源码，因为gbp默认要在master分支打包，所以要加 --git-ignore-branch 参数，--git-tag参数可以在打包成功后自动创建tag
gbp buildpackage --git-ignore-branch --git-tag -S
# 打包时创建的tag同步到gitee或gitlab
git push --tags
```



# 5. 更新upstream版本（需要maintainer手动处理）

软件包需要更新上游版本号时，要先导入上游源码，并添加相应的标签。这一步需要由maintainer在正式版的git库中操作。

然后开发者可以将打包分支的代码rebase到新版本。

## 5.1 导入上游新版本的源码（修改包）

当软件包的上游版本更新时，需要新导入orig包

```Bash
gbp import-orig ../live-build_4.0.orig.tar.gz --no-merge --pristine-tar --upstream-branch=upstream
git checkout upstream
git push
git push --tags
```

参数含义

| --pristine-tar     | 记录原始orig tar包的数据，避免重新用gbp打包后tar包内容发生变化 |
| ------------------ | ------------------------------------------------------------ |
| --no-merge         | 不将上游版本的代码合并到打包分支，因为我们的开发分支是native格式的，基于upstream修改了源码，一般是没办法自动合并的 |
| --upstream-branch= | 指定上游分支名称，修改包使用upstream                         |

在以上的例子中，会新创建一个tag：upstream/4.0

## 5.2 创建新版本的标签（自研包）

maintainer基于upstream分支（或者其他的上游稳定版本分支）创建upstream/<新版本号>的标签，作为新版本打包的上游。

可以在gitlab和gitee上操作。

## 5.3 打包分支rebase到新版本

### 5.3.1 判断旧版本上是否有patch

```Bash
git checkout packaging/openkylin/yangtze
wc debian/patches/series
# 查看patch数量
```

如果没有patch，按5.3.2、5.3.4操作。

如果有patch，按5.3.3、5.3.4操作。

### 5.3.2 无patch情况下rebase

更新packaging分支

```Bash
git checkout packaging/openkylin/yangtze
# 假设原版本号是2:4.11.6-ok1，新的upstream版本号2:4.13.17
git merge upstream/4.13.17
```

更新开发分支

```Bash
git checkout openkylin/yangtze
# 假设原版本号是2:4.11.6-ok1，新的upstream版本号2:4.13.17
git merge upstream/4.13.17
```

在开发分支修改包版本号

```Bash
# 此时，可以在开发分支对基于上游upstream的代码进行调试，如：
# 1. 修改代码,直到能编译出二进制包以及能正常安装
vim xxx
# 2. 在开发分支下进行debuild进行编译测试
debuild
# 3. 提交改动
git add xxx
# 4. 修改一下debian/changelog，版本号基于新的upstream version。
dch -R
git add debian/changelog
git commit -m 'merge upstream 4.13.17'
```

### 5.3.3 有patch情况下rebase

#### 5.3.3.1 获取旧版本patch对应的commit id

```Bash
# 基于旧版本创建临时分支
git checkout packaging/openkylin/yangtze
# 导入patch，形成git commit记录
# 导入前也可以筛选一下补丁列表，修改debian/patches/series，删除不要的补丁，然后提交即可
gbp pq import --force
# 计算补丁数量
wc debian/patches/series # 14  14 511 debian/patches/series
# 获取commit id，重定向到文件保存
git log --max-count=14  > /tmp/patch-list.txt
```

#### 5.3.3.2 更新packaging分支

```Bash
git checkout packaging/openkylin/yangtze
# 假设原版本号是2:4.11.6-ok1，新的upstream版本号2:4.13.17
git merge upstream/4.13.17
```

#### 5.3.3.3 筛选需要移植的patch，分别cherry-pick

```Bash
# 切换到packaging分支
git checkout packaging/openkylin/yangtze
# 清空patch列表
git rm debian/patches/series
git commit -m 'prepare for new patch list'
# 创建临时分支，移植低版本patch
gbp pq import --force
# 从后往前读取patch-queue commit列表文件 /tmp/patch-list.txt，根据提交信息和补丁文件名确定是否要集成该补丁，对需要集成的补丁执行cherry-pick
git cherry-pick b2cf5dd7923c50c4b4e1809564a7da63a3e38312 f32df40245f054363698225f44158c2673a06978 2853828b649d5c826bda93c9a11a9720954b915c
# 导出patch列表
gbp pq export
git add debian/patches
# 然后记得修改一下debian/changelog，版本号基于新的upstream version。
dch -R
git add debian/changelog
git commit -m 'Apply patches on new upstream 4.13.17'
# patch-queue分支可以删除了
git branch -D patch-queue/packaging/openkylin/yangtze
```

假如存在冲突，根据实际需求进行冲突合并（git mergetool; git cherry-pick --continue）或者跳过此patch（git cherry-pick --skip）。

> git cherry-pick基础用法

> 挑选一个commit-id合并

> ```
> git cherry-pick commit-id
> ```

- > 注意：合并过来的commit-id将会变掉，产生一个新的commit-id，跟原来的不在相同

> 挑选多个commit-id合并

> ```
> git cherry-pick commit-idA commit-idB
> ```

> 挑选连续的多个commit-id合并

> ```
> git cherry-pick commit-idA..commit-idB
> ```

- > 该指令是将从commit-idA开始到commit-idB之间的所有commit-id提交记录都合并过来，需要注意的是，commit-idA必须比commit-idB提前提交，也就是说在被挑选的分支上，先有的commit-idA，然后才有的commit-idB

#### 5.3.3.4 反向同步到开发分支

```Bash
git checkout openkylin/yangtze
git diff --binary -r patch-queue/packaging/openkylin/yangtze -- ':!debian/source/format' | git apply -R
git add .
git commit -m 'merge upstream 4.13.17'
```

### 5.3.4 提交到git服务器

提交开发分支与packaging打包分支即可，其他临时分支不要提交

```
!push时，要先push packaging打包分支，再push 开发分支。
```

```Bash
git checkout packaging/openkylin/yangtze
git push
git checkout openkylin/yangtze
git push
```



# 6. 自研包改为quilt格式

（1）解压源码包。

```Apache
dpkg-source -x kylin-camera_1.2kord01rc2.22.dsc
cd kylin-camera-1.2kord01rc2.22
```

（2）修改changelog，版本号改为{upstream_version}-{revision}形式。

```
dch -R
# 在编辑器中，将版本号从1.2kord01rc2.22改为1.2kord01rc2.22-0k1
```

（3）修改debian/source/format文件，改为3.0 (quilt)

```Bash
mkdir -p debian/source
echo "3.0 (quilt)" > debian/source/format
```

（4）打包orig.tar

```Bash
debmake -t
```

（5）生成新的dsc

```Bash
dpkg-buildpackage -S -nc
```

（6）导入到git

```Apache
cd ../
gbp import-dsc --pristine-tar --debian-branch=openkylin/yangtze --upstream-branch=upstream kylin-camera_1.2kord01rc2.22-0k1.dsc kylin-camera
```



# 7. 自研包不改变代码情况下修改上游版本号

例如ukui在某个节点时，要将当前已导入的包统一升级上游版本号，需要按如下流程操作，

（1）切换到开发分支 （例如openkylin/yangtze）。

```Shell
git checkout openkylin/yangtze
```

（2）读取debian/changelog，确认当前upstream版本。

​		  例如changelog中的版本号为3.10.0-0k0

（3）修改changelog版本号，upstream version改成新版本号，假设要设置为3.13.0，那么changelog中的包版本号可以改成3.13.0-0k0。然后提交改动。

（4）添加对应新版本号的upstream标签。

```Shell
git tag upstream/3.13.0 upstream/3.10.0
```

（5）openkylin/yangtze分支的改动以及新标签upstream/3.13.0推送到git服务器。

```Shell
git push
git push --tags
```

（6）生成新版本orig.tar包（CI平台完成）。

```Shell
# 将changelog改动同步到打包分支
git diff --binary -r openkylin/yangtze debian ':!debian/patches' ':!debian/source/format' | git apply -R
git add debian
git log -r openkylin/yangtze -1 --pretty=format:"%s" | git commit -F -
# 打包，并自动保存orig.tar
gbp buildpackage --git-ignore-branch --git-tag --git-pristine-tar --git-pristine-tar-commit -S -nc
# 打包新的upstream版本后，pristine-tar分支会更新，需要上传到git服务器
git push
git push origin pristine-tar
```



# 8. 使用Kylin修改过的源码覆盖openkylin社区仓库

前提是upstream版本没有改变，为quilt格式，否则需要清空仓库全新导入或者走“更新upstream版本”流程。

（1）在打包分支覆盖debian目录

```Shell
# 切换到打包分支

# 删除现有debian目录，准备用Kylin的版本覆盖
# 解压Kylin版本的*.debian.tar文件
```

（2）修改changelog中的版本号与系列代号

新增一条修改记录，版本号为<upstream version>-ok<N>，N是数字，要比覆盖前openkylin中的版本号大。

（3）覆盖开发分支



# 9. 从头开始实现自动打包

本节介绍如何在只有upstream等不包含debian配置文件的分支时，如何创建开发分支与打包分支

即从upstream建立openkylin/yangtze和packaging/openkylin/yangtze分支。

## 9.1 首先从gitee上的openkylin clone一份干净的代码

```Bash
git clone git@gitee.com:openkylin/<包名>
cd <包名>
```

## 9.2 在upstream分支上添加upstream标签

我们要选择一个上游版本对应的commit，打上upstream标签。

分别可以用以下几种方式实现，假设上游版本号定为1.2.3

（1）以upstream分支最新版本进行打包

```Bash
git tag upstream/v1.2.3 upstream
```

（2）以已存在的tag打包，假设已经有一个tag，名称为v1.2.3

```Bash
git tag upstream/1.2.3 v1.2.3
```

（3）以upstream分支某个commit为基点打包

```Bash
git log upstream # 找到想要使用的commit id
git tag upstream/1.2.3 031eae9e49ef7b523286410d45ee44ba57e29f79
```

## 9.3 基于upstream标签创建打包分支，添加debian打包配置文件

```Bash
git checkout -b packaging/openkylin/yangtze upstream/1.2.3
# 把准备好的debian打包配置文件复制过来
cp -r /path-to/debian ./
# 修改changelog，确保版本号为"1.2.3-"开头，并符合版本号规范
dch -R
# 确保源码包为quilt格式
mkdir -p debian/source
echo "3.0 (quilt)" > debian/source/format
# 测试打包，并保存pristine-tar数据
gbp buildpackage --git-ignore-branch --git-tag --git-pristine-tar --git-pristine-tar-commit -S -nc
```

## 9.4 创建相应的开发分支，方便开发人员修改代码后本地测试

```Bash
# 创建临时的集成patch分支，会自动命名为 patch-queue/packaging/openkylin/yangtze
gbp pq import
# 重命名为标准的开发分支名称
git checkout -b openkylin/yangtze
# 删除临时分支
git branch -D patch-queue/packaging/openkylin/yangtze
# 然后删除debian/patches目录，将debian/source/format修改为“3.0 (native)”
rm -rf debian/pathes
echo "3.0 (native)" > debian/source/format
git add debian
git commit -m 'change format to native'
```

## 9.5 推送到git服务器

```Bash
# 因为新建打包分支涉及到3个分支的修改，推送时使用push --all简化操作，这也是为什么一定要clone一份干净的代码来操作
git push origin --tags
git push origin --all
```

## 9.6 创建新版本系列开发分支

当前openkylin 1.0版本已发布，2.0版本的开发分支名称已确定为openkylin/nile，本节介绍如何实现2.0版本代码自动打包。

如果2.0版本不继承1.0版本代码开发，那么可以参考第9节进行，只需要把分支名称中的yangtze改为nile。

如果2.0版本继承1.0版本代码，由包维护者在gitee上按如下流程操作：

1. 访问仓库的分支列表

![img](assets/openKylin%E6%BA%90%E7%A0%81%E5%8C%85git%E5%B7%A5%E4%BD%9C%E6%B5%81/%E5%88%9B%E5%BB%BA%E6%96%B0%E4%BA%A7%E7%BA%BF%E7%9A%84%E5%88%86%E6%94%AF%E7%A4%BA%E4%BE%8B1.png)

2. 点击“新建分支”

![img](assets/openKylin%E6%BA%90%E7%A0%81%E5%8C%85git%E5%B7%A5%E4%BD%9C%E6%B5%81/%E5%88%9B%E5%BB%BA%E6%96%B0%E4%BA%A7%E7%BA%BF%E7%9A%84%E5%88%86%E6%94%AF%E7%A4%BA%E4%BE%8B2.png)

1. 输入分支名，分别新建两个分支：

​	openkylin/nile 基于openkylin/yangtze

​	packaging/openkylin/nile 基于 packaging/openkylin/yangtze



# 10. 已有native格式的分支，转换成quilt格式

（1）先基于当前版本创建upstream分支和tag

```Bash
# 假设原来的分支名为master,changelog中的版本号为1.2.3，要创建的开发分支为openkylin/yangtze
# 创建upstream分支
git checkout -b upstream master
# upstream分支不应该有debian目录
git rm -rf debian
git commit -m 'import upstream 1.2.3'
# 创建一个upstream标签，版本号不能包含 - 符号
git tag upstream/1.2.3
```

（2）创建打包分支

```Bash
# 创建打包分支
git checkout -b packaging/openkylin/yangtze master
# 版本号和源码包格式改为quilt
dch -v 1.2.3-0k1 "quilt format" -D v101 --force-distribution
echo '3.0 (quilt)' > debian/source/format
git add debian
git commit -m 'to quilt format'
# 基于打包分支创建新的开发分支
git checkout -b openkylin/yangtze
echo "3.0 (native)" > debian/source/format
git commit -m 'to native format'
# 测试打包，并保存pristine-tar信息
git checkout packaging/openkylin/yangtze
gbp buildpackage --git-pristine-tar --git-pristine-tar-commit --git-ignore-branch -S -nc
```


# 附：个别问题处理

**1.**

gbp:error: Error creating libwacom_1.3.orig.tar.xz: Pristine-tar couldn't checkout "libwacom_1.3.orig.tar.xz": fatal: Path 'libwacom_1.3.orig.tar.xz.delta' does not exist in 'refs/remotes/origin/pristine-tar'

原因是初始导入的压缩包是gz格式的，debian/gbp.conf中却指定了压缩格式为xz。

解决方法，在openkylin/yangtze分支修改debian/gbp.conf，删除compression=xz，提交。

**2.**

gbp:error: Error creating kbd_2.0.4.orig.tar.xz: Pristine-tar couldn't checkout "kbd_2.0.4.orig.tar.xz": xdelta3: target window checksum mismatch: XD3_INVALID_INPUT

压缩格式一致，但是checksum不同，可能是导入时文件损坏或者环境和ci环境版本不同导致的，需重新导入一次

**3.**

dpkg-source: error: detected 1 unwanted binary file (add it in debian/source/include-binaries to allow its inclusion).

假如开发分支涉及二进制文件修改，生成的patch也会有二进制信息，buildpackage时会提示`detected 1 unwanted binary file (add it in debian/source/include-binaries to allow its inclusion)`

debian推荐把要替换的二进制文件放在debian目录中，再修改debian/*.install或者debian/rules的install步骤，在编译时覆盖上游的二进制文件。debian中包含二进制文件，无论是图片还是patch，都需要将文件路径添加到debian/source/include-binaries中，或者是直接在debian/source/options里添加一行include-binaries，允许包含任何二进制文件。

https://www.debian.org/doc/manuals/debmake-doc/ch08.en.html

> If you wish to replace upstream provided PNG file **data/hello.png** with maintainer provided one **debian/hello.png**, editing **debian/install** isn’t enough. When you add **debian/hello.png**, you need to add a line "`include-binaries`" to **debian/source/options** since PNG is a binary file. See `dpkg-source`(1).

