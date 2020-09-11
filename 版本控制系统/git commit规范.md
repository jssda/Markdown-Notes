## 格式

目前业界常用的是[Angular 团队的规范](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)，主要的格式如下。

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

第一行是**Header**，是必须的。不管是哪一个部分，任何一行都不得超过100个字符。这是为了避免自动换行影响美观。

- 标题行(第一行/header): 必填, 描述主要修改类型和内容
- 主题内容(body): 描述为什么修改, 做了什么样的修改, 以及开发的思路等等
- 页脚注释(footer): 放 Breaking Changes 或 Closed Issues
- scope: commit 影响的范围, 比如: route, component, utils, build...
- subject: commit 的概述, 建议符合 50/72 formatting
- body: commit 具体修改内容, 可以分为多行, 建议符合 50/72 formatting
- footer: 一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接.

## Header

Header部分只有一行，包括三个字段：`type`（必需）、`scope`（可选）和`subject`（必需）。

### **type（必填）**：

`type`用于说明 commit 的类别。

- feat：新增功能
- fix：bug 修复
- docs：文档更新
- style：不影响程序逻辑的代码修改(修改空白字符，格式缩进，补全缺失的分号等，没有改变代码逻辑)
- refactor：重构代码(既没有新增功能，也没有修复 bug)
- perf：性能, 体验优化
- test：新增测试用例或是更新现有测试
- build：主要目的是修改项目构建系统(例如 glup，webpack，rollup 的配置等)的提交
- ci：主要目的是修改项目继续集成流程(例如 Travis，Jenkins，GitLab CI，Circle等)的提交
- chore：不属于以上类型的其他类，比如构建流程, 依赖管理
- revert：回滚某个更早之前的提交

### **scope（可选）**：

`scope`用于说明 commit 影响的范围，比如数据层、控制层、视图层等等，视项目不同而不同。

### **subject（必填）**:

`subject`是 commit 目的的简短描述，

- 以动词开头，使用第一人称现在时，比如`change`，而不是`changed`或`changes`
- 第一个字母小写
- 结尾不加句号（`.`）

## Body（可省）

Body 部分是对本次 commit 的详细描述，可以分成多行。

有两个注意点。

（1）使用第一人称现在时，比如使用`change`而不是`changed`或`changes`。

（2）应该说明代码变动的动机，以及与以前行为的对比。

## Footer（可省）

Footer 部分只用于两种情况。

### 1）不兼容变动

如果当前代码与上一个版本不兼容，则 Footer 部分以`BREAKING CHANGE`开头，后面是对变动的描述、以及变动理由和迁移方法

### 2）关闭 Issue

如果当前 commit 针对某个issue，那么可以在 Footer 部分关闭这个 issue 。

## Revert

还有一种特殊情况，如果当前 commit 用于撤销以前的 commit，则必须以 revert: 开头，后面跟着被撤销 Commit 的 Header。

```
revert: feat(pencil): add 'graphiteWidth' option

This reverts commit 667ecc1654a317a13331b17617d973392f415f02.
```

 

Body部分的格式是固定的，必须写成 This reverts commit &lt;hash>. ，其中的 hash 是被撤销 commit 的 SHA 标识符。

如果当前 commit 与被撤销的 commit，在同一个发布（release）里面，那么它们都不会出现在 Change log 里面。如果两者在不同的发布，那么当前 commit，会出现在 Change log 的 Reverts 小标题下面。

## 参考资料及工具

https://blog.csdn.net/itguangit/article/details/99590995