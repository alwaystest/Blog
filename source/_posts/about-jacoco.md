---
title: About JaCoCo
date: 2021-04-17 11:42:32
tags:
- JaCoCo
- Programming
---

来聊一聊 JaCoCo
<!--more-->

![AboutJaCoCo](JacocoForBlog.001.jpeg)
![AboutJaCoCo](JacocoForBlog.002.jpeg)
![AboutJaCoCo](JacocoForBlog.003.jpeg)
为代码质量保驾护航
衡量开发自测的细致程度
作为单元测试的补充
让我们有信心做下一次发布
![AboutJaCoCo](JacocoForBlog.004.jpeg)
![AboutJaCoCo](JacocoForBlog.005.jpeg)
![AboutJaCoCo](JacocoForBlog.006.jpeg)
避免重新发明轮子
![AboutJaCoCo](JacocoForBlog.007.jpeg)
在这么多轮子里面，我们选择 JaCoCo 来衡量测试覆盖率。
维护活跃，用户广泛，易于使用。
![AboutJaCoCo](JacocoForBlog.008.jpeg)
了解一下 JaCoCo 的工作原理。可以让我们更好的使用它。
![AboutJaCoCo](JacocoForBlog.009.jpeg)
根据 JaCoCo 提供的文档，Java 的覆盖率探测手段基本分为这几个方向。
我们从左往右依次来看一下。
![AboutJaCoCo](JacocoForBlog.010.jpeg)
首先是运行时分析机制。
JVMTI 是 Java Virtual Machine Tools Interface 的缩写。主要目的是给开发者提供控制 JVM 虚拟机状态的工具。像 Debugger 和 Profiler 都可以通过这个接口来实现。
上一个页面还有一个 JVMPI 的东西，看 wikipedia 的介绍，JVMPI 和 JVMDI 已经被 JVMTI 代替了。
感觉客户端开发时很少接触到这些东西，具体就不在这里过多介绍了，感兴趣的同学可以找找相关资料。
值得一提的是，JVMTI 是 JVM 暴露的 Native 编程接口。
![AboutJaCoCo](JacocoForBlog.011.jpeg)
接下来看看右面高亮的第一个
![AboutJaCoCo](JacocoForBlog.012.jpeg)
![AboutJaCoCo](JacocoForBlog.013.jpeg)
![AboutJaCoCo](JacocoForBlog.014.jpeg)
查了资料发现，JDK 中也有相关的概念，我们来看看介绍。
Instrumentation 是在方法中添加的用来收集数据的字节码。
这个操作只是增加一些收集数据的操作，并不会影响程序的状态或者行为。经常会被用来做 monitor，profiler，覆盖率分析，打 log。
那么怎么给这个词一个合理的翻译呢？
![AboutJaCoCo](JacocoForBlog.015.jpeg)
![AboutJaCoCo](JacocoForBlog.016.jpeg)
顺便查找到了 Android 的一个文档页面，可以对比一下 Google 给出的中文翻译。
![AboutJaCoCo](JacocoForBlog.017.jpeg)
理解了 Instrumentation 的含义之后，继续往下看。
插桩这个操作，宽泛一点理解，可以不仅仅用在字节码层面，也可以在源码级别进行插桩处理。
想一下基于 JVM 的语言有那么多，Java，Kotlin，Groovy 等等，每个源码都实现一套插桩逻辑是不是很费事，想要在 JVM 上运行，绕不过 JVM 讲的通用语言，字节码，那对字节码进行插桩操作，就可以起到四两拨千斤的作用，一个工具就可以在多个语言的场景中使用。
继续往下看，对字节码插桩，又分为两个方向。我们先看看 On-The-Fly。
![AboutJaCoCo](JacocoForBlog.018.jpeg)
再次祭出 Google 大法。
可以看到在计算机行业，on the fly 的意思是在程序运行期间，并且不会中断程序运行的一种状态。
再继续往下看，on the fly 又被分为两个方向， ClassLoader 和 JavaAgents。
![AboutJaCoCo](JacocoForBlog.019.jpeg)
先说 ClassLoader。
熟悉 JVM 的同学应该可以想到，一段代码在运行之前，首先会被 JVM 通过 ClassLoader 加载。字节码本质上就是一堆二进制的数据，如果我们在类加载的阶段，通过定制的 ClassLoader 来加载字节码，对原始字节码进行修改，就可以实现运行过程中的插桩。
![AboutJaCoCo](JacocoForBlog.020.jpeg)
JavaAgent 也是一个 JVM 提供的机制，借助 JVMTI，在 JVM 上运行的一个特殊的 Jar 包，可以实现在运行时对字节码插桩的的功能。
![AboutJaCoCo](JacocoForBlog.021.jpeg)
来看看 JaCoCo 对自己的 JaCoCoAgent 的介绍吧。
使用方式如图，可以用 -javaagent 参数来指定 JVM 启动时加载的 JavaAgent。
![AboutJaCoCo](JacocoForBlog.022.jpeg)
JavaAgent 还有一种动态加载的方式，类似于 Android Attach 方式的 Debug 一样，在 JVM 运行过程中把 JavaAgent 动态加载起来。
不做过多介绍，感兴趣的同学可以到后面附录找相关文档了解。
https://www.throwable.club/2019/06/29/java-understand-instrument-first/
![AboutJaCoCo](JacocoForBlog.023.jpeg)
我们先来试一试。
找了一段之前刷 LeetCode 的代码，逻辑不算太简单，也不是很复杂，可以方便的观察到字节码前后的变化。
![AboutJaCoCo](JacocoForBlog.024.jpeg)
这里直接给截图吧，免得现场演示翻车🐶️
先用 kotlinc 把源码编译打包成 Jar 包的形式。
那个 include-runtime 会把 Kotlin 的运行时依赖一起打包进来，毕竟 JRE 不会带那些东西。
然后直接用 java 来运行这个 Jar 包。同时添加 javaagent 参数，默认什么都不配置的话，Jacoco Agent 会在运行结束的时候自动把收集到的覆盖率数据导出保存为 jacoco.exec 文件。
同时 JaCoCo 还提供了一个命令行工具，可以用覆盖率数据生成界面友好的网页。
来看看生成的报告吧。
![AboutJaCoCo](JacocoForBlog.025.jpeg)
这个网页版的报告列出了各种角度的覆盖率数据，没有执行到的指令，没有执行到的分支，圈复杂度，代码行数，方法数，类的个数。
这里带上了 Kotlin 的运行时环境，大部分都没有调用到。直接看运行的目标代码吧。
![AboutJaCoCo](JacocoForBlog.026.jpeg)
在生成覆盖率报告的时候，指定了源码的位置，报告中就可以把不同覆盖率的代码用不同的颜色标识出来。具体的颜色解读规则是这样的。
![AboutJaCoCo](JacocoForBlog.027.jpeg)
![AboutJaCoCo](JacocoForBlog.028.jpeg)
看起来这波操作是挺简单的，那 JaCoCo 都干了些什么事？
主要有下面几点。
看看 JaCoCo 对字节码做了什么改动
![AboutJaCoCo](JacocoForBlog.029.jpeg)
这个是 JaCoCo 官方提供的一个简单的样例。用 Java 写的。
最左面是源码，右面这幅图展示了这个方法编译成字节码之后，对应的一些操作，以及经过插桩之后，字节码变成了什么样子。
可以看到， JaCoCo 对字节码做的改动并不多，对这段代码来说，只有三个改动点，高亮展示出来了。看看这个 P 是什么东西。
![AboutJaCoCo](JacocoForBlog.030.jpeg)
P 代表 Probe，是探针的意思。
直接用 JaCoCo 官网的介绍，探针是在已有的指令中插进去的指令，这些指令不会改变原有的代码执行逻辑，只是起到记录哪些指令被执行的作用。理论上讲，可以在每条指令后面都插一条探针，这样就可以分辨出来哪些指令是执行过的，哪些是没有执行过的。但是实现探针的功能，需要用多条指令，如果每条指令后面都添加一个探针，会显著的增大 class 文件的大小，并且在运行时探针命令也是有开销的，添加过多，也会减慢程序正常运行的速度。
其实并不需要做这么多插桩操作。JaCoCo 的术语叫 Control Flow Analysis。只需要在控制流程的边界点插入探针，就可以实现目的。举个例子，如果一个方法，没有任何的分支，那只需要一个探针，就可以知道这个方法里面的所有指令都被执行了，或者都没有被执行。比如在右图，高亮的位置，第一个探针如果收集到了被执行的数据，就说明这个探针之前的那些指令都被执行覆盖过了。
![AboutJaCoCo](JacocoForBlog.031.jpeg)
JaCoCo 对探针是这么介绍的：
记录指令是否被执行
不同位置的探针，记录的数据需要区分开，否则无法知道到底是哪里被执行过了
线程安全
没有副作用
需要尽可能小的运行时开销
![AboutJaCoCo](JacocoForBlog.032.jpeg)
JaCoCo 对每个类都对应了一个 bool 值的数组，探针在这里存储对应的数据。
怎么做到一个 bool 数组就能存储这么多信息呢？刚才看到的覆盖率报告中命名有类名，方法名，分支等等很多数据。
先看一下 JaCoCo 导出的覆盖率结果文件
![AboutJaCoCo](JacocoForBlog.033.jpeg)
看一下 exec 文件的结构
正确的打开方式是用命令行执行 execinfo 命令，直接用文本方式打开会乱码
这里可以看到只有指针的数量，类名，session，class id 信息
![AboutJaCoCo](JacocoForBlog.034.jpeg)
再看一下生成报告的命令
注意一下唯一的一个 required 选项，是 class 文件
sourcefiles 是可选的，如果给了源码位置，在生成报告的时候就会去关联源码，给出对应的高亮样式，便于阅读。
JaCoCo 会从 exec 文件中获取探针收集到的信息，哪些探针所在的位置被执行过了，哪些没有。exec 文件中并没有分析覆盖率需要的方法名或者行数的信息，这些信息是 class 文件来提供的。只要拿到插桩时候的那些 class 文件，再次做个一模一样的控制流程分析，就可以找到每个探针在指令中对应的位置。class 文件中可以保留行号的信息，通过这个信息，就可以计算出源码中的行覆盖率了。
这里在提一下 execinfo 中的 class id 信息。因为要对 class 文件进行控制流程分析才能获取探针正确的对应位置，如果 class 信息变了，那所有的分析就都没有意义了。所以探针需要记录下来插桩的时候对应的 class 文件的 id，只要 class 文件发生变化，这个 id 就应该发生变化。那这个 id 就需要一个散列算法来对类文件进行计算。JaCoCo 用的是 CRC64 算法。
![AboutJaCoCo](JacocoForBlog.035.jpeg)
回来看看，我们刚才谈了半天 JavaAgent，ClassLoader。都需要我们对运行时环境有控制能力。
Android 的运行环境受限，只能另寻它路。
![AboutJaCoCo](JacocoForBlog.036.jpeg)
让 JaCoCo 在 Android 上运行，我们面临这样的问题。
![AboutJaCoCo](JacocoForBlog.037.jpeg)
JaCoCo 提供了离线插桩的方式，具体的使用方式如图。
会对现有的 class 文件直接做修改，修改方式和 JavaAgent 做的一样。只不过是从运行时提前到了运行前。同时，JaCoCo 提供了一个运行时依赖，可以通过这个依赖把已经收集到的探针数据导出。在 Android App 运行的过程中，可以选择导出的时机，如果等到系统杀 App 的时候才进行导出，很可能来不及做持久化的操作。比如可以在发生页面切换的时候进行导出，或者 App 发生前后台切换的时候进行导出。后面要做的就很简单啦，还是用命令行用导出的数据和 class 文件就可以生成漂亮的报告了。
![AboutJaCoCo](JacocoForBlog.038.jpeg)
Android 端的 AGP 提供了覆盖率收集的功能，底层也是依赖 JaCoCo 来实现的。
这个开关是 BuildType 的一个属性，设置为 true，打包的时候就会自动给 APK 做离线插桩操作。并且会将 JaCoCo 的运行时依赖添加到 APK 中。为了防止类冲突，JaCoCo 有一个比较 Hack 的做法，会把运行时的依赖放到一个随机命名的包里面，只暴露一个入口类，而且因为这个依赖是插桩的时候才会添加，并不是我们声明的依赖，所以一般我们会用反射来调用 JaCoCo 的入口，避免编译时检查找不到类导致编译失败。
对于 Android 来说，只需要开启一个配置，AGP 就帮我们把所有事情都做完了。但是对于使用命令行来做插桩的 jar 包，运行的时候还是需要 JaCoCoAgent 在 classpath 中，否则会报错 ClassNotFound。
还有一个坑是目前 AGP 的实现中，只要开启了测试覆盖率，就会自动将 BuildType 的 debug 设置为 true，如果在 Release 环境开启插桩，需要注意一下影响。
来看一下离线插桩后的 class 文件吧。
![AboutJaCoCo](JacocoForBlog.039.jpeg)
先看看入口函数 main 方法。
方法刚进来，就先执行了一个 jacocoInit，对 class 对应的 boolean 数组赋值。
那个 Offline 就是 JaCoCo Runtime 中的类。
初始化之后，就可以看到在指令中间插入了对 jaCoCo 探针数组修改的指令，数组中每个位置，都代表了一个探针。可以看到在 main 方法中，有两个控制流程边界。
因为 main 方法定义的靠后，所以这里的探针数组赋值不是从 0 开始的。看看剩余部分。
![AboutJaCoCo](JacocoForBlog.040.jpeg)
这里就可以看到，探针数组从 0 开始赋值了。
![AboutJaCoCo](JacocoForBlog.041.jpeg)
再看一下 exec 文件的内容，和这里的探针数组长度。
可以看到，对这个类，JaCoCo 插了 14 个探针，对应的，对探针数组操作的下标最大值是 13，在 main 方法结束的时候。
![AboutJaCoCo](JacocoForBlog.042.jpeg)
到这里，JaCoCo 的工作原理就说完了，其实大部分都是翻译了官网的文档。当时看的时候感觉一头雾水，看的晕头转向的，所以希望这里按照顺序讲解完之后，能对大家理解 JaCoCo 的工作原理有帮助。
当然，我的介绍非常简单，细节才是魔鬼，这里为了方便大家理解，我的水平也有限，省略了很多细节的部分，大家感兴趣的话可以去翻官网的文档。我自己感觉看完 JaCoCo 的实现原理和设计思路后，学习到很多东西。
![AboutJaCoCo](JacocoForBlog.043.jpeg)
顺便给去年我的分享做个续集。
大家开发的时候，如果需要用 Charles 看 API 的情况，需要进入设置，打开 WIFI 设置，等等一连串的操作，才能给当前的 WIFI 设置上 HTTP 代理。实在是太复杂了。
我去年的时候就尝试着用代理直接重定向来实现简化设置步骤。但是实现的思路有点问题。HTTPS 的请求没搞定。
前段时间又去想这个事情，才发现自己绕了弯子。Charles 开放局域网监听后，本身就可以被看作是一个 HTTP 方式提供的 Proxy Server。只需要把它当作上游代理服务器就好了。
![AboutJaCoCo](JacocoForBlog.044.jpeg)
目前我常用的代理软件有两个，一个是 V2ray，一个是 Clash。其实 Clash 并没有自己独有的代理协议，我就直接把它当作一个前端分流工具来使用了，主要是 Mac 端的 ClashX 好用。具体使用 V2ray 还是 Clash 都可以达到对应的效果，我猜 iOS 的 ShadowRocket 应该也能做到。
就以 Clash 为例。
![AboutJaCoCo](JacocoForBlog.045.jpeg)
关键配置很简单，提供一个 HTTP 格式的代理服务器，IP 和端口指向自己的 Charles 位置即可。
为了方便，直接启用全局代理，绕过乱七八糟的配置。重启一下要代理的 App 就行。重启是为了让 App 的网络请求建立的连接都走代理，被转发到 Charles。具体规则格式，Github 有 LazyRule 可以参考，后文贴了链接，有需要的同学可以自取。
![AboutJaCoCo](JacocoForBlog.046.jpeg)
![AboutJaCoCo](JacocoForBlog.047.jpeg)
