---
title: NewBull2020
date: 2020-07-06 00:19:33
tags: Programming
---

新牛计划是我司为应届毕业同学安排的入职培训课程，希望能够帮新同学更快的了解工作内容和熟悉开发环境。
<!--more-->
今年有幸参加这个项目，给新同学讲解 Android 开发基础知识中 View 和 Layout 相关的知识点，希望能够抛砖引玉，帮大家尽快踏上 Android 开发之旅。
本文默认读者已经提前阅读过我们提供的参考资料：《第一行代码》(第三版)
下面是本次的 Keynote 和讲稿内容的初稿，如果发现有什么问题或者可以改进的地方，烦请指出。感谢。
本次讲稿的内容也大量参考了去年新牛讲师的 Keynote，给大佬致谢。

---
大家好，今天我来给大家介绍一下 Android 开发中 View 和 Layout 的一些概念，希望能对大家之后的开发和学习有帮助。
![NewBull2020](NewBull2020.001.jpeg)
首先来看一下今天要介绍的内容。主要分为以下几点。
- View/ViewGroup 的基础知识
- 一些常用的布局
- 如何实现自定义 View
- 一些小 Tip

![NewBull2020](NewBull2020.002.jpeg)
先来看一下 View/ViewGroup 的概念
![NewBull2020](NewBull2020.003.jpeg)
从开发者官网贴个图过来。
layout 的作用是定义 UI 的结构。
UI 的结构如图所示，ViewGroup 可以认为是一个不可见的容器，其中又包含了多个 ViewGroup 和 View。这样就形成了一个树状结构，来描述当前的用户界面。
![NewBull2020](NewBull2020.004.jpeg)
从类结构上来看，ViewGroup 也是一种 View，可以将其理解为不可见的 View。
下面这些比较常见的 Layout 都是 继承自 ViewGroup，作为容器管理他们的子 View。
![NewBull2020](NewBull2020.005.jpeg)
接下来我们看一下 View 这个类中最经常使用的一些方法。
![NewBull2020](NewBull2020.006.jpeg)
首先是 setVisibility 方法，用来设置 View 的可见属性。
![NewBull2020](NewBull2020.007.jpeg)
可以使用的参数有以下三种，根据字面意义就可以很方便的理解。
![NewBull2020](NewBull2020.008.jpeg)
然后是 setEnabled 方法，设置 View 的 Enable 状态。
![NewBull2020](NewBull2020.009.jpeg)
可以来看一下效果。
Android Studio 中提供了工具可以预览各个状态下 View 的样式。
XML 在 Android 系统中还可以用来定义一幅图像。
比如我们用 Selector 标签定义了一个 Android 中的 Drawable 对象，通过给不同的状态设置不同的图像，可以实现在程序中根据当前状态展示对应样式的效果。
以这个 Drawable 对象为例，我们定义了 enable 状态下这个图像应该展示一个什么颜色，在默认条件下展示一个什么颜色。然后我们就可以在右面预览对应的效果，当前展示的是 enable 为 false 的样式。
我们来看一下 enable 为 true 的样式。
![NewBull2020](NewBull2020.010.jpeg)
现在我们在右边预览界面选择了 Enabled 状态为 true，此时就可以看到展示了一个对应效果的图像了。
那么 setEnabled 这个方法什么时候用呢？
![NewBull2020](NewBull2020.011.jpeg)
在一个 App 中，我们经常会用到按钮来让用户点击，按钮是存在状态的，当没有输入，或者输入格式不合法的情况下，我们需要给用户一个视觉上的提示，标明这个状态下按钮是不可以点击的，这个时候，我们就可以将按钮的背景色设为刚才用 Selector 声明的 Drawable。此时我们将按钮的 enable 设为 false 的时候，就会展示下面的样式。而将 enable 设为 true 的时候，就会展示上面的样式。
同理我们还能在 IDE 的预览界面看到其他状态，同样的我们也可以给那些状态设置特殊的样式，用作区分展示。比如常用的有按钮按下的时候展示不同的颜色，可选框选中的时候展示另一个样式。
![NewBull2020](NewBull2020.012.jpeg)
接下来再看另一个常用的方法，setClickable
也很明显，可以设置这个 View 是否可以被点击。当设置 clickable 为 false 的时候，点击这个 View 就不会有任何响应了。
![NewBull2020](NewBull2020.013.jpeg)
有一点需要注意的是，在 setOnClickListener 和 setOnLongClickListener 的时候，会自动将 clickable 或 longClickable 设为 true。
某些特殊场景可能会遇到这样的坑。
![NewBull2020](NewBull2020.014.jpeg)
常用的方法说完了，我们来简单看一下 View 是怎么工作的吧。
![NewBull2020](NewBull2020.015.jpeg)
大家可能注意到了，我们在使用 View 的时候，一般都是在 XML 文件中声明，设置各种属性大部分也都是可以在 XML 声明中搞定的，但是我们去看对应的 View 的实现的时候，却发现这些类是用 Java 代码来定义的。那 XML 和 Java 代码之间是什么关系呢？
简单来讲，Android 提供了一个工具类叫 LayoutInflater，在我们调用 Activity.setContentView 方法的时候，帮我们把 XML 格式的 Layout 文件解析成了对应的 View，并且根据 XML 中声明的各种属性，帮我们设置了 View 对应的属性，这么一来，帮我们简化了大量的构建 View 的代码，而通过 XML 的格式，也可以将 View 的属性和 View 之间的关系表达的清晰明了。降低了项目代码的复杂度。这里我们不详细讲，大家之后可以看看相关的代码实现。
我们的第一个方法 onFinishInflate 就是在 XML 格式的 Layout 被实例化为对应的 View 类对象后调用的，更准确一点的话，根据文档，这个方法是在 View 被 Inflate 的最后一个阶段被调用的，在所有的子 View 都被添加完之后才会被调用。这个方法我们在开发中用的不多，这里只是顺带提一下。
一般我们关注的 View 的工作流程主要是指 measure、layout、draw 这三大流程。
其中，measure 确定 View 的测量宽/高，layout 确定 View 的最终宽/高和四个顶点的位置，而 draw 则将 View 绘制到屏幕上。
而要修改这三个流程的具体操作，通常我们会选择 override onMeasure、onLayout、onDraw 这三个方法，并在其中作出具体的逻辑实现。
在 onMeasure 方法中，我们需要调用 setMeasuredDimension 来设置 View 宽/高的测量值。
在 layout 方法中，则是通过 setFrame 方法来设置 View 的 mLeft、mRight、mTop、mBottom 这四个值，相当于设定了 View 的四个顶点。而 onLayout 方法则会确定 ViewGroup 的所有子元素的位置。
draw 方法就比较简单啦，基本上就是按照层级从底层到顶层将对应的 View 内容绘制出来。

