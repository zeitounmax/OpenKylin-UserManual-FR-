---
title: issue报告规范
description: 
published: true
date: 2022-05-17T07:16:52.060Z
tags: 
editor: markdown
dateCreated: 2022-03-11T03:17:41.558Z
---

# issue报告规范


## 角色定义
为了更好的管理 issue，我们将参与者划分为以下角色  

| 角色 | 职能 |
| :---: | :---: |
| 测试人员 | 对 openKylin 社区下的各项目进行测试 |
| 开发人员 |  openKylin 社区下的各项目的实际开发与维护者 |
| 社区参与人员 | 社区爱好者、参与者 |

<br>

## Issue 分类标签
在中大型项目中，不同的参与者对项目参与程度不同。为了便于每位参与者发现并解决 issue，我们需要对 issue 进行分类。 openKylin 社区下的各项目默认有 9 个约定俗成的分类标签。 

| 标签名 | 描述 |
| :---: | :---: |
| kind/bug | 该 issue 是参与者提出的未知 bug |
| kind/documentation | 该 issue 文档相关的问题 |
| kind/duplicate | 该 issue 是参与者提出的已知 bug |
| kind/enhancement | 该 issue 请求添加新功能特性或者优化已有功能 |
| kind/good-first-issue | 该 issue 对新接触项目的开发人员或者测试人员有帮助 |
| kind/help-wanted | 该 issue 需要寻求其他人员的帮助，或者项目新参与人员提的帮助问题 |
| kind/invalid | 该 issue 是错误的或不可用的 |
| kind/question | 该 issue 是存疑的或需要提供更多信息|
| kind/wontfix |	该 issue 在可见的项目排期内是不计划实现的功能需求或者修复 |  
  
<br>

## Issue 状态标签
事实上，不同的 issue 对项目的影响程度或者说需要解决的急迫程度大有不同。为了对 issue 的优先级以及当前状态进行追踪，  openKylin 社区约定了许多有用的标签。

<br>

### 优先级标签
| 标签名 | 描述 |
| :---: | :---: |
| priority/high | 高优先级的，重要紧急需要尽快处理的 issue |
| priority/medium |	中等优先级的，重要不紧急的 issue |
| priority/low | 低优先级的，不重要不紧急，一般用于小问题，或者需要确认排期计划的 issue |

<br>

### 优先级标签的颜色值：
- priority/high:  <span style="padding:5px; border-radius:4px; background-color: #ff0000;color:white;">#ff0000</span>
- priority/medium:  <span style="padding:5px; border-radius:4px; background-color: #ff3366;color:white;">#ff3366</span>
- priority/low:  <span style="padding:5px; border-radius:4px; background-color: #aa8899;color:white;">#aa8899</span>

<br>

### Issue 状态标签
| 标签名 | 描述 |
| :---: | :---: |
| status/confirmed | 项目维护者已确认 issue 描述的问题存在 |
| status/resolved | 项目维护者已处理 issue 描述的问题，审阅无误后 close issue |
| status/reopen | 标记为 resolve 的 issue 再次出现问题 |
| status/need-assign | 该 issue 需要指定参与者负责 |  

<br>

### issue 状态标签的颜色：
- status/need-assign:  <span style="padding:5px; border-radius:4px; background-color: #ffff00;">#ffff00</span>
- status/confirmed:  <span style="padding:5px; border-radius:4px; background-color: #00ffff;">#00ffff</span>
- status/resolved:  <span style="padding:5px; border-radius:4px; background-color: #00ff00;">#00ff00</span>
- status/reopen:  <span style="padding:5px; border-radius:4px; background-color: #ff6666;">#ff6666</span>

<br>

## 标签扩展
如果因为项目需要，而另外添加新标签，新标签不应该与已有的标签产生歧义，且应该说明该标签的用法以及规范，并更新到本文档，以保证项目参与人员的统一认知。

<br>

## Issue 工作流

### 测试人员 issue 工作流
1. 对于内测发现的 bug ，提交之后打上 kind/bug 标签，同时标明优先级 ( priority/high / priority/medium / priority/low );
2. 对于其他人提交的 issue ，根据提交内容，打上 kind/bug / kind/enhancement / kind/invalid / kind/documentation，并对 bug 进行验证，验证通过则评估其优先级，添加或者修改优先级标签;
3. 如果能明确责任人可直接在 issue 内分配到人，否则等待开发者自己领取或者项目负责人去分配;
4. 对于标注为 status/resolved 标签的 issue ，进行回归测试，如果验收通过则关闭该 issue ，不通过则将 status/resolved 标签替换为 status/reopen;
5. 对于验收通过的 issue 再次出现，需要将该 issue 重新打开，并打上 status/reopen 标签;

### 开发人员 issue 工作流
1. 对标注为 kind/bug 的 issue 进行自我验证，验证通过，打上 status/confirmed 标签;
2. 对于 status/confirmed 的 bug 进行领取(分配给自己);
3. 对于分配到自己名下的 issue ，如果不能复现或者有疑问，则打上 kind/question 标签让 issue 提交者提供更多信息;
4. 如果分配的 bug 已经修复，则改为 status/resolved 标签，让测试人员回归测试。如果自己名下有 status/reopen 的 issue , 请及时处理。
5. 对于暂时没时间处理需要别人帮助的 issue ，打上 kind/help-wanted 标签。
6. 对于可见的项目排期内不会去实现的功能需求或者修复的问题，打上 kind/wontfix。

### 社区参与人员 issue 工作流
1. 所有人发现问题或需求可提交 issue;
2. 可协助确认bug(在issue下回复);
3. 可对未分配到人的 issue 进行认领(不在优麒麟社区下的各项目内的人员，在 issue 下进行回复即可)，优先认领 kind/help-wanted 的 issue.

### Bug Issue
测试或者使用 openKylin 过程中遇到了已有功能问题，提出的Issue归属于该类型。 Issue模版如下：  

> ISO版本: ISO 的版本号
> 
> 测试环境: 描述测试的基础环境，cpu架构、机型等
> 
> 测试步骤: 复现问题的步骤
>
> 1. xxx
> 2. xxx
> 3. xxxx
>
> 预期结果： 测试步骤正常应该出现的结果. 
>
> 实际结果： 描述测试过程中出现的问题  
>
> 重现概率： 必现 or 偶发
>
> 截图： 辅助问题说明的截图 
>
> 补充说明： 需要补充说明的内容.


### Feature Issue
 openKylin 社区下的各项目持续发开过程中提出的新的功能需求，或者用户在使用 openKylin 社区下的各项目过程中希望添加功能，提出的Issue归属于该类型。 Issue模版如下：
> 功能需求： 描述需求功能点。  
>
> 设计方案： 描述该需求的设计方案。  
>
> 补充说明： 需要补充说明的内容。

以上就是 BUG 和 Issue 的提交规范，欢迎各位向 openKylin 社区贡献力量，本规范也在持续更新中...
