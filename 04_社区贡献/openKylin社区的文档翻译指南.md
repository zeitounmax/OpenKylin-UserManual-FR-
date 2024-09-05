## 签署 CLA
按照社区的要求，贡献者需要签署 CLA。
## 目录结构
主仓库文档的译文建议放到 [en](https://gitee.com/openkylin/docs/tree/master/en) 文件夹下对应的目录，英文的目录树应当与中文的目录树一致。SIG 成员会有专人去进行审核。
## 文档的类别
为了避免开发者对各类文档的混淆，这里简单介绍一下区别。

规范（Specification）一般是技术类文档，比如，如何编译软件，如何翻译文档……

政策（Policies）一般是管理类文档，偏向于章程或纲领，比如，社区的理念，社区的组织架构……

规则（Rules）一般也是管理类文档，偏向于守则或规章制度，比如，代码的版权，讨论问题的礼仪……

## 命名规范
如果目录有空格则全部用短横线“-”隔开，文件名有空格用下划线“_”隔开。
## 查重
翻译前请先查阅文档库和搜索引擎，看看是否已有文档的译文。这里有2种情况。

（1）中文版是初始版本，已有相应的英文译文。

（2）英文版是初始版本，中文版是从英文翻译而来。

如果是情况（1），目前文库里已经有了一篇译文。此时你如果提交 PR 可以有 2 个方案。

（1）给原文打补丁，对原文进行修改。

（2）提交新文档。

## 插图的路径
译文中的插图应当放置在译文同级目录的 assets 文件夹，不要与译文并列存放。引用图片时使用相对路径“./”。这样做的好处是：如果上级文件夹被修改，引用图片的路径不会失效。

## 插图里文本的翻译
如果条件允许，建议将插图中的中文界面换成英文界面，重新截取。

## 机器翻译和人工校验
第一，我不反对机器翻译，但是，选择的翻译工具具备可靠性。

第二，人工校对不可缺少，而且，要认真仔细。

## 译文里的常见问题

1、注意英语中的语法、词性和时态。

差：In order to better manage issues, we divide participants into the following roles:

好：For better issues "management" , we "divided" the participants into the following roles:

2、译文要准确表达原文的意思。
对各项目进行测试。

差：Test projects。

好：Testing of various projects。

3、注意用词的统一。
中文原文里，分类标签应该翻译为“Kind Tags”。这不是对错的问题，是一种约定吧。

标签，这里不翻译为“label”，应为“Tags”。

参与者，出现了“participant”和“contributor”2种译文。

提出，出现了“proposed”和“raised”2种译文。

## 注意事项
如无特殊说明主仓库指的是[docs仓库](https://gitee.com/openkylin/docs)，副仓库指的是[sig-documentation](https://gitee.com/openkylin/sig-documentation)。
一般情况下，爱好者写的教程都会放到副仓库，官方提供的文档则会放到主仓库。但此种情况并不绝对，sig组也会根据写的内容来做出对应的建议。