---
layout: default
title: Home
---
# 个人学习笔记归档

---
layout: default
---

<!--- 上面这部分是Jekyll的Front Matter，它告诉Jekyll这个页面使用'default'布局 --->
<!--- 下面的内容会根据您的设置和文章动态生成 --->

<h2>最新文章</h2>

<!--- 此循环将遍历您_posts目录下的所有文章 --->
<ul>
  {% for post in site.posts %}
    <li>
      <!--- 为每篇文章生成一个带链接的列表项 --->
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a> - <span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span>
    </li>
  {% endfor %}
</ul>


## Recent Posts
<!-- 这里未来可以自动列出您的博客文章 -->
- [My First Post](/_posts/2024-10-06-first-post.md)


## About Me
[Learn more about me](/about.md)
