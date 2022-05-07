---
title: 使用Markdown+LaTeX编写模板并生成pdf
date: 2022-05-07 03:01:01
tags: [Markdown, LaTeX]
author: VsKendo
hide: true
categories: 写作
summary: 许多人写博客都喜欢用Markdown，但是想将多个Markdown文章内容合在一起然后生成pdf却不容易，本教程将讲述如何完成你的pdf编写（多文件引入+代码高亮）。
description: 许多人写博客都喜欢用Markdown，但是想将多个Markdown文章内容合在一起然后生成pdf却不容易，本教程将讲述如何完成你的pdf编写（多文件引入+代码高亮）。
---

# 简述

本教程目的是让使用Markdown的人都可以生成理想的pdf文件。如果只是使用各种软件将多个Markdown合并再导出成pdf固然可以，但是这不优雅，且几乎无法自定义。对于一些要求高的（比如要创建自己的ACM比赛模板）的人，需要一种更好的方法。

在本教程，你可以方便地将你的MD文件（使用优雅的字体）与代码（支持高亮显示）引用到一个LaTex文件中，最后直接将该LaTex文件转化为PDF文件，同时还不用深入学习LaTex。

# 环境

我使用的是2020的M1 MAC，但是Windows、Linux和x86的Mac用户也不用担心，这个教程已经考虑了跨平台的问题，不用担心多平台的困扰。

## 所用软件

1. Typora——用于编写Markdown的软件
2. MacTex——用于编写Latex的软件。其安装后会有几个小软件
   1. TeXShop：这是一个 Tex 的前端，是 Tex 的编辑器和预览器。新手可以使用这个来学习。（老手一般都有自己喜欢的编辑器，比如使用VsCode而不是它）
   2. TeX Live Utility：这个实用工具可以通过网络更新 TeX Live 包，以及配置 Tex 中的页面大小。（说白了就是个包管理器，用于更新软件用的）
   3. LaTexiT：这是一个简单的 LaTex 图形应用程序，但是一般被当作方程编辑器，用于写数学方程，因为可以很快看到写的公式的样式。（本教程并未使用，但是一些需要经常使用LaTex编写数学公式的人会用到）
   4. BibDesk：这是一个用于编辑和管理参考书目的工具。（论文、参考文献管理工具，本教程并未使用）
   5. 在“应用程序”里多了一个“Docs and Spell Utilities”，这个是一些介绍文档，有空可以看看。
3. pandoc——一个转换器，可以把各种格式的文档转换成其他格式，比如将HTML转PDF等（本教程用它来将Markdown文件转换为Tex文件以便让LaTex引用）

# References

1. 【LaTeX】新手教程：从入门到日常使用：https://zhuanlan.zhihu.com/p/456055339
2. 「MacTex 小笔记」准备篇：https://blog.csdn.net/qq_33919450/article/details/124444187
3. ACM赛前准备——模板(排版篇)：https://www.cnblogs.com/palayutm/p/6444833.html#e5ae89e8a385texlive_11
4. ACM赛前准备——模板GitHub：https://github.com/palayutm/ply-template
5. Code Highlighting with minted：https://www.overleaf.com/learn/latex/Code_Highlighting_with_minted
6. Multi-file LaTeX projects：https://www.overleaf.com/learn/latex/Multi-file_LaTeX_projects
7. how to typeset Chinese documents：https://www.overleaf.com/learn/latex/Chinese
8. Pandoc  a universal document converter：https://pandoc.org/index.html
9. pandoc的mac操作指南，小白版：https://blog.csdn.net/weixin_43649776/article/details/115233416
10. 使用pandoc将markdown转latex去除\hypertarget的解决方法：https://blog.csdn.net/qq_43039472/article/details/122422737
11. macOS LaTeX 提示Package fontspec Warning: Font “STSong“ does not contain requested Script “CJK“的原因：https://blog.csdn.net/HermitSun/article/details/116674418
12. MscTeX警告Package fontspec Warning: Font “Songti SC Light“ does not contain requested(fontspec)的解决：https://blog.csdn.net/qq_41437512/article/details/121313935
13. Mac中使用LaTeX的中文字体出现Package fontspec Error: The font “宋体“ cannot be found.解决方案：https://blog.csdn.net/qq_41437512/article/details/114054458
