---
layout: default
title: home
permalink: /
---

# jbritain.net/
Hello, and welcome to my website. Here you can find my various programming projects, and a blog where I ramble about them.

- [Github](https://github.com/pr0x1mas)
- [YouTube](https://www.youtube.com/channel/UCsixy16a1K_PZ2sudNt2zrQ) (Be warned, this currently contains just memes but I plan to put programming content there in the future)

---

## Latest Blog Post
{% for post in site.posts limit:1 %}
### [{{ post.title }}]({{ post.url }})
<small>By {{ post.author }} • {{ post.date | date: "%-d %B %Y" }}</small>
{{ post.excerpt }}
{% endfor %}

[See more posts](/blog)