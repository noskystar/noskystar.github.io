---
layout: home
title: 首页
---

# 欢迎来到「没有夜空的星星」 ✨

这里没有夜空，但有星星闪烁。

## 最近的文章

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>

---

## 关于我

一个喜欢在代码宇宙里探索的程序员。
偶尔记录一些技术踩坑、学习笔记和生活碎片。

*正在慢慢把这里填满...*
