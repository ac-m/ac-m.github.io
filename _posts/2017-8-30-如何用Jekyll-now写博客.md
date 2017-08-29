---
layout: post
title: 如何用Jekyll-now写博客
---

## 准备工作

1. 到<https://github.com/>注册帐号，登录你的github帐号。
1. Fork(即复制)Jekyll-now博客架构。具体操作可看<https://github.com/barryclark/jekyll-now>说明，一般操作如下：
    1. 到<https://github.com/barryclark/jekyll-now>，点右上角的"Fork"按钮。
    1. 点右边"Settings"按钮，修改"Repository name"为"ac-m.github.io"形式，其中"ac-m"替换为你github注册名。
    1. 用浏览器看"xxx.github.io"，你的博客已准备好。

    动态图 ![an image alt text](/images/step1.gif "an image title")

## 写博客(需把下面的ac-m替换为你github帐号)

1. 到<https://github.com/ac-m/ac-m.github.io/tree/master/_posts>，点"Create new file"按钮。
1. 文件名为2017-8-30-xxx.md格式，前面是日期，后面xxx为任意，文件名为.md。
1. 编辑开头为如下形式

```
---
layout: post
title: 如何用Jekyll-now写博客
---
```

1. 用Markdown语法编辑文件内容，Markdown效果请看<http://www.jekyllnow.com/Markdown-Style-Guide/>
1. Markdown效果对应原文请参考<https://raw.githubusercontent.com/barryclark/www.jekyllnow.com/gh-pages/_posts/2014-6-19-Markdown-Style-Guide.md>
1. Jekyll变量请参考<http://jekyllrb.com/docs/variables/>
1. 编辑结束请按"Commit changes"按钮。
1. 调试页面时判断页面是否刷新可用Jekyll变量site.time {{ site.time }} 。
