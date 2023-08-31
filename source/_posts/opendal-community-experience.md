---
title: 加入 OpenDAL 社区是一种怎样的体验
date: 2023-08-30 22:53:44
tags: 
---

![谢邀!](/images/image.png)

谢邀，本文将以一个开源社区的新人的视角来讲述我在 OpenDAL 开源社区中的体验

## 完整且方便的集成测试

在我向 OpenDAL 贡献代码时最大的感受就是：**不怕写错**

这背后的底气自然不是我水平有多高（相反是一个Rust苦手），而是覆盖全面的测试，OpenDAL 可以让用户方便地对不同的存储服务的数据进行访问，这就意味着 OpenDAL 的测试中涉及大量和某个具体存储服务相关的部分。

但并不是每个开发者的电脑上都有各种各样的能够模拟这些存储服务的环境，OpenDAL 使用 GitHub Actions 来自动化测试，这样每次提交代码都会自动触发测试。截止到我写这篇文章为止，OpenDAL 的 `.github/workflows` 目录下已经有了 56 个文件。
![opendal 的 ci](/images/opendal-ci.png)


## 快速的反馈和顺畅的沟通

这一点主要体现在各位 committer 的 review 上，我在提交 PR 后，基本上都能在一天之内收到反馈。这大大降低了我在开发时「上下文切换」的次数，很少出现一个 PR 会因为 committer 没有 review 而被阻塞住的现象。

另外，除了在 GitHub / 邮件列表 的异步沟通外。OpenDAL 还有一个 Discord 频道，当一些需要沟通的问题介于 「完全异步」 和 「及时消息」之间时，使用 Discord 就非常方便了。借助 Discord 的 「子区」功能，同一个话题的历史记录会被归档为一个 Thread中， 而不污染整个频道的时间线，当需要时可以方便地查看，这样的做法既保证了信息的公开性，也不会让频道变得杂乱无章。

对于没有 Discord 的开发者，OpenDAL 借助 Linen 将信息同步出去，你可以不注册 Discord 账号，像浏览邮件列表一样查看这些信息：https://www.linen.dev/d/opendal

## 公开的协作

OpenDAL 使用 GitHub Issues 来追踪不同工作的进度，这样做可以让任何一个刚来到社区的人都能够快速地了解社区的工作进展，也可以让社区的工作更加透明。

我本人参与了开源之夏2023中 OpenDAL 发布的将 Fuzz test 集成到 OSS-Fuzz 中的项目。

在项目申请阶段，我借助 Good First Issues 作为一个切入口，成功的向 OpenDAL 提交了第一个 PR。

{% link https://github.com/apache/incubator-opendal/pull/2310 desc:true %}

并且在我开始做这个项目之前，已经有社区的小伙伴提出了他的解决方案，这也为我撰写项目的申请书，提供了参考。

{% link https://github.com/apache/incubator-opendal/issues/1250 desc:true %}

在项目开始后，我的导师 [Xuanwo](https://xuanwo.io/) 也第一时间提醒我应当创建一个 Issues 来追踪这个项目的进度。

{% link https://github.com/apache/incubator-opendal/issues/2551 desc:true %}

总之，这样的做法不仅给我带来了便利，也给社区带来了透明度。

