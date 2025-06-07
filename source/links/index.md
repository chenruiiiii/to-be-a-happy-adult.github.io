---
title: links
date: 2025-06-05 22:29:00
type: 
---


<style>
a {
    text-decoration: none;
    border-bottom: none;
}
.link-container {
    display: grid;
    gap: 20px;
}

@media screen and (768px <=width ) {
    .link-container {
        grid-template-columns: repeat(3, 1fr);
    }
}

@media screen and (400px <=width <=768px) {
    .link-container {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media screen and (width <=400px) {
    .link-container {
        grid-template-columns: repeat(1, 1fr);
    }
}

.link-item {
    padding: 20px;
    border-radius: 8px;
    box-shadow: 1px 1px 5px 0 rgba(0, 0, 0, .1), inset 1px 1px 2px 0 rgba(0,0,0, .1);
    transition: box-shadow .2s ease-in-out, transform .2s ease-in-out;
}

.link-item:hover {
    box-shadow: 1px 1px 5px 0 rgba(0, 0, 0, .2),inset 1px 1px 2px 0 rgba(0,0,0, .1);
    transform: scale(1.1);
}

body[class*='darkmode--activated'] .link-item {
    box-shadow: 1px 1px 5px 0 rgba(255,255,255, .1), inset 1px 1px 5px 0 rgba(255,255,255, .1);
}

body[class*='darkmode--activated'] .link-item:hover {
    box-shadow: 1px 1px 5px 0 rgba(255,255,255, .2), inset 1px 1px 5px 0 rgba(255,255,255, .1);
}

.link-item .item-top {
    display: flex;
    align-items: center;
    gap: 10px;
    margin-bottom: 10px;
    color: #000;
}

body[class*='darkmode--activated']  .link-item .item-top {
    color: rgba(255,255,255,.9)
}

.item-top img {
  margin:0;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    object-fit: cover;
}

.description {
    font-size: 14px;
    color: rgba(0,0,0,.8);
    overflow: hidden;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 2;
}

body[class*='darkmode--activated'] .description {
    color: rgba(255,255,255,.8);
}
</style>

<div class="links-container">
  <div class="link-item">
    <a href="https://a-big-dreamer.github.io/" target="_blank" class="link-card">
      <div class="link-icon">
        <img src="../img/A-Big-Dreamer's-Blog.jpg" alt="LINUX DO" />
      </div>
      <div class="link-info">
        <h3>A Big Dreamer's Blog</h3>
        <p>梦想成为伟大的梦想家</p>
      </div>
    </a>
  </div>
</div>

