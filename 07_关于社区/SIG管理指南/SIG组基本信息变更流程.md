---
title: 03-SIG组基本信息变更流程
description: 
published: true
date: 2022-06-23T06:18:11.712Z
tags: 
editor: markdown
dateCreated: 2022-03-11T03:16:26.469Z
---

# SIG 组基本信息变更流程

## SIG组名称变更

1. SIG组提交改名Pr申请，注意修改覆盖gitee上所有显示SIG组名的地方，避免SIG组名不一致的情况发生。一般涉及到：community/sig/xxxx目录名、sig.yaml文件里的name字段、README.md文件里的SIG组名；
2. 管理员审核是否符合SIG组命名规范等；
3. 审核通过后合并修改，管理员通知社区运营人员修改openKylin官网、邮件列表等涉及到SIG组名的地方。

附SIG组命名规范
1. 全拼的SIG名，只有一个单词的则首字母大写；多个单词的每个单词首字母大写且中间不加连字符号，例如：Release、BootAndInstall等； 
2. 特殊名词或者必须缩写的SIG名字母全部大写，例如：RISC-V、QA等； 
3. SIG命名不得侵犯任何第三方的合法权益（包括但不限于未经授权使用的第三方的商标、商号、标识等）；
4. 社区拥有对SIG命名的最终审核权。

## 团队成员/成员权限变更
** SIG 组成立之后，如有成员变更需求，请按照以下步骤执行申请：**
    1. 由相关提议人 Fork 项目 [openKylin / community](https://gitee.com/openkylin/community)  到您的 Gitee 下。并根据变动修改您的 Gitee 项目下的对应 SIG 组目录下的 `README.md`；
    2. 根据变动在对应 SIG 组目录下完成对 sig.yaml 文件的修改；
    3. 完成以上两步后，将以上改动提交到 Gitee 上，并向 [openKylin / community](https://gitee.com/openkylin/community) 项目提交 PR 申请 SIG 组成员变更(如新增成员，则新成员需先签署cla)，填写好相关信息后，技术委员会将提前审核相关信息，并在下一次例会上进行进一步沟通。

## SIG 组维护包列表变更
** SIG 组成立后，如有包列表更新需求，请按照以下步骤执行申请：**
    1. 由相关提议人 Fork 项目 [openKylin / community](https://gitee.com/openkylin/community) 到你的 Gitee 下。并根据变动修改您的 Gitee 项目下的对应 SIG 组目录下的 `README.md`；
    2. 根据变动在对应 SIG 组目录下完成对 sig.yaml 文件的修改；
    3. 完成以上两步后，将以上改动提交到 Gitee 上，并向[openKylin / community](https://gitee.com/openkylin/community) 项目提交 PR 申请 SIG 组包列表变更，填写好相关信息后，技术委员会将提前审核相关信息，并在下一次例会上进行进一步沟通。