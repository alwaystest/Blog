---
title: Cursor 使用指南
date: 2025-03-07 11:14:39
tags: Share
---

AI 助力开发的工具日新月异，需要及时更新知识库。

本文使用 Obsidian 进行编辑，分享过程使用 Slides Extended 进行演示。

<!-- more -->

# Cursor 使用指南

---

## 1. 引言

---

### 为什么选择 Cursor
#### 核心优势
1. **智能编码体验**
   - 上下文感知的 Tab 补全（[官网](https://www.cursor.com/)）
   - 多行代码预测（Kevin Whinnery, OpenAI 工程师评价）
   - 自然语言转代码（需配合 Composer 工具使用，详见[文档](https://docs.cursor.com/composer))

---

2. **代码库感知能力**
   - 自动分析项目上下文（Alex MacCaw 案例）
   - 支持 @ 符号引用代码片段
   - 文档智能索引（最大 10k 文件限制[1](https://docs.cursor.com/faq#are-there-size-limitations-for-project-indexing)）

---

3. **专业级代码质量**
   - 生产环境就绪的代码生成（对比 Windsurf 测试结果[1](https://www.thepromptwarrior.com/p/windsurf-vs-cursor-which-ai-coding-app-is-better)）
   - 支持复杂系统开发（支付集成/认证等场景）
   - 错误预防机制（Wes Bos 的标记一致性案例）

---

4. **开发者生态兼容**
   - 无缝迁移 VS Code 配置（主题/插件/快捷键）
   - 多语言支持（Python/JS/Java/Kotlin 等）
   - 终端集成能力（Zeke Sikelianos 评价）

---

5. **隐私与安全**
   - SOC 2 认证（[官网](https://www.cursor.com/)）
   - 本地隐私模式
   - 企业级访问控制

---

#### 差异化价值
- **生产力提升**：Instacart 团队实测 2x 效率提升（Ben Bernard 反馈）
- **智能演进**：每两周发布重要更新（Morgan McGuire 评价）
- **专业适配**：已被 OpenAI/Vercel/Figma 等顶尖技术团队采用
- **学习曲线**：平衡易用性与专业性（对比 Windsurf 更适合生产环境[1](https://www.thepromptwarrior.com/p/windsurf-vs-cursor-which-ai-coding-app-is-better)）

---

#### 用户证言
> "Cursor 的自动补全有时神奇到超越现实，约 25% 的时间它能准确预测我的编码意图" —— Kevin Whinnery (OpenAI)
>
> "这是我近年来最大的工作流改进" —— Sawyer Hood (Figma)
>
> "现在无法想象脱离 Cursor 写代码，它给了我编辑器里的 AI 超能力" —— Zeke Sikelianos (Replicate)

---

## 为什么要做一次 Cursor 分享

![Pasted image 20250218200530.png](Pasted%20image%2020250218200530.png)

Note: 从 Github Copilot 转变到 Cursor 之后，开发的模式有了很大的改变。

我自己也是经历了很长一段时间的适应和学习才逐渐习惯了使用 Cursor 目前的开发方式。

虽然不一定是最好的，但我感觉自己在这个过程中走了不少弯路。把这个过程记录和分享出来，可能会对大家有所帮助。

---

## 2. 快速入门

Notes:
小互动：有没有同学主力用 VS Code 进行开发？
有没有什么好用的插件可以推荐一下？

---

### 2.1 安装与配置
- 从 VS Code 迁移
- 基础配置说明
- 快捷键设置与冲突处理
  - Intellij-idea-keybindings 配置
  - 常见快捷键冲突解决

---

> Cursor is a fork of VS Code.

Note: 所以使用体验和 VS Code 非常相似。在 VS Code 中使用的 Extensions 在 Cursor 中同样适用。

首次打开 Cursor 时，可以导入 VS Code 的配置。

或者在右上角打开设置页，手动导入 VS Code 的配置。

这让 Cursor 的切换体验变的非常丝滑。

但是对于客户端开发来说，我们更习惯使用 IDEA 系列， 或者 XCode。常年形成的快捷键肌肉记忆，让我们刚切换到 Cursor 时浑身难受。

这就不得不夸一下 Extensions 的强大和 VS Code 的高度可定制性。

---

我们可以在 VS Code 中安装 Intellij-idea-keybindings 插件，来使用 IDEA 的快捷键。

但是使用习惯上可能还是会有一些冲突。

---

- CMD + L
  - 避免 Conflict with Cursor Chat


Notes: 比如 CMD + L 在 Cursor 中是打开 Chat，在 Intellij-idea-keybindings 中是跳转到行。

这时候我们可以通过 Command + Shift + P 打开命令面板，搜索 Open Keyboard Short Cuts，然后选择 JSON 模式，添加以下配置：

---

```json
[
    {
        "key": "cmd+l",
        "command": "-workbench.action.gotoLine"
    }
]
```

---

- Command + Shift + P
- Open Keyboard Short Cuts
- Right Click Command + L
- remove

Notes: 习惯图形化操作的可以这样操作

---

移除其他快捷键也类似

比如:

Vim: Ctrl + D

Idea: Shift + Shift

Notes:
安装了 Vim 插件后， Ctrl + D 在 Vim 和 Intellij 快捷键之间也存在冲突

我们习惯使用中文输入，但是 Cursor 有很多操作是英文符号来触发的。从中文输入法切换到英文输入法之后马上按 SHIFT + 2 来输入 @ 符号的话，会触发 idea 中的快捷键。

大家可以根据自己的使用习惯慢慢调整。

---

### 投票统计

1. 目前有多少人在使用 Cursor？
2. 大家使用 Cursor 的方式？
   - 疯狂 Tab
   - 尝试使用 Chat 功能
   - 尝试使用 Composer 功能

---

### 2.2 核心功能介绍

- Tab
- Chat
- Agent
- Context
- Models

Notes: 那么这么多的新概念，我们怎么快速上手呢？

---

[Read The Fucking Manual](https://docs.cursor.com/getting-started/overview)

Note: 相对于不爱写文档的程序员们，Cursor 提供了非常详细的文档。更新非常及时。这和我使用 Cursor 开发代码时的体验非常吻合。文档嘛，自己不爱写，但是如果有人帮我写好了，我肯定是乐意接受的。

接下来我们就按照 Cursor 的介绍文档走马观花的整体过一遍。

---

#### Tab

- Tab：智能代码补全
  - 上下文感知补全
  - 多行代码预测
  - 自然语言描述转代码
  - 补全建议的接受/拒绝策略

> AI-powered code autocomplete that suggests edits and multi-line changes based on your recent work

Note: Powered by a custom model, Cursor Tab can:

- Suggest edits around your cursor, not just insertions of additional code.
- Modify multiple lines at once.
- Make suggestions based on your recent changes and linter errors.

常见的使用体验就是不断的 Tab

提到 lint，这是一个语言相关的特性，那么 Cursor 归根结底是一个语言无关的编辑器。这就不得不提到一个概念，Language Server Protocol。

---

#### Language Server Protocol (LSP)

1. **跨语言智能基石**
   - 微软提出的开放协议（2016）
   - 解耦语言服务与编辑器
   - 支持 50+ 编程语言

Note: 这里可以举个具体例子：比如在 Python 中自动补全 import 时，背后是 pyls 在工作；在 TypeScript 中则是 tsserver

小互动：有没有同学在之前解除过这个玩意？都是什么场景下接触到的？

---

2. **Cursor 的 LSP 实践**
   - Lint Error 感知
   
Notes: 目前明确已知的一个功能是 Cursor 的 Composer 和 Agent 可以感知到 Lint Error，鼠标在 Lint Error 上悬浮的时候也可以提示把错误发送给 AI 来出解决方案。
生成代码的过程中是否会读取 LSP 提供的上下文信息目前还不太清楚，大家玩的时候可以尝试关注一下相关的日志。如果可以的话，应该也会对生成代码的准确度有很大的提升。
可以演示一下 Cursor 对 Markdown Lint 的感知。
对于我们来说，引入 LSP 只是一个让编程体验无缝迁移的尝试。
前端使用 VS Code 开发的体验相对来说会更顺畅一些。
然而，对客户端来说结果是不太乐观的。

---

#### JSON-RPC 通信细节

1. **基础消息结构**
   ```json
   // 请求示例
   {
     "jsonrpc": "2.0",
     "id": 42,
     "method": "textDocument/completion",
     "params": {
       "textDocument": { "uri": "file:///src/main.py" },
       "position": { "line": 10, "character": 5 }
     }
   }

   // 响应示例
   {
     "jsonrpc": "2.0",
     "id": 42,
     "result": [{
       "label": "print",
       "kind": 3, // 函数类型
       "detail": "Built-in function"
     }]
   }
   ```

---

2. **核心方法类型**

   | 方法类型                  | 用途                     |
   | ------------------------- | ------------------------ |
   | `textDocument/didOpen`    | 文件打开通知             |
   | `textDocument/completion` | 代码补全请求             |
   | `workspace/symbol`        | 项目符号搜索             |
   | `$/progress`              | 进度通知                |

---

#### LSP for Kotlin & Java & OC & Swift

Note: 经过实际使用，Cursor 对 Java 系的 LSP 支持相当一般。
初始化很慢，Index 不全，会有非常多的代码报错。
因为 Android 开发环境可能会使用多个不同版本的 Java 环境，LSP 会尝试和 Gradle 进行交互，在多个 Java 版本的复杂场景下，可能会启动多个 Gradle Daemon，占用 CPU 和内存。
我也偶然间观察到几次 Gradle Daemon 占满内存，无法自动退出，打满 CPU 的情况。
所以整体上来说，不太建议目前给 Cursor 搭配 LSP 进行开发。
目前主要是给大家演示一个可能的优化开发体验的路径，大家可以自行探索。

clangd 目前没有遇到什么消耗性能的情况。
swift 我的配置好像有点问题，一直没搞通。

总之，我现在的开发体验是，在 Cursor 完成代码改动之后，切换到 Android Studio 进行 import 增删，运行验证。

---

#### Others

除了直接按 Tab 全局接受 Cursor 的补全建议，还有一种精细化接受补全的方式。

`Ctrl/⌘ →` 可以按照单词选择补全的范围。

---

#### Tab in Peek

[features](https://docs.cursor.com/tab/advanced-features#tab-in-peek)

Note: LSP 还有一个功能是导航到函数定义。在 Vim 模式下，一般用 `gd` 来触发。
如果我们要修改某个函数的参数，修改完函数定义之后，按 `gd`，Cursor 会展示所有引用这个函数的地方，这个时候，Cursor 会尝试预测并修改所有函数调用，可以使用 Tab 来接受修改。但是这个方法，我们结合刚才学过的知识就知道，依赖 LSP 给出函数调用的位置，如果没有 LSP 支持，这个功能也就无从谈起了。

---

#### Tab in Comments

Note: 还有一个细节是当我们在手动补全注释的时候，Cursor 也会预测我们接下来可能要写的内容。

But

及其不准确。可以关掉，否则……

---

![Pasted image 20250221210444.png](Pasted%20image%2020250221210444.png)


---

#### Chat & Composer

Note: 这个才是今天要讲的重头戏。我们先来看看这个功能的位置。

---

- Chat：上下文感知的 AI 助手
  - 三种使用场景
    - 普通对话：一般性编程问题
    - 文件聚焦：针对当前文件的修改
    - 代码库范围：跨文件的全局理解
  - 提示词最佳实践
  - 上下文管理技巧

Notes: 先介绍三种使用场景
后面我们会介绍提示词和上下文相关的问题

---

- Composer
  - Normal
    - Read and writes code. Usually faster. Works with all models
  - Agent
    - 进行深度推理
    - 支持多步骤问题解决
    - CheckPoints
    - 可调用外部 API

---

### Chat 功能详解

1. **基础交互**
   - 三种激活方式：
     - `⌘/Ctrl + L` 打开 AI 面板
     - 行内悬浮的 AI 修复按钮
     - `⌘/Ctrl + Shift + E` 快速修复快捷键
	 
Notes: 可以打开一个项目进行具体的演示
小互动：大家平常都在什么场景使用 Chat 功能？

---

1. **上下文管理**
   - 默认包含当前文件（显示为文件标签(pill)）
   - 动态添加上下文：
     - @ 符号引用代码片段（支持文件/函数/类级引用）
	 
Notes: 介绍一下上下文
大型语言模型（LLM）的实质是一个通过海量文本训练出的"概率预测器"。其核心能力是：根据已出现的文字（上下文），计算下一个最可能出现的字词，通过反复执行这个过程生成连贯文本
**上下文**在LLM中指模型处理当前任务时需要考虑的先前信息

计算资源瓶颈
GPU显存容量决定了可缓存的上下文长度
训练数据偏差：模型在训练时接触的长文本较少，对超长上下文的处理能力较弱

所以上下文的长度会有限制，超出限制后，LLM 会遗忘最早的会话内容，有可能开始胡编乱造。

---

2. **核心能力**
   - codebase 理解
   - 即时应用代码块（点击 Apply 按钮）
   - 历史对话管理：
     - 可修改对话标题（笔形图标）
     - 支持中断和重新生成
	 
Notes: 当发现 AI 响应的内容已经开始偏离预期的时候，可以点 Stop 停止生成。也可以直接发送新的要求，中断对话。AI 会按照新的要求开始生成内容。
小互动：有人使用过 codebase 的能力吗？通常是什么场景会用？

---

## 2.3 Codebase 工作原理

---

### 索引机制（Indexing）

Note: CodeBase 功能通过智能代码索引和语义分析实现代码库深度理解

---

#### 嵌入搜索（Embeddings Search)

通过代码片段向量化建立语义索引，可精准匹配自然语言查询与代码逻辑关系

---

1. **优势对比**
   - **传统 IDE 搜索**：
     - 仅支持文本匹配
     - 无法理解代码语义
     - 跨文件关联困难
   - **Cursor 搜索**：
     - 突破单纯关键字匹配，实现语义级代码关联
     - 支持实时索引更新，适应敏捷开发需求
     - 可直接用自然语言提问
     - 支持 "类似功能实现" 搜索

---

2. **一些微操**

   - chat 模式下，ctrl + enter 发起基于 codebase 的对话
   - 如果需要更精细的策略调整，可以手动 @codebase，有一些参数可以调整
   - 在 chat 模式下，不会自动把生成的代码插入到当前的文件，需要手动点击 code block 的 apply 按钮
     - 虽然不知道底层的具体实现，但是看起来是让 AI 生成一整段代码，然后调用编辑器的 diff 能力让用户手动选择是否接受更改

Note: 我们可以先看看 Codebase 的位置。
可以关注一下这里的 Index 状态。
如果还未完成 Index 的情况下发 @codebase，Cursor 会先尝试用关键字直接搜索相关代码来作为上下文。
当然，为了搜索结果的准确，还是建议等待 Index 完成之后再发起会话。

---

## 2.4 Composer 深度应用

---

### 最佳实践

---

- 提示词工程：

     ```kotlin
     // Bad: 帮我写个网络请求
     // Good: 为 Retrofit 实现带以下特性的 API Client：
     // - 使用 Kotlin Coroutine
     // - 支持 JWT(Json Web Token) 自动刷新
     // - 统一的错误处理
     ```

Notes:
目前提示词工程还是非常重要的，大家之前可能也略有耳闻，跟 AI 保证完成任务之后给它钱，会提升 AI 生成的内容质量。
我们不会研究这种黑科技，但是对于需求的描述还是要保证精准，这样才能确保生成的代码质量是符合要求的。
虽然未来工具会进化，但精准表达需求的能力将始终是开发者的核心竞争优势

---

- 长会话
  - 尽量保持长会话，保持 AI 的短期记忆，防止重复声明注意事项
  - 不要维持太长的会话。LLM 能处理的信息有限制，过长的会话会导致信息丢失。
  - Cursor summarize earlier messages with smaller models like cursor-small and gpt-4o-mini to keep responses fast and relevant.
  - 必要时，可以 `@summarized composers` 保持记忆 (新功能)
  - 善用 cursor rule

---

- 上下文优化技巧：
  - 优先引用接口而非实现
  - 保持单次对话聚焦
  - 使用代码片段而非整个文件

---

- 任务拆解
  - AI 擅长解决的是明确的小问题
  - 如果问题比较复杂，可以分步进行
  - 确保代码质量，用 AI 生成单元测试

---

- 及时提交代码
  - 让 AI 来生成提交信息

Note: 虽然有 CheckPoints，但 Cursor 有时候的撤销操作还是比较迷惑的。Vim 模式下不小心按一个 u，就把所有改动都撤销了。建议大家把分阶段的完成结果及时提交到 git，防止代码丢失。

---

- AI Generated commit message
  - 可以尝试使用 `@commit` 来生成 commit message，甚至是 Review （没啥卵用）
  - 让 AI 解释某个 Commit 的目的和逻辑改动还是比较香的，review 不写 PR Summary 的代码时快速跟上思路有奇效
  - 可以尝试使用 `@pr` 来生成 PR 描述
  - 有一个方便的入口

Note: 
AI 辅助提交代码是一个很香的功能，大家都应该多用用。
有两种方式，一种是让 AI 帮忙生成 commit message
另一种是 Agent 模式直接让 AI 生成带 commit message 的 git commit 命令

AI Review 代码由于缺少太多上下文，基本上没有发现出什么有用的问题。期待有更进一步的玩法，比如可以尝试关联 codebase 和 PRD 内容来供 AI 分析。

---

![Pasted image 20250222163844.png](Pasted%20image%2020250222163844.png)

---

> The generated commit message will be based on the changes in your staged files and your repository’s git history. This means Cursor will analyze both your current changes and previous commit messages to generate a contextually appropriate message. Cursor learns from your commit history, which means if you use conventions like [Conventional Commits](https://www.conventionalcommits.org/), the generated messages will follow the same pattern.

---

- 代码审查流程
  - 自己先 review 改动
    - 逐项清点，防止生成的代码夹带私货
    - 防止 AI 删除代码注释
    - 防止 AI 改变标点符号的格式

Notes: 这里的代码审查是指我们自己来 Review AI 生成的改动。一定要细致。
俗话说，眼过千遍不如手过一遍。在使用 AI 生成代码的时候非常容易忽略一些边边角角的逻辑。甚至可能出现这种情况：
第二天看到这段代码写的不行，准备开喷的时候，愕然发现是作者是自己。

我们目前的经验是，对于工作年限比较少的同学来说，一定要多参与代码 Review，提升对古法纯手工打磨的代码的品鉴力，多看，多学，才能分辨出来 AI 生成的代码到底是不是高质量的代码。
千万不能成为 AI 的执行器，把报错信息粘过来粘过去，完全依赖 AI 解决问题。

目光放长远一些的话，将来我们可能不再需要手工打造代码，不需要关注具体的实现细节。像流浪地球一样直接让 AI 生成一整套解决方案就可以了。
在现阶段，独立思考能力还是非常重要的，也是区分程序员水平高低的重要标准。

---

### Be careful

小心背刺
![Pasted image 20250114202504.png](Pasted%20image%2020250114202504.png)

Note: 如果保持长对话，上下文超出限制后，AI 可能会开始胡言乱语。
把任务拆小，小步迭代，每一步的改动都容易检查，不容易出错。

---

### 常见问题处理

- 上下文丢失：通过 @ 重新引用关键类。大部分情况下我会选择重开会话。
- 生成代码风格不符：添加 .cursorrules 约束
- 复杂需求分解：新建进度跟进文档，逐步实现

Notes: 进度跟进文档我们后面会给出例子

---

### FYI

CodeBase 和 Chat History 都是存储在本地的，无法跨设备访问。

Just in case 有同学有跨设备工作的需求。

不是所有 LLM 都支持 Agent 模式。

---

## Command + K

- Inline Generation
- Inline Edits
- Run in Terminal

---

Default Context 和 Chat 有点区别。

> By default, Cursor will try to find different kinds of useful information to improve code generation, in addition to the manual @ symbols you include.
> Additional context may include related files, recently viewed files, and more. After gathering, Cursor ranks the context items by relevance to your edit/generation and keeps the top items in context for the large language model.

Note: 至于 Run in Terminal，我个人不习惯用 IDE 中提供的 UI，总感觉展示面积不够，所以用的不多。有感兴趣的同学获取可以试试 Warp。我试用过一段时间，发现和 tmux 有点冲突，也没有更深入了解。

---

>What's the difference between Agent and Composer?
>You can toggle between Normal and Agent mode in Composer. The main difference is that Agent will think harder, use reasoning and tools to solve problems thrown at it. Normal composer doesn't have access to tools, only managing code.

---

## NotePads

- Beta 功能
- 提供额外信息供 Cursor 引用

---

可能使用的场景

- 代码模板
- 项目文档，比如 PRD
- 设计文档
- 代码实施进度跟踪
  - DNSPod SDK 接入案例

---

[官方的使用指南](https://docs.cursor.com/beta/notepads)

Notes: 官方使用指南提供了一些常见使用场景和避免使用的场景，供大家参考。
顺便提一句，如果是想跨项目引用代码，不建议用 NotePads，可以尝试把两个项目放到同一个文件夹下，然后用 cursor 打开父文件夹。
手动粘来粘去还是容易出问题并且比较麻烦，没有什么比直接 @ 更方便的了。

---

## 3. 高效使用策略

---

### 3.1 AI 规则配置

#### Project Rules

Stored in `.cursor/rules`

---

需要包含的内容

```txt
Semantic Descriptions: Each rule can include a description of when it should be applied
File Pattern Matching: Use glob patterns to specify which files/folders the rule applies to
Automatic Attachment: Rules can be automatically included when matching files are referenced
Reference files: Use @file in your project rules to include them as context when the rule is applied.
```

---

.cursorrules

老版本的 Cursor 会从这里读取 project rules，已经被废弃了

---

#### Global Rules

```.cursorrules
Prefer 生成的代码具有以下特性：
- 比较高的复用性
- 利用函数式编程的优势
- 具有比较高的可测试性
- 生成新代码时不要清除现有代码的注释
- 生成的代码有比较完备的代码注释
- 不要使用 Kotlin 的非空断言运算符 !!
- 不保留无用的 import
- 不要挪动没有改动的代码的位置

对于 Android 项目，我们有以下的约定
- textSize 统一使用 dp 作为单位，不使用 sp
- 除非字符串只是用来 IDE 预览，代码和布局文件中用到的字符串资源要放到 string 资源文件中
- private 的 LiveData 类型数据以 m 作为 prefix 来区分 public 的 LiveData

使用 git 提交代码时，请在 commit message 中详细描述改动内容。
不要使用 single-line commit message，用多个 -m 加引号来实现多行 commit message
commit message 的 summary 部分要简要精确的描述改动的主要内容
commit type 要准确反映改动的性质
生成 git 提交时，我可能对生成的代码有一些改动，需要在提交之前使用 git 命令确认一下我最终接受的代码改动内容，然后再生成提交信息
只在我明确要求提交代码的时候才进行 git 提交，我需要时间检查生成的代码
没有确认项目目录内容之前不要使用 "git add ." ，项目目录下可能有其他非预期提交的文件
生成提交信息时，不要描述修改代码的过程，直接总结改动结果

帮我 Review 代码时，请遵循下面的要求：
- 代码需要遵守上面生成代码的所有特性
- Android 项目的代码需要遵循上面 Android 项目的约定
- 请理解每个函数尝试达成的目的，检查其逻辑是否存在问题

对于测试用例的代码，我们有下面约定：
- 方法命名格式为 functionToTest_condition_expectation
- 使用 hamcrest 框架进行断言
  - 使用 shouldBe alias 代替 kotlin 的关键字 is
  - 当要校验的结果为 null 时，应该用 nullValue()
- 对于没有设置过值的 LiveData，应该直接取值进行校验。getOrAwaitValue 会因为没有赋值而超时报错

生成要执行的命令时，注意把输出重定向到 cat，避免执行需要交互的代码，会导致流程卡住。
```

---

### MCP

Model Context Protocol (MCP)

类似于 Extension 的实现，给 Cursor 提供外部调用能力，比如给 Cursor 增加生成图片的能力。

---

#### 左脚踩右脚，原地起飞

[x.com](https://x.com/msfeldstein/status/1888740587698036894)

可以使用 Cursor 帮忙写一套 MCP 的实现，给自己集成。

---

### Model 选择

[available models](https://docs.cursor.com/settings/models)

---

[fast request / slow request](https://docs.cursor.com/account/usage#fast-and-slow-requests)

---

> Context windows
>In Chat and Composer, we use a 40,000 token context window by default. For Cmd-K, we limit to around 10,000 tokens to balance TTFT and quality. Agent starts at 60,000 tokens and supports up to 120,000 tokens. For longer conversations, we automatically summarize the context to preserve token space. Note that these threshold are changed from time to time to optimize the experience

Note: 需要大家多多关注世面上的风云变幻，及时根据实际情况调整模型选择。目前已知编程质量较好且支持 Agent 模式的模型就是 Sonnet 3.5。有深度思考能力的是 deepseek r1. 都不妨一试。
就在我写 PPT 的这两天，Sonnet 3.7 也出来了。只能说学不完，真的学不完。

---

## 4. 实战案例分析

- DNSPod SDK 接入案例
- 发起 PR
- 找一个最近开发代码的 Chat History 大致看一眼
- Color Template 实现，AI 考虑到 Demo 工程中需要给不同颜色的预览提供不同的背景色。预判了我的预判。

---

Docs 能力
![Pasted image 20250116164155.png](Pasted%20image%2020250116164155.png)

Note: 看起来 Cursor 可以自动爬取相关联的页面，不需要手动添加每一个文档页面的链接。
但是现在很多网页都用 Div 来响应点击，Cursor 目前对于这种跳转是爬不到的。比如我写这个分享的时候想让 Cursor 爬一下官方文档，帮我列个大纲，搞不定。

Confluence 文档无法做 Index，有 Auth 拦着。需要想办法复制一份出来给 Cursor 做引导。

实际测试效果，一下给 AI 投放过多的文档内容，依然容易导致 AI 抓不到重点，开发过程中出现以下问题
- 添加依赖的包信息写错了
- dns 请求结果的格式处理完全是错的，即使文档提供了 demo 代码，也没能直接抄过来，需要多次沟通，一点一点把错误的代码纠正到想要的状态。

经验：
步子不要迈太大，在质量不高的代码上改来改去花费的时间甚至比自己写更多。
一定要做好任务的拆分，不要让 AI 一下搞一坨出来，改不完，真的改不完。
添加文档引用的时候，最好限定一下参考的具体内容，信息越准确越好。

---

发起 PR
![Pasted image 20250117202941.png](Pasted%20image%2020250117202941.png)

Notes: 我们可以利用 `@PR` 的能力，让 AI 来总结一下改动内容，方便 Reviewer 快速了解上下文
目前的一个限制：`@PR` 只能和 master 或者 main 分支进行 diff。
解决办法，在本地把 master 分支强制指定到 merge 的目标分支。
另一个不那么危险的办法，让 Agent 自己生成 diff 命令，读取并生成结果。
Cursor 甚至贴心的准备了 Copy 键。
也可以尝试把 TutorWorkflow 的文档贴给 Agent 教会它直接执行命令。

最近我节省脑力的一个好实践就是写完代码让 AI 帮我生成提交信息，生成分支名称，生成 PR Message。力荐！

---

![Pasted image 20250218200221.png](Pasted%20image%2020250218200221.png)


Note: 光有一个好的工具是不够的，还需要我们强迫自己改变编码习惯。
比如我经常会因为一个可能只有一两行的改动而犹豫要不要直接上手改代码，最终我还是选择使用 Agent 模式，讲清楚现在遇到的问题，希望怎么解决，让 AI 改代码，生成提交，一条龙服务。
其实有的时候就像是 AI 时代的小黄鸭调试法，沟通的过程中可能气的长结节，但有些问题在组织语言和思路的过程中可能会有更好的解决方案。

---

## 5. 总结与展望

- Cursor 的优势与局限
- AI 编程助手的发展趋势
- 持续学习和适应的重要性
- 持续关注 Cursor 的 Changelog

Note: 官方的 Changelog 可以帮我们快速了解新功能，及时调整使用策略。

---

>当 AI 把编程细节接过去，软件工程相关的知识开始变得更为重要。
>没有这些知识，一个不懂技术的人或许可以在 Cursor 帮助下写出功能简单的小应用，但一旦功能变多，复杂性增加，整个应用的稳定性会随着每一次 apply 下降，直至坍塌。

---

> 鲁迅先生说得好，Cursor 就像一盒巧克力，你永远不知道下一个是 Feature 还是 Bug。
> 因此，至少需要引入两个工具来保持稳定性。

Notes:
一个是版本管理，可以让你随时回到想要的节点。
另一个是自动化测试，可以实时验证新加入的功能没有损毁已有的功能。
幸运的是，版本管理编辑器自带界面的，自动化测试也是可以让 Cursor 写的。所以不懂技术也能用。

---

Long Conversation

> Summarize Previous Composers: When conversations grow too long, you can start a new conversation while referencing the previous one.

YOLO Mode

> With Yolo Mode, the agent can auto-run terminal commands

---

[Cursor 即将发布新版本，又有重磅改进](https://mp.weixin.qq.com/s/5tbmC_9IWDQQIH0sOxHCsQ)

---

## 参考资源

- Cursor 官方文档：https://docs.cursor.com
- 内部分享文档
