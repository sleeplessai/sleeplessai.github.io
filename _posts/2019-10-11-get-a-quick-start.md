---
layout: post
title: "Get a Quick Start"
date: 2019-10-11 20:30:27 +0800
comments: true
categories: [Road2PhD]
excerpt: "快速开始！Work Hard! Play Hard!"
---

## Be Proficient

请重视环境管理，因为 __对于环境的适应和构建是一种专业素养__。从2017年进入AI/ML/DataScience开始，一直有机会不停地重复这件事情，动手实践了很多很多次才慢慢变得熟练。新学期搬到新座位，又是一个机会，一直想写一篇文章记录“快速开始”的经验，于是择日不如撞日。本文将以事件为单位叙述个人在计算机上环境维护事项，其中包含一些长期培养起来的一些能力和观念，同时这也将作为一份备忘录，或持续更新。

## Hardware Perception

__很少有人不喜欢高性能、高颜值的计算机。__

首先，要对计算机的硬件配置足够敏感，能清晰地认识到笔记本、台式机、工作站、服务器、计算节点、路由器、开发板等不同类型计算机用于工作时的差别，即上手新硬件后通过观察和经验，可以快速想象出会如何使用这些硬件，如何将使用习惯与之相结合；而如果有一些拆卸维护的硬件的经验，或者阅读过品牌计算机的硬件维护手册，也许能体会到这些计算机的工业之美，对工作心态有正面积极的影响。

个人而言，我不会重视评测机构的跑分对比测试，也不会轻信他人的使用感受。大多数值性能上的差距并不能对工作效率产生质的影响，比如CPU是选择Core i5的8代还是9代，SSD是选择Samsung还是Intel，等等；专业地使用计算机（非硬件方向）要对计算机的硬件型号和特性有选择性地无视，因为这个时代大多数硬件带来的体验是同质化的，拘泥小节浪费时间。当然，如果条件允许，更高性能的计算机应用算力的标准去度量效率，工作站及以下带来的桌面工作体验，十分建议无视差异，善用手中的资源。

## Base on OS

__操作系统的选择，不是一个阵营站队问题，不必为了一味追求损失效率。__

在操作系统的选择上，要完全舍弃Windows很困难，特别是在游戏的诱惑下，在笔电和台式上都安装Windows 10。Linux的AI/ML环境有得天独厚的优势，举几个实际例子：无法在Windows上完全编译Gym[all]，无法编译PyTorch geometric，也无法使用libtorch CUDA版本的pre-built，Linux有全局通透的包管理器，等等，使用Linux是明智之举。至于发行版的选择，Debian、Ubuntu、openSUSE、Arch等都是（曾重度使用过）相当优秀的发行版，它们的社区和Wiki也都对应的成熟完备，可以多多尝试。

在实验室的工作站上安装了`Ubuntu 18.04.3 LTS`，并备用其他Windows 10/Server和Linux的镜像文件以制作闪存。使用Linux时，其他工作的Windows PC最大化地同步环境，以便近似无缝地衔接工作。

