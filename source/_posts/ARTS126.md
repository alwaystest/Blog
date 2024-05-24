---
title: ARTS126
tags: ARTS
date: 2024-05-24 18:15:54
---

听过很多道理，却依旧过不好这一生。

<!-- more -->

# Algorithm

none

# Review

[# Python: Mark Homebrew Python as externally managed](https://github.com/orgs/Homebrew/discussions/3404)
新电脑配置环境，突然发现 `pip install` 报错了。
原来是有一个新的提案在 Brew 安装的 Python 3.12 被启用了。
工作中暂时用到 Pyton 的场景还不是特别多，提案解决的依赖冲突的问题还没怎么遇到过。不过在 AI 的帮助下快速的了解了一波这个提案。
懒人方案暂时用 pipx 来解决这个问题，再研究一下 pipx 的解法是什么思路。

[Introducing npx: an npm package runner | by Kat Marchán | Medium](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b)
顺藤摸瓜居然还发现了 NodeJS 中对应的工具 npx
# Tips

[为 Android 换上任意喜欢的字体，你可以试试这个 Magisk 模块 - 少数派](https://sspai.com/post/58049)
很久之前折腾 Terminal 的时候遇到过字体缺失导致无法渲染的问题。想摸清楚字体选择及 Fallback 的逻辑很久了。最近终于看到一篇文章描述 Android 下的字体选择机制及全局替换字体的思路的，学到了。

# Share

PEP 668 – Marking Python base environments as “externally managed”

## 什么是 PEP

PEP for **Python Enhancement Proposal** 是一个设计文档，是 Python 社区用来提出新特性、收集社区意见以及记录设计决策的机制。每一个 PEP 都是一个设计文档，详细描述了某个新特性或改进的技术细节和实现方案。

PEP 的主要流程包括以下几个步骤：
1. 提出想法，建议每个 PEP 只包含一个关键特性或新想法，以提高成功率
2. 初步讨论，撰写 PEP 之前，作者应该在适当的论坛上讨论这个想法，确保它具有接受的可能性
3. 撰写草案，提交给 PEP 编辑团队进行审查
4. PEP 编辑团队和 Python 核心开发者会对 PEP 进行审查，最终由 Python 指导委员会决定接受或拒绝该 PEP

### FYI
PEP 讨论的渠道有很多种，包括 Python 邮件列表，Python 社区论坛，Github 等多个渠道。
PEP 编辑团队成员的选拔主要基于他们在 Python 社区的经验和贡献。
Python 指导委员会的成员通常是通过选举产生的，成员包括经验丰富的核心开发者和社区领袖。

举个例子：
[PEP 8104 – 2023 Term Steering Council election | peps.python.org](https://peps.python.org/pep-8104/)
这个 PEP 记录了 2023 年指导委员会的选举情况。

以上我们可以一窥 Python 特性的发展和演进过程。

## 什么是 PEP668

[PEP 668 – Marking Python base environments as “externally managed” | peps.python.org](https://peps.python.org/pep-0668/)

PEP 668 是为了应对在系统管理的 Python 环境中随意安装包所带来的问题而提出的。它的主要目的是保护系统环境的稳定性，避免因为不兼容的包导致系统问题。

如果你是一个摸爬滚打多年的开发，大概率在你的日常工作流程中接触过 Python 脚本。
举个例子来说：使用 Python 发起网络请求，我们通常会选择 `requests` 这个库。
我们的 CI Job 在打包完成后会执行一个 Python 脚本，通过 requests 库发起网络请求，进行结果通知。
这个脚本平安无事的运行了若干年，直到某天，一个新项目 B 的 CI Job 调用了 requests 库的某个新功能，于是我们自信的执行了这条命令：
```bash
sudo pip3 install requests
```
触发 B 项目的 CI Job，一切都很完美，执行成功了。

一周之后，A 项目有一个改动需要打包，打包完成之后你惊奇的发现：诶？CI Job 怎么挂了？
排查了好久，才发现原来是 requests 库升了一个大版本，存在不向前兼容的改动，导致 A 项目的脚本运行失败了。

当上面的这个依赖问题发生在某个系统工具上，问题就会更加严重，可能影响到系统的稳定性。

## PEP668 的原理

那么 PEP668 是怎么做的呢？

PEP 668 提出了一个机制，允许操作系统的包管理器在系统的 Python 环境中创建一个标记文件，以指示该环境是“外部管理”的。具体操作如下：

1. **创建标记文件**： 操作系统的包管理器会在 Python 环境的特定目录下创建一个名为 `EXTERNALLY-MANAGED` 的文件。这个文件的存在表明该环境是“外部管理”的。
    例如，在 Debian 或 Ubuntu 系统中，这个文件可能会位于 `/usr/lib/python3.x/EXTERNALLY-MANAGED`。
2. **`pip` 检查标记文件**： 当用户尝试在这个环境中使用 `pip` 安装包时，`pip` 会检查这个标记文件。如果发现该文件存在，`pip` 会拒绝在这个环境中安装包，并提示用户这是一个“外部管理”的环境。

什么是“外部管理”？

“外部管理”（Externally Managed Environment）是指系统的 Python 环境由操作系统的包管理器（如 `apt`、`dnf`、`yum` 等）进行管理，而不是由 Python 的包管理工具（如 `pip`）进行管理。在这种环境中，Python 包的安装、升级和删除操作应该通过操作系统的包管理器来完成，而不是直接使用 `pip`。
当然，在 Mac 下，我们使用 Brew 进行包管理，而使用算 Brew 安装 Python 之后，也会设置这个 Flag，避免出现上述问题。相关讨论可以在这里看到：
[Python: Mark Homebrew Python as externally managed · Homebrew · Discussion #3404 · GitHub](https://github.com/orgs/Homebrew/discussions/3404)

## PEP668 推荐我们怎么做

既然封禁了直接安装的方式，总得提供另一套可行的方案。
PEP 668 鼓励用户使用虚拟环境（如 `virtualenv` 或 `venv`）来安装和管理包。虚拟环境是一个独立的 Python 环境，用户可以在其中自由安装包，而不会影响系统环境。

### 使用虚拟环境

虚拟环境是一个独立的 Python 环境，它包含了自己的 Python 解释器和一套独立的 Python 包。通过使用虚拟环境，用户可以在不影响系统环境的情况下安装和管理包。
#### virtualenv

`virtualenv` 是一个第三方工具，用于创建隔离的 Python 环境。它的工作原理如下：

1. **创建目录**：`virtualenv` 会在指定的目录下创建一个新的文件夹，用于存放虚拟环境的所有文件。
2. **复制 Python 解释器**：`virtualenv` 会将系统的 Python 解释器复制到虚拟环境的目录中。
3. **创建独立的包目录**：`virtualenv` 会在虚拟环境中创建一个独立的 `site-packages` 目录，用于存放安装的包。
4. **修改环境变量**：当你激活虚拟环境时 `virtualenv` 会修改环境变量，使得在激活虚拟环境后，所有的 Python 命令（如 `python` 和 `pip`）都指向虚拟环境中的解释器和包目录。具体来说，它会修改 `PATH` 环境变量，将虚拟环境的 `bin`（或 `Scripts`）目录添加到 `PATH` 中。
#### venv

`venv` 是 Python 3.3 及以上版本自带的模块，用于创建虚拟环境。它的工作原理与 `virtualenv` 类似，但有一些不同之处：

#### virtualenv 和 venv 的比较

- **兼容性**：`virtualenv` 支持 Python 2 和 Python 3，而 `venv` 仅支持 Python 3.3 及以上版本。
- **功能**：`virtualenv` 提供了一些额外的功能，如支持不同版本的 Python 解释器，而 `venv` 的功能相对简单。
- **安装**：`venv` 是 Python 标准库的一部分，不需要额外安装，而 `virtualenv` 需要通过 `pip` 安装。

### 最佳实践

- 始终在虚拟环境中进行开发和测试，避免直接在系统环境中安装包。
- 定期更新虚拟环境中的包，确保使用最新的版本。
- 使用 `requirements.txt` 文件记录项目依赖，方便在新的虚拟环境中快速安装所有依赖包。

### 偷懒实践

比如说我们的打包机上跑着好多个老项目，其中都用到了 `requests` 库，进行 CI Job 的结果通知。
我们又没有足够的时间挨个把所有项目都重构成使用 `venv`。
我就想起了很久之前折腾过的另一套工具 `pyenv`

pyenv 主要用于管理和切换不同版本的 Python 解释器，而不是用于管理项目的依赖包。虽然 pyenv 可以与 `pyenv-virtualenv` 插件结合使用来创建虚拟环境，但其主要目的是解决 Python 版本管理问题。

在我们的使用场景中，打包机上通过 brew 安装的 Python 被添加了 PEP668 的标记，不能直接安装包。但是 `pyenv` 安装的 Python 环境是没有这个限制的。
那么我们就可以通过 `pyenv` 安装需要的 Python 版本，并安装对应的依赖，让打包机默认使用通过 `pyenv` 安装的 Python 解释器就能临时解决这些问题。

我再贴心的给出 `pyenv` 的简单使用方法

**切换 Python 版本**

- 全局切换： 设置全局默认的 Python 版本：

```bash
`pyenv global 3.8.10`
```

- 本地切换： 在当前目录下设置特定的 Python 版本（仅对当前目录及其子目录有效）：

```bash
`pyenv local 3.8.10`
```

- 临时切换： 临时使用特定的 Python 版本（仅对当前终端会话有效）：

```bash
`pyenv shell 3.8.10`
```

切换完成之后即可自由使用 `pip3 install xxx`
