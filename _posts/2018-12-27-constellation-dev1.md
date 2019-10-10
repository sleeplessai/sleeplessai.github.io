---
layout: post
title: "Project Constellation Dev1"
date: 2018-12-27 01:34:20 +0800
comments: true
categories: [dev]
excerpt: "一份送给自己的2019年的新年礼物、2018年十周年的纪念礼物，现与你分享！"
stickie: true
---

## Project Constellation

Project Constellation（中文意为星座、荟萃）, which will be officially released in 2019 January, is my brand new blog powered by Jekyll and hosted on GitHub Pages.

## The Brief Story of Constellation

萌发做这件事情的想法已经不是一两天了，现在来看这件事情却拥有更多的仪式感，或者说需要开始理由和契机。长期以来我主要研究算法，对于整体系统的构建没什么自信，也一直找不到心仪的（定制化、可开发性等）代替品，想到要购买云主机自己挂站，又因为早年的WordPress维护失败造成了阴影，用这类框架开发任务又过于繁重，一直搁置到近期。

近期萌生这个想法大概是在2018年11月末，因为很想很想写一些东西，但不管是Qzone还是Cppblog貌似都太老旧，独立性也不够强，归属感更是无从谈起。10月我在Windows上编译Caffe出现了很多问题，因此阅读了很多博客的文章，有一篇对我帮助很大的文章，那个博客是维护在[Coding.net](https://coding.net/)上的。这又一次激起了我的开发热情，而课业忙碌就没行动。直到12月初对这类独立博客调查后，恍然间发现原来[GitHub Pages](https://pages.github.com/) based Blog一直在我身边：以文档、博客、专栏、Wiki的形式在帮助我的理解和学习。通过一天的整理，我终于开始了行动，经过几天努力，才得以见到[Constellation](https://sleeplessai.github.io/)的Dev1版本，在未来的一个月里，它将不断成长，最终稳定下来，成为见证我下一路成长的伙伴。

2018年是我学习计算机的十周年，同在今年，我成为了计算机科学与技术专业的研究生，尽管至此我还抱有遗憾或经历低迷，但2018年作为一个始末，我真真切切地享受到了这十年来最畅快的一次成功，“这个机会再合适不过了！”，我对自己说。

作为一份送给自己**2019年的新年礼物**、**2018年十周年的纪念礼物**，我很开心与你分享[**Constellation**](https://sleeplessai.github.io/)项目的Dev1版本。

## Journals

### Before 2018/12/23

- 学习使用Markdown，着重在格式排版、数学编辑，图表功能短期内不会使用，尚未学习。

### 2018/12/23

- 新建第一个空仓库 ConstellationDev0（测试完成后已删除），使用README.md构建了第一个原型。

- 在`_config.yml`中开启MathJax支持，通过Markdown的HTML支持，使得满足对Latex数学公式的支持。

- 发现在线维护GitHub Pages局限太多，不支持大多数git功能：不能在线commit多个文件的修改、提供主题过于丑陋等。

- 开始探索Jekyll主题，最先选中的是[Contrast](https://github.com/niklasbuschmann/contrast)，在`_config.yml`中添加remote-theme可以直接加载主题，缺失可维护性，最终弃用。

- 转而使用Fork，但由于尚未理解整个Jekyll构造，添加[Katex](https://katex.org/)支持时候出现问题，找不到加入JS代码的文件等，使用新的主题。

### 2018/12/24-25

- 在本地使用MSYS2的Mingw64构建Jekyll环境。Fork、Clone项目，使用`bundle exec jekyll serve`启动项目时出现时区等问题，在`_config.yml`中添加timezone解决。

- 关于环境，存在问题是Ctrl+C停用serve之后，会留存名为`Ruby interupreter (CUI) 2.5.3p105[x64-mingw32]`和`Runtime Broker`进程，目前不影响效率，手动结束进程。

- 项目启动后，会将文件生成到静态的`_site/`中，直接修改`_site/`中的文件是无效的。最终通过**观察法**，理解了“拼接”起来的HTML页面和CSS文件关系，开发思路完全打开。

- [Constellation2019Dev1Leonids](https://github.com/sleeplessai/Constellation2019Dev1Leonids)是选中的另一款主题，同样是简洁风，有sidebar，第一感觉字体偏小，图片很大。以狮子座流星雨为名，的确很吸引我。

- [mirror-web](https://github.com/sleeplessai/mirror-web)是清华大学开源镜像站的网站项目。作为我唯一指定的[开源镜像站](https://tuna.moe/)，它的页面设计和功能真的非常棒，一度成为我想模仿的对象，因此引发了无人应答的[question not issue #26](https://github.com/tuna/blogroll/issues/26)。通过这个项目，很早之前就了解有Jekyll这么个玩意，那时候只认为Jekyll是一个静态页面生成脚本，语言也是我不会的Ruby。没想到却与[Constellation项目](https://sleeplessai.github.io/)的技术路线不谋而合。本次fork该项目是用于理解Jekyll的运行原理，感谢清华大学的TUNA协会。

### 2018/12/26-27

- 今天进度主要是在26日凌晨和傍晚至晚上完成的。

- 博客的核心理念是表达（express）,核心是post，故暂时从项目中删除tags（词云）、resume（简历）、about（关于）页面。

- 26日凌晨，通读了[Constellation2019Dev1Leonids](https://github.com/sleeplessai/Constellation2019Dev1Leonids)的代码，在主题色和头像文件上做出了更改，我对原生配色不满意，需要重新寻找配色。

- 顺带解决了[Katex](https://katex.org/)的支持问题，拥有了更快的数学公式渲染速度。到CDN找到最新版本的源，由于使用了kramdown解析，数学书写全部改用双dollar的形式。

- 26日下午，对主题色、代码高亮色进行挑选，在[配色网](http://peise.net)挑选、试验了6小时之后，于26日晚10时找到满意的配色。最终使用了[richleland/pygments-css](https://github.com/richleland/pygments-css/blob/master/vs.css)VS的配色组作为基础，玫红深紫结合主题青为主色，完成了高亮配色。

- Windows的字体渲染的确还是赶不上MacOS，在我的X24Q屏幕以2K分辨率观察字体，边缘还是有不少模糊，在使用亮色系时，字体会变得更加模糊。如果字体不会冲突，代码高亮渲染颜色不会出现歧义，Dev1版本的颜色就不作改变了。

### 2018/12/29-30

- 29日晚，完成了Comment功能的初步实现，基于[Gitalk](https://github.com/gitalk/gitalk)。[Disqus](https://disqus.com/)由于载入、用户登陆问题，弃用。

- [Disqus](https://disqus.com/)样式和Emoji样式非常棒，后续中有想法将更漂亮的Emoji样式加入，还需修改[Gitalk](https://github.com/gitalk/gitalk)样式使其与[Constellation](https://sleeplessai.github.io/)风格更一致。

- 目前还没有完成自动Initialize Comment的功能；另外，Console中会出现401（Unauthorized）错误。

- 考虑到让更多朋友加入讨论，需要引入除GitHub账户以外的OAuth，目前使用的[Gitalk](https://github.com/gitalk/gitalk)将会被增强或自己开发一个新的Comment模块。

- 30日凌晨，完成了小部分代码结构优化：删除了独立的JS脚本，合并到统一的HTML文件中的script标签中，并为Comment和数学渲染的JS代码模块添加了载入判断，以优化载入速度。

- 将原先内置的jQuery删除，所有JS库均更新至最新版本，在线源由[CDNJS](https://cdnjs.com/)提供。

- 今日工作开始前，由于pacman将Ruby更新到2.6.0，导致坏境崩掉了，原因是Gem依赖中的ffi-1.9.25包仅支持到Ruby < 2.6.0。通过移除Ruby 2.6.0的包，到源上找了Ruby 2.5.3的包手工安装并在`/etc/pacman.conf`添加了`IgnorePkg = mingw-w64-x86_64-ruby`避免日常滚动破坏依赖。此外，开发前切记使用`bundle update`更新或使用`bundle install`安装所需的Gem包。

### 2019/09/14

- 博客的Ruby升级到`ruby 2.6.3p62 (2019-04-16 revision 67580) [x64-mingw32]`，成功生成。

- 删除了test分类下的post，修改了一些措辞。

### 2019/10/10

- 最近学习阵地转移到316实验室，HP Z840上系统是`Ubuntu 18.04.3 LTS`，博客的Ruby版本选择了`ruby 2.5.1p57 (2018-03-29 revision 63029) [x86_64-linux-gnu]`。原因是，Snap Store里Ruby的版本是2.6.5，找不到对应版本的`ruby-dev`包，因此缺少`ruby.h zlib.h`等头文件，以致于某些native extension无法生成，只能通过`sudo apt install ruby-dev libz-dev`安装。

