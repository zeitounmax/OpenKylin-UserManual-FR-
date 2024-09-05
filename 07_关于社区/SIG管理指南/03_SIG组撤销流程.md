---
title: SIG组撤销流程
description: 
dateCreated: 2024-08-13
---

## SIG 组的撤销规范
### 撤销原则

以下情形发生时可以由该SIG 组Owner或者技术委员会委员提出撤销 SIG 组申请： 

* SIG 组长时间活跃度很低，无法维持日常运转。包括但不限于：SIG组长时间（超过6个月）没有召开过例会、从未参与过社区SIG组相关活动（包括版本发行）、 所参与或负责的软件仓库超过6个月没有代码更新、社区反馈的issues超过3个月未响应等。
* SIG 组负责openKylin版本中重要的模块或技术方向，但是目前的工作无法满足openKylin版本对该模块或技术方向的要求，阻碍了openKylin版本的发布和技术发展；
* SIG组的目标规划、技术路线等与另外的SIG组有重合；
* 其他技术委员会认为需要撤销SIG组的情形。

### 撤销流程
#### 提交撤销申请
SIG组撤销申请应由该SIG组Owner或者技术委员会委员提交，提交方式如下：
1. Fork 项目 [openKylin / community](https://gitee.com/openkylin/community) 到您的 Gitee 下，并删除 [community/sig](https://gitee.com/openkylin/community/tree/master/sig)目录下该SIG组对应的目录。
2. 完成以上步骤后，将改动提交到 Gitee 上，并向[openKylin / community](https://gitee.com/openkylin/community) 项目提交 PR 申请撤销 SIG 组，填写好相关信息（撤销SIG组的详细原因）后，技术委员会将提前审核相关信息，并在下一次例会上针对该议题进行讨论和投票。
#### 讨论并投票表决
SIG组撤销申请应在技术委员会例会上进行讨论并通过投票决策。技术委员会全体委员需参与投票表决，投票分为赞同票、反对票和弃权票，可以在例会上直接表决或会后回复邮件表决。需要三分之二或以上委员投赞同票时，撤销申请才能通过。投票结果通过邮件列表公示。  
当 SIG 组被撤销后，该 SIG 组名下需要继续维护的软件包将暂时划分到 Packaging SIG组，并通过邮件列表公示，这些软件包可以由其他 SIG 组或者其他成立新的SIG组来认领维护。
