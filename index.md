---
layout: page
title: Kilt and Code is the personal website of <a href="/about">Steven McLintock</a>
---

<section class="text-center">
  <p>I'm a Scottish software developer based in Stratford, Ontario. I enjoy writing about the things I 
  learn, primarily on the languages and frameworks I use on a daily basis. You can also read about 
  <a href="{{ site.baseurl }}/2020/02/16/my-journey-to-becoming-a-full-stack-developer/">my journey to becoming a full stack developer</a>, 
  <a href="{{ site.baseurl }}/2019/06/30/coming-to-canada-immigrating-to-toronto-as-a-dotnet-developer/">immigrating to Toronto as a .NET developer</a> 
  and more in the <a href="{{ site.baseurl }}/articles">archives</a>.</p>

  {%
    include image.html
    file='me.jpg'
    alt='Steven McLintock'
  %}
</section>

<section class="text-center" style="margin: 35px 0;">
  <h4>Latest Articles</h4>
</section>

{% for post in site.posts limit:3 %}
  {%
    include article-summary.html
    url=post.url
    title=post.title
    date=post.date
    icon=post.icon
    excerpt=post.excerpt
  %}
{% endfor %}

<section class="text-center" style="margin-top: 35px;">
  <a href="{{ site.baseurl }}/articles">View all articles</a>
</section>