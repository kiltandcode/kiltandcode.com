---
layout: page
title: "All Articles"
---

{% for post in site.posts %}
    {%
        include article-summary.html
        url=post.url
        title=post.title
        date=post.date
        icon=post.icon
        excerpt=post.excerpt
    %}
{% endfor %}