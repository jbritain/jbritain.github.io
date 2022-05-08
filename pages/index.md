---
layout: default
title: home
permalink: /
---

# {{ site.title }}/
Hello, and welcome to my website. Here you can find my various programming projects, and a blog where I ramble about them.

**You can find me online on**
- [Github](https://github.com/{{ site.github }})
- [YouTube](https://www.youtube.com/channel/{{ site.youtube }})

If you want to contact me, fill out the form on the [contact page](/contact) to send me an email.

---

## Latest Blog Post
{% for post in site.posts limit:1 %}
### [{{ post.title }}]({{ post.url }})
<small>By {{ post.author }} • {{ post.date | date: "%-d %B %Y" }}</small>
{{ post.excerpt }}
{% endfor %}

[More posts](/blog)

## Latest YouTube Video
<h3 id="YTTitle">Video failed to load</h3>
<small id="YTInfo"></small>
<iframe style="width: 560px; height: 315px; max-width: 100%" frameborder="0" class="card" id="YTEmbed">Your browser does not support iFrames.</iframe>
<p id="YTDescription"></p>

[More videos](https://www.youtube.com/channel/{{ site.youtube }})

## Other Stuff
- [List of Funny Wikipedia Articles](/wikipedia)
- [Funny Quotes From My Discord Server](/quotes)

<script> // yes, javascript code I wrote myself that isn't spaghetti
    const frame = document.getElementById("YTEmbed");
    const title = document.getElementById("YTTitle")
    const info = document.getElementById("YTInfo")
    const description = document.getElementById("YTDescription")

    const channelID = "{{ site.youtube }}";
                
    fetch("https://api.rss2json.com/v1/api.json?rss_url=" + encodeURIComponent("https://www.youtube.com/feeds/videos.xml?channel_id=" + channelID))
        .then(response => response.json())
        .then(data => {
            video = data["items"][0];
            guid = video["guid"]

            const embedURL = "https://www.youtube-nocookie.com/embed/" + guid.replace("yt:video:", "");
            
            frame.src = embedURL;
            title.innerText = video["title"];
            info.innerText = "By " + video["author"]
            description.innerText = video["description"]
        })
        .catch(console.error);
</script>
