---
layout: home
title: 首页
---

<style>
.star-home {
  text-align: center;
  padding: 3rem 1rem;
  background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
  border-radius: 12px;
  margin-bottom: 2rem;
  position: relative;
  overflow: hidden;
}

.star-home::before {
  content: "✦ ⋆ ✧ ⋆ ✦ ⋆ ✧ ⋆ ✦ ⋆ ✧ ⋆ ✦";
  position: absolute;
  top: 10px;
  left: 50%;
  transform: translateX(-50%);
  color: rgba(255, 255, 255, 0.3);
  font-size: 0.9rem;
  letter-spacing: 8px;
}

.star-home::after {
  content: "✦ ⋆ ✧ ⋆ ✦ ⋆ ✧ ⋆ ✦ ⋆ ✧ ⋆ ✦";
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  color: rgba(255, 255, 255, 0.3);
  font-size: 0.9rem;
  letter-spacing: 8px;
}

.star-home h1 {
  color: #fff;
  font-size: 2rem;
  margin-bottom: 0.5rem;
  text-shadow: 0 0 20px rgba(255, 255, 255, 0.5);
}

.star-home .subtitle {
  color: #a8d8ea;
  font-size: 1.2rem;
  font-style: italic;
  margin-bottom: 1rem;
}

.star-home .description {
  color: #e94560;
  font-size: 1rem;
  max-width: 500px;
  margin: 0 auto;
}

.section-title {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  color: #1a1a2e;
  border-bottom: 2px solid #e94560;
  padding-bottom: 0.5rem;
  margin-top: 2rem;
}

.about-section {
  background: #f8f9fa;
  padding: 1.5rem;
  border-radius: 8px;
  border-left: 4px solid #e94560;
}
</style>

<div class="star-home">
  <h1>🌟 没有夜空的星星</h1>
  <p class="subtitle">"Even without a night sky, stars still shine"</p>
  <p class="description">
    夜空太亮时，看不见星星。<br>
    但在黑暗里，每一点微光都值得被记录。
  </p>
</div>

## <span class="section-title">✨ 最新闪烁的星光</span>

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

## <span class="section-title">💫 关于这颗星星</span>

<div class="about-section">

一个喜欢在代码宇宙里探索的程序员。

**这里记录的是：**
- 🛠️ 技术踩坑的微弱火光
- 📚 读书笔记的点点星光  
- 💡 偶尔闪现的生活碎片

> *"夜空太亮，星星不易被发现。*  
> *但这个角落够暗，足够让微光被看见。"*

</div>

---

<p align="center" style="color: #8b949e; font-size: 0.9rem; margin-top: 2rem;">
  ✦ ⋆ 感谢路过，愿这里能给你一点微光 ⋆ ✦
</p>
