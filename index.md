---
layout: home
title: 首页
---

<div class="home-intro">
  <h1>✨ 欢迎来到「没有夜空的星星」</h1>
  <p style="font-size: 1.1rem; color: #8b949e;">
    这里没有夜空，但有星星闪烁
  </p>
</div>

## 最近的文章

<ul class="post-list">
  {% for post in site.posts %}
    <li>
      <h3>
        <a class="post-link" href="{{ post.url | relative_url }}">{{ post.title }}</a>
      </h3>
      <span class="post-meta">{{ post.date | date: "%Y年%m月%d日" }}</span>
      {% if post.excerpt %}
        <p>{{ post.excerpt | strip_html | truncate: 100 }}</p>
      {% endif %}
    </li>
  {% endfor %}
</ul>

---

## 关于我

一个喜欢在代码宇宙里探索的程序员。  
偶尔记录一些技术踩坑、学习笔记和生活碎片。

*正在慢慢把这里填满...*
