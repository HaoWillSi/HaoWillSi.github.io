---
title: "关于我"                            # 设置文章标题
categories:                               # 设置文章分类
- 随笔
tags:                                     # 设置文章标签
- blogs
excerpt: "Welcome to know me ~"           # 设置文章的展示内容
header:
    image: /images/painter.jpg           # 设置页面的顶端图片（完全扩展）
    overlay_image: /images/painter.jpg   # 设置页面的顶端图片（标题以及excerpt会显示在图片上）
    overlay_filter: 0.5                  # 图片的透明度设置（也可以设置为 rgba(255, 0, 0, 0.5)，前面三个为红绿蓝）
    teaser: /images/painter.jpg          # 设置该博客在相关推荐处的显示图像
    og_image: /images/painter.jpg        # 设置分享时的显示图像
    video:                               # 设置页面顶端的视频（基于 viemo 平台）
           id: 212731897
           provider: vimeo
---
{% include lib/mathjax.html %}

# 测试是否支持数学公式
# It works!
you can use an inline formula $$\forall x \in R$$ like this one


$$
M = \left( \begin{array}{ccc}
x_{11} & x_{12} & \ldots \\
x_{21} & x_{22} & \ldots \\
\vdots & \vdots & \ldots \\
\end{array} \right)
$$