在 View 的工作流程中，会有一个小坑需要了解。在 measure 阶段，我们获取到的是 View 的测量后的大小，而 View 最终的大小是在 layout 阶段才确定的，这里需要注意区分，虽然大部分情况下 View 的测量大小和最终大小是相等的。还有一点是 View 的 measure 过程和 Activity 的声明周期方法并不是同步的，所以在 onCreate、onStart、onResume 中不一定能拿到 View 的宽/高。
那应该怎么获取 View 的宽高信息呢？
![NewBull2020](NewBull2020.016.jpeg)
这里简单提供两个常用的方法。
第一个方法利用了 Android 的消息队列机制，把测量宽高的 Runnable 放到了消息队列的尾部，Runnable 会在 View 初始化好之后被执行。
第二个方法是 Android 提供的一个接口，需要注意的是，随着 View 树的状态改变，onGlobalLayout 方法会被调用多次。
当然，我们也可以调用 View 的 measure 方法来获得 View 的测量宽/高，这种方法比较复杂，会有一定的局限性，当 View 的大小是 match_parent 的时候，测量还需要知道父 View 的大小，而这个时候我们无法知道父 View 的大小，这种情况就比较难处理。
![NewBull2020](NewBull2020.017.jpeg)
然后我们再了解一下 View 的尺寸。
![NewBull2020](NewBull2020.018.jpeg)
从官方文档，我们可以看到 View 的尺寸有很多的单位，在 Android 开发过程中，我们主要会使用到 dp 这个单位。
官方推荐设置 text_size 的时候使用 sp 这个单位，这样就可以在系统中设置文字缩放，方便不同的人群阅读。但是这个事情在我目前的实践过程中很少能被遵循，要支持这个特性，就需要兼顾测试各个文字大小下的 UI 适配。成本比较高，所以好多软件的实践都是将 text_size 也用 dp 来设置。当然，如果是要做一款用户体验好的阅读类 App，这种方式就不太合适了。
![NewBull2020](NewBull2020.019.jpeg)
关于 View 的尺寸，还有两个概念需要了解。Padding 和 Margin。
![NewBull2020](NewBull2020.020.jpeg)
我们来看这两个 View。View A 在 View B 的上面。他们都设置了 Margin 和 Padding。
可以看到，Margin 位于 View 的边框外层，View A 下边框和 View B 的上边框的距离是 MarginA + MarginB。
而 Padding 决定了 View 的内容距离边框的距离。
四个方向的 Margin 和 Padding 都可以单独设置，不需要是完全一致的。
但是需要注意的是，Padding 是 View 自己的属性，但是 Margin 却不是。Margin 是通过 ViewGroup 的 LayoutParams 来提供的。
我们可以看一下 LayoutParams 是一个什么样的状态。
![NewBull2020](NewBull2020.021.jpeg)
在 XML 的 layout 文件中，我们经常会用到 layout_xxx 这样的属性，没错，这些属性会被转换为 LayoutParams 对象，这些对象包含了 ViewGroup 在对子 View 做测量和定位时需要的属性，因为不同的 ViewGroup 类型对子 View 做测量和定位的时候可能需要不同的属性，所以我们可以看到 ViewGroup.LayoutParams 类有许多子类，每个需要新增 LayoutParams 属性的 ViewGroup 都继承了 ViewGroup.LayoutParams，并在子类中增加其需要的属性。
从图中我们可以看到，View 持有的 LayoutParams 并不是和自己相关，实际上是和 View 所在的 ViewGroup 类型相关的。这点需要注意。
在实际使用中，我们这么定义：
![NewBull2020](NewBull2020.022.jpeg)
从右边的使用也能看出来，margin 属性是和 LayoutParams 相关的。
padding 则是 View 自己的属性。
![NewBull2020](NewBull2020.023.jpeg)
接下来我们看看 View 的尺寸怎么通过代码定义。
![NewBull2020](NewBull2020.024.jpeg)
ViewGroup.LayoutParams 提供了这么一个构造函数，我们可以通过这种方式来创建一个 LayoutParams 对象，并指定其宽高。
![NewBull2020](NewBull2020.025.jpeg)
这里，我们创建了一个宽和父 View 相等，高度和 View 的内容相等的 LayoutParams 对象，并将其设置给了 View。
![NewBull2020](NewBull2020.026.jpeg)
另一种使用方式是在向 ViewGroup 中添加子 View 的同时，也可以指定这个子 View 使用的 LayoutParams。
当然，宽高这两个参数不是只能用 match_parent 和 wrap_content，还可以指定具体的值，单位是像素，这里需要注意和 dp 的转换关系。
![NewBull2020](NewBull2020.027.jpeg)
接下来我们大致看一下常用的 Layout 及其属性。
![NewBull2020](NewBull2020.028.jpeg)
首先是 FrameLayout，这种 Layout 使用的地方比较少，一般用来做 Fragment 的容器。
![NewBull2020](NewBull2020.029.jpeg)
使用 FrameLayout 的时候，最常用到的就是 layout_gravity 属性了，这个属性一看就明白了，是设置 LayoutParams 的。
具体的用法是设给 FrameLayout 的子 View，这样我们就可以控制这个子 View 在 FrameLayout 中展示的位置，比如靠左，靠右，居中等等。给大家看一个具体例子。
![NewBull2020](NewBull2020.030.jpeg)
这里我们设置了第一个 View 的 layout_gravity 属性为 start|top，没错，他们可以组合使用。效果会让这个 View 出现在左上角。
第二个 View 设置了 end|bottom，会让这个 View 出现在右下角。
![NewBull2020](NewBull2020.031.jpeg)
接下来是 LinearLayout。这个 Layout 是我们使用特别频繁的一个布局方式。
简单来讲呢，就是所有的子 View 排排坐，一个一个来。
![NewBull2020](NewBull2020.032.jpeg)
来看看具体的属性。
第一个 orientation 控制了子 View 排排坐的方式，是横着排列还是竖着排列。
layout_gravity 我们之前说过。
layout_weight 可以设置子 View 占 LinearLayout 的比例。具体的算法大家可以之后单独查找相关资料，在某些特殊场景还是比较复杂的。
![NewBull2020](NewBull2020.033.jpeg)
这里我将 LinearLayout 的 orientation 属性设为横向。
注意，我将中间的这个 View 设置了 layout_witdh 为 0dp，并且设置了 layout_weight，这样就达到了一个效果。第二个 View 的宽度占据了除了第一个和第三个 View 之外的所有宽度。
使用 layout_weight 的时候要注意，需要将对应方向的尺寸设为 0dp 才能生效。当 LinearLayout 是横向 orientation 的时候，我们可以设置宽度为 0，而 LinearLayout 是纵向 orientation 的时候，我们可以设置高度为 0。
![NewBull2020](NewBull2020.034.jpeg)
接下来也是一个比较常用的布局方式，RelativeLayout。
![NewBull2020](NewBull2020.035.jpeg)
这里常用的属性就比较多了，从名字上我们可以看出来，这是一个相对布局，布局的主要方式就是设置各个子 View 之间的关系，比如两个 View 左对齐，A 在 B 的右面等等。具体我们就不一个一个过了，上例子。
![NewBull2020](NewBull2020.036.jpeg)
这里我们定义了一个 centerView，居中展示。
然后又定义了第二个 View，规定第二个 View 在 centerView 的右面，和上面，于是就可以展示出如图的效果。
id 是 View 识别的标志。
![NewBull2020](NewBull2020.037.jpeg)
最后呢，是更加强大灵活的 ConstraintLayout。
LinearLayout 只能横排或者竖排，RelativeLayout 又提供了更加灵活的相对布局的方式。为什么又提供了 ConstraintLayout 呢？
![NewBull2020](NewBull2020.038.jpeg)
Google 是这么解释的，ConstraintLayout 和 RelativeLayout 很像，但是它可以提供更加扁平化的 View 层级，这样可以提供更好的性能，同时，Google 还提供了一套 UI 工具来帮助开发者们通过拖动操作更加方便的设置布局。
因为 ConstraintLayout 有性能好，更灵活，功能更强等优点，现在我们的 IDE 新建 Layout 文件的时候，默认的的根布局就已经被替换成 ConstraintLayout 了，这里也推荐大家在进行比较复杂的布局文件编写时，优先使用 ConstraintLayout。
这里我还有一个建议，尽量不要依赖 IDE 提供的可视化拖动布局的方式，而是要知道你的每一个操作后面发生了什么。
![NewBull2020](NewBull2020.039.jpeg)
那么怎么看呢？
![NewBull2020](NewBull2020.040.jpeg)
大家可以打开 LayoutEditor 之后，在 LayoutEditor 右上角切换到 Split 模式。
这样在拖动完 View 之后，就可以看到左面对应的 XML 中被修改了什么。
相对来说，我个人熟练了之后还是喜欢用手写 XML 的方式来编写布局文件。
![NewBull2020](NewBull2020.041.jpeg)
接下来我们看看如何简单的自定义一个 View。
Android SDK 中提供了一些基础的 View，那如果我们有一些特殊的需求，需要基于现有 View 增加一些功能，或者创造一个新的 View 类型，能不能行呢？
当然是可以的。
建议大家由易到难，先考虑能否复用或组合现有的 View，再考虑继承某一个 View，增加修改它的功能。
![NewBull2020](NewBull2020.042.jpeg)
我们来先看一个自定义导航栏的例子。
主要的实现方式是继承了一个现有的 ViewGroup 类，然后在初始化的时候调用 inflate 方法，将 layout 实例化出来填充到这个 ViewGroup 中。
这是比较简单的组合的方式。
![NewBull2020](NewBull2020.043.jpeg)
这里有一个小技巧，在新建了类文件后，声明了要继承的类，然后什么都不写，此时 IDE 会提示错误，此时我们看 IDE 提示的解决方式，选择使用 JvmOverloads 添加 View 的构造函数。就可以快速的创建出来构造函数。
![NewBull2020](NewBull2020.044.jpeg)
还有一个需要注意的地方是，我们编写的自定义 View 的 Layout 文件，建议使用 merge 标签作为 root，然后可以添加 parentTag，告诉 LayoutEditor 当前的 layout 文件会被附加到哪种 ViewGroup 中，这样就可以得到正确的预览了。
使用 merge 标签的另一个好处是降低 View 的层级，可以提高渲染的效率。
注意 merge 标签中定义的属性，在解析的时候就被丢掉了，无法被解析出来，只能在预览的时候看看效果。
![NewBull2020](NewBull2020.045.jpeg)
有些需求是不能通过组合 View 来实现的。比如需要新增一个特殊的属性，还需要支持通过 XML 属性的方式来简化配置方式，就需要通过自定义属性的方式来实现了。
![NewBull2020](NewBull2020.046.jpeg)
代码实现的属性解析是这样的。
这里我们可以看到有用到之前的构造函数中传递过来的 AttributeSet 对象 attrs，从 XML 构造 View 的时候，会将 XML 中定义的属性解析出来放到 AttributeSet 中传进来，我们就可以从中读取到定义的属性和值。
比如这里我们给自定义的 NavigationBar 定义了一个 Title 属性，类型是字符串，这样我们就可以在 XML 中声明 title 属性，而不用到代码中手动调用 findViewById 然后再设置对应的 text 了。
这里需要注意的是， TypedArray 对象在使用后需要及时调用 recycle 方法，回收对应的资源。
那 R.styleable 是怎么定义的呢？
![NewBull2020](NewBull2020.047.jpeg)
首先，我们在 res 文件夹中创建 Values Resource file，在其中添加 declare-styleable tag，在其中定义属性的名称和格式，然后就可以在 XML 中给自定义 View 设置 title 这个属性，然后在自定义 View 类中实现解析和对应的设置代码，我们的自定义 View 就可以支持自定义属性啦。
![NewBull2020](NewBull2020.048.jpeg)
对应 View 的布局和使用，有一些经验和技巧需要注意。
![NewBull2020](NewBull2020.049.jpeg)
使用 include 标签，提高 XML 文件的复用率，和我们写代码是一样的，遵循 DRY 原则，增加代码复用率。
使用 merge 标签降低 View 的层级，降低布局的复杂度，提高 View 渲染的效率。
对于一些不会马上展示出来的 View ，使用 ViewStub，做延迟加载，提高首次加载速度。
复杂的 Layout，尽量使用 ConstraintLayout 打平 View 的层级，提高渲染效率。
自定义 View 要保持简单，不要把业务逻辑耦合进去，这样才能更好的被复用。
要考虑性能，虽然现在移动设备的性能已经很高了，但是我们还是需要多关注性能方面的表现，避免复杂的逻辑造成用户层面感知到卡顿，无谓的增加耗电量。从微小的地方做起，才能保证整个应用是流畅，高效的。
![NewBull2020](NewBull2020.050.jpeg)
![NewBull2020](NewBull2020.051.jpeg)
![NewBull2020](NewBull2020.052.jpeg)
![NewBull2020](NewBull2020.053.jpeg)