我对Linux命令行的依赖，跨越了整个计算机使用经验，因此Windows可以使用[MSYS2](https://www.msys2.org/)延续这种习惯，同时密切关注WSL 2的工作进展。通过MSYS2，将`C:/msys64/usr/bin`加入`PATH`，使得在cmd下使用很多Linux基本命令，同时还带来了一组GNU编译器，完成简单的C/C++开发，也为[Constellation博客](https://sleeplessai.github.io/)带来了Ruby支持。目前由于WSL 2需要通过Insider Update才能使用，稳定起见就不继续跟进预览版本了。

## Sources

安装完操作系统或包管理器之后第一件事是更换更快的软件源。

一般会选择使用[tuna萌](https://tuna.moe)，同时还推荐[Tencent云](https://mirrors.cloud.tencent.com/)和[OPSX](https://opsx.alibaba.com/mirror)，因为这些镜像服务器分布在、服务于各地的机房，一个URL自动解析到速度非常不错的镜像上。

- Ubuntu APT源参考[tuna-ubuntu](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)，编辑`/etc/apt/sources.list`进行更换，其他Linux发行版的源也可以在各大镜像站点上找到；

- MSYS2参考[tuna-msys2](https://mirrors.tuna.tsinghua.edu.cn/help/msys2/)即可；

- tuna已经[拿到了Anaconda的镜像授权](https://mirrors.tuna.tsinghua.edu.cn/news/#anaconda-restored)，参考[tuna-anaconda](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)通过编辑`~/.condarc`文件配置；

- PyPi也可以在各大镜像站点找到源，例如[tuna-pypi](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)。

## Core Environments

当下主要工作方向是（深度）神经网络和计算机视觉，编程语言使用Python和C/C++（其他的不会），重点说一下 __Python的环境__ 的管理。

现在，conda已经作为一个等同于virtualenv的工具使用，用于快速的构建Python环境，一般地情况下`(base)`做保留，定期升级conda，同时建立了一个LTS环境作为日常使用的AL/ML环境，包均通过pip进行安装，这是当下最核心的开发环境。如果有新的项目和开源仓库，工作流则是用`conda create`为项目建一个专属环境，安装所有的包，若有不完整的则用pip补齐，在这个项目中，如果有包特别青睐或者全新掌握的，会考虑将其加入到LTS环境下进行维护。每当环境重点包更新或者大变化的时候，就重新生成环境的`pip freeze`，并备份到Git仓库中。

几个月前，tuna Anaconda源被下，换回了官方源，其实速度也不错。此前喜欢添加一些第三方的channel，使得conda检查依赖（Resolving Environment）变得缓慢，所以要慎重使用。十分建议根据[Anaconda Cloud](https://anaconda.org/anaconda/repo)包的安装命令、一次性地使用channel参数进行安装，而不是将channel加入到`custom_channels`，某些第三方仓库的包有一定概率会破坏当前的依赖，`conda install`时会被告知要降级一些包、修改一些包源，对于个人稳定的核心生产力环境（例如上述LTS环境），不建议加入这样的channel。

## GPU Configuration

GPU作为算力最重要的组成部分，配置需要具体说明。

GPU驱动程序一般选择系统安装时的最新版本，并保持很长一段时间。之后Windows就采取随缘更新的策略；Ubuntu驱动程序的错误安装会瞬间宕掉一个系统，建议安装成功后保持稳定不更新的状态。使用系统提供的安装方式最为稳妥，在`Software & Updates`的`Additional Drivers`选项卡中选择最新的驱动安装即可。

![additional drivers](https://i.loli.net/2019/10/10/Iti62F8o3spUqzR.png)

CUDA Toolkit选择次新版10.0，主要是适配PyTorch和TensorFlow的需求，cuDNN library可以根据CUDA的版本选择最新的7.6.x。CUDA推荐使用`runfile(local)`的方式安装，在命令行指引下，可以方便地选择不安装驱动，使用deb需要选择meta package，稍显麻烦，如果不小心直接安装了所有包，包内的410驱动会与已经安装的430驱动冲突，重启后系统就完全崩溃。cuDNN登录开发者账户后，下载对应deb包，用dpkg直接安装即可。

![cuda10 target platform](https://i.loli.net/2019/10/11/Tgva3ZCiH7zmjk5.png)

Windows下安装驱动和CUDA简单便捷，不单Windows 10会自动安装一个适配版本，还可以通过Update自动安装认证驱动。推荐到NVIDIA官网去下载最新或次新的驱动程序，CUDA Toolkit安装方法同上，且安装包里自带一份驱动，如果追求兼容性稳定就安装包内的驱动，可以选择clean installation的方式而不必担心冲突；唯一要做的可能是需要手工拷贝一下适配版本的cuDNN散装文件，还需要根据需要在`PATH`中添加路径。

至于AMD GPU，设计漂亮，性能尚佳，以后倘若离开了AI方向或AMD也能提供AI的算力，一片到若干片AMD公版GPU也是非常棒的选择。就算力基建而言，NVIDIA GPU昂贵在CUDA核心，作为AI通用算力的唯一基石，别无选择，OpenCL还有很长的一段路要走。

## IDE, Editor

很长时间以来，我都更青睐编辑器而不是集成开发环境，追溯原因，大概是初中的时候喜欢用一款叫Q10的沉浸式文本编辑器写文章。[Sublime Text 2](https://www.sublimetext.com/2)使用过很长一段时间（一直没买正版真是抱歉了），同时习惯性地会使用[vim](https://www.vim.org/)，因为MSYS2可以安装它。

这两款编辑器最大的特点就是轻盈且好用：

- Sublime Text最初令人着迷的是使用`Ctrl+Alt`进行多行编辑，现在是对编辑器功能的硬性要求；

- 通过`Ctrl+Wheel`自由改变字体大小也是一个好功能，VS Code至今都没有这个功能；

- Sublime Text配色主题Monokai是我使用的第一个暗色主题，现在会根据心情和光照条件选择，亮色主题优先；

- 因为依赖命令行自然对vim也产生依赖，MSYS2使用的terminal是Mintty，如果在`~/.vimrc`中`set cursorline`会得到一根当前编辑行的标线，在cmd下却是高亮行，Linux Terminal也有这根标线；

- vim的自带高亮配色简单清晰，命令行下面不花眼，也是一个很棒的特性；

- 此外，对二者的插件功能没有深度自定义，只安装了少量的插件，不过之后都通过配置替代了插件，好写字才是硬道理。

时至今日，Sublime Text已经基本不用了，现在更多的是使用VS Code。VS Code的流行曲线和Chrome非常相像，Microsoft对开发工具的雕琢实力可见一斑。Microsoft正在优化开发生态对std C++的原生化支持，比如推出C++/WinRT（C++17），主推WSL的环境做Linux开发环境等。

VSCode下面Extension也非常丰富，根据工作时所使用的编程语言安装相应的语言插件。代码助手推荐[IntelliCode](https://visualstudio.microsoft.com/services/intellicode/)，这是全新的AI代码助手，主要功能有：代码补全建议，辅助代码重构，以及标注代码审阅。

虽然只有轻度使用经验，依然推荐JetBrain的[PyCharm](https://www.jetbrains.com/pycharm/whatsnew/)和[IntelliJ](https://www.jetbrains.com/idea/whatsnew/)，专业开发IDE的厂家产品很强。

弃用Jupyter Notebook的主要原因是有体验更佳的[Colaboratory](https://colab.research.google.com)。现在，可以点击右上角的“小烧瓶”体验[全新的编辑器](https://colab.research.google.com/notebooks/editor_details.ipynb)，加入了自动填充和键盘映射，作为一个自带算力的notebook，令人享受。

![colab new features](https://i.loli.net/2019/10/11/fprTLyKaEX5iMgb.png)

## Fonts

字体表达的是语言文字的 __形态之美__，换上清晰美观的字体可以让心情愉悦很多。

Windows cmd采用chcp编码的方式来限制字体的选择，这导致很多漂亮的英文字体在chcp:936下不能使用或中文显示都是不正确显示的，Mintty下中文可以正常显示，但字体一般不是特别漂亮，考虑到配置的统一，将cmd和Mintty的字体配置成[Inziu Iosevka](https://typeof.net/Iosevka/inziu.html)，并根据分辨率的不同设置成合适的大小，vim的字体就跟随命令行统一，vim下的体验觉得Inziu Iosevka会显得有一些紧凑、偏细。
Ubuntu Terminal字体和配色不做特别的设置，原生的字体和主题就很美观，尤其是深紫色的背景。

VS Code字体可以按照font-family设置，所以中英文混合使用就不成问题，从Windows拷贝一份Microsoft YaHei到Linux下作为中文字体。

![vscode font family](https://i.loli.net/2019/10/11/9pVJkIZO4yuREA1.png)

在此推荐几款等宽字体：

- Consolas，一款著名的命令行、编程等宽字体，VS Code在Windows下的默认字体，听说隔壁Eclipse也在用；

- [Roboto Mono](https://fonts.google.com/specimen/Roboto+Mono)，Roboto是Google的开源字体家族，旨在优化多种设备上的显示；Roboto Mono是等宽版本，在Google的网站上展示代码一般用的就是这一款；

- [Cascadia Code](https://devblogs.microsoft.com/commandline/cascadia-code/)，在Windows Terminal的宣传片上第一次看到，近期正式发布，是为Windows Terminal特别定制的等宽字体，这个字体使用感受可以用“有趣”二字概括；

- CamingoCode，是个人2018年的年度字体，弧圈形状偏方，看起来是比较工整板正的感觉，宽度不紧不松刚刚好。

