---
title: ARTS089
tags: ARTS
date: 2023-03-27 17:20:28
---

AI 写码 YYDS！

<!--more-->

# Algorithm

None

# Review

None

# Tips

用 AI 帮同事写了一个提升效率的脚本，小白同事配置 Python 环境的时候遇到了问题。发送网络请求的时候一直报错如下：

> unable to get local issuer certificate
> 

Google 搜索了半天才找到解决方式，需要手动运行一下 `install_certificates.command` 命令。

问了一下 AI 为什么需要这个文件，得到的回答如下：

`install_certificates.command` 是在 macOS 上安装 Python 时，Python 安装器自动添加到 Python 应用程序文件夹中的一个可执行文件。它的目的是在 Python 运行时配置`ssl`模块，确保 Python 可以正确地使用 HTTPS 请求和其他加密方案。

当你使用 Python 发起 HTTPS 请求（例如使用`requests`模块时），Python 应该能够验证对等方的 SSL 证书，以确保你正在与正确的服务器通信，并且你的通信是加密的。如果缺少必要的证书，就会遇到 SSL/TLS 验证错误。

在 macOS 上使用 Python 安装器安装 Python 默认会包括此文件并执行，它将确保你的 Python 安装程序包括可以成功验证加密连接所需的本地 SSL 证书目录。如果你在 macOS 上手动安装 Python，或使用其他操作系统，则可能需要在安装 Python 后手动设置 SSL 证书（或针对不受信任的 SSL 证书执行其他必要的步骤）。

因此，如果你在 macOS 上安装了 Python，则不需要手动运行`install_certificates.command`文件。它会在 Python 安装时自动执行。

顺着 Stackoverflow 的问题又找到了 brew 的 issue。

https://github.com/Homebrew/homebrew-core/issues/42198

[Pipeline: Basic Steps](https://www.jenkins.io/doc/pipeline/steps/workflow-basic-steps/#wrap-general-build-wrapper)

Jenkins 配置 Pipeline 的时候经常看到一个 wrap 的语法，后面跟着一个莫名其妙的 class，有时候还有别的一些参数。终于在这里找到这个语法的说明了。

PipelineSyntax 里面没有这个说明，如果想知道怎么找对应的 Class，可以在 Job Config 页面，点击 Pipeline Syntax，进入 Snippet Generator 页面，就可以根据 Jenkins 已经安装的 Plugin 生成对应的模板以供参考了。

# Share

None
