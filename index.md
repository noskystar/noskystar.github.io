---
layout: home
title: 首页
---

<style>
/* 主页星空头部 */
.star-home {
  text-align: center;
  padding: 5rem 2rem;
  background: linear-gradient(135deg, 
    rgba(26, 26, 46, 0.95) 0%, 
    rgba(22, 33, 62, 0.98) 50%, 
    rgba(15, 52, 96, 0.95) 100%);
  border-radius: 20px;
  margin-bottom: 3rem;
  position: relative;
  overflow: hidden;
  border: 1px solid rgba(57, 208, 216, 0.2);
  box-shadow: 
    0 0 80px rgba(57, 208, 216, 0.15),
    inset 0 0 80px rgba(255, 215, 0, 0.05);
}

/* 星星闪烁动画 */
.star-home .star {
  position: absolute;
  color: #ffd700;
  font-size: 1.2rem;
  animation: twinkle 2.5s ease-in-out infinite;
}

.star-home .star:nth-child(1) { top: 15%; left: 10%; animation-delay: 0s; }
.star-home .star:nth-child(2) { top: 25%; right: 15%; animation-delay: 0.5s; }
.star-home .star:nth-child(3) { top: 60%; left: 8%; animation-delay: 1s; }
.star-home .star:nth-child(4) { bottom: 20%; right: 10%; animation-delay: 1.5s; }
.star-home .star:nth-child(5) { top: 40%; right: 25%; animation-delay: 2s; }

@keyframes twinkle {
  0%, 100% { opacity: 0.2; transform: scale(1) rotate(0deg); }
  50% { opacity: 1; transform: scale(1.3) rotate(180deg); }
}

/* 流星效果 */
.star-home .shooting-star {
  position: absolute;
  width: 100px;
  height: 2px;
  background: linear-gradient(90deg, transparent, #fff, transparent);
  animation: shoot 3s linear infinite;
  opacity: 0;
}

.star-home .shooting-star:nth-child(6) { 
  top: 20%; left: -100px; 
  animation-delay: 0s;
}
.star-home .shooting-star:nth-child(7) { 
  top: 50%; left: -100px; 
  animation-delay: 2s;
}

@keyframes shoot {
  0% { transform: translateX(0); opacity: 1; }
  100% { transform: translateX(600px); opacity: 0; }
}

.star-home h1 {
  font-size: 2.8rem;
  margin-bottom: 0.75rem;
  background: linear-gradient(135deg, #ffd700 0%, #fff 50%, #39d0d8 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  letter-spacing: 2px;
}

.star-home .subtitle {
  color: #39d0d8;
  font-size: 1.15rem;
  font-style: italic;
  margin-bottom: 1.5rem;
  opacity: 0.9;
  letter-spacing: 1px;
}

.star-home .description {
  color: #7d8590;
  font-size: 1.05rem;
  max-width: 500px;
  margin: 0 auto;
  line-height: 1.9;
}

/* 分区标题 */
.section-title {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  color: #39d0d8;
  font-size: 1.4rem;
  font-weight: 600;
  border-bottom: 2px solid #30363d;
  padding-bottom: 0.75rem;
  margin-top: 3rem;
  margin-bottom: 1.5rem;
}

.section-title::before {
  content: "";
  display: inline-block;
  width: 4px;
  height: 28px;
  background: linear-gradient(180deg, #ffd700, #39d0d8);
  border-radius: 2px;
}

/* 关于区域 */
.about-section {
  background: linear-gradient(135deg, #1c2128 0%, #161b22 100%);
  padding: 2.5rem;
  border-radius: 16px;
  border: 1px solid #30363d;
  border-left: 4px solid #39d0d8;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
}

.about-section p {
  color: #7d8590;
  line-height: 1.9;
  margin-bottom: 1rem;
}

.about-section strong {
  color: #e6edf3;
}

.about-section blockquote {
  border-left-color: #ffd700;
  background: linear-gradient(135deg, rgba(255, 215, 0, 0.08) 0%, rgba(57, 208, 216, 0.08) 100%);
  margin: 1.5rem 0 0 0;
}

/* 页脚签名 */
.footer-signature {
  text-align: center;
  color: #6e7681;
  font-size: 0.95rem;
  margin-top: 3rem;
  padding: 2rem 0;
  border-top: 1px solid #30363d;
  letter-spacing: 2px;
}

.footer-signature span {
  background: linear-gradient(90deg, #ffd700, #39d0d8);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

/* 响应式 */
@media screen and (max-width: 600px) {
  .star-home { padding: 3rem 1.5rem; }
  .star-home h1 { font-size: 2rem; }
  .star-home .subtitle { font-size: 1rem; }
  .about-section { padding: 1.75rem; }
}
</style>

<div class="star-home">
  <!-- 装饰星星 -->
  <span class="star">✦</span>
  <span class="star">✧</span>
  <span class="star">⋆</span>
  <span class="star">✦</span>
  <span class="star">✧</span>
  <!-- 流星 -->
  <span class="shooting-star"></span>
  <span class="shooting-star"></span>
  
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

<p>一个喜欢在代码宇宙里探索的程序员。</p>

<p><strong>这里记录的是：</strong></p>
<ul>
  <li>🛠️ 技术踩坑的微弱火光</li>
  <li>📚 读书笔记的点点星光</li>
  <li>💡 偶尔闪现的生活碎片</li>
</ul>

<blockquote>
  <p>"夜空太亮，星星不易被发现。<br>
  但这个角落够暗，足够让微光被看见。"</p>
</blockquote>

</div>

<div class="footer-signature">
  ✦ ⋆ 感谢路过，愿这里能给你一点微光 ⋆ ✦
</div>
