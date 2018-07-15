---
layout: post
title: What's Jekyll?
category: Dev
tags: [web, jekyll]
---
<div style="color:#1A86A8;" class="post-categories">
  {% if post %}
    {% assign categories = post.categories %}
  {% else %}
    {% assign categories = page.categories %}
  {% endif %}
  {% for category in categories %}
  {{category}}
  {% unless forloop.last %}&nbsp;{% endunless %}
  {% endfor %}
</div>

<br><br>
[Jekyll](http://jekyllrb.com) is a static site generator, an open-source tool for creating simple yet powerful websites of all shapes and sizes. From [the project's readme](https://github.com/mojombo/jekyll/blob/master/README.markdown):

  > Jekyll is a simple, blog aware, static site generator. It takes a template directory [...] and spits out a complete, static website suitable for serving with Apache or your favorite web server. This is also the engine behind GitHub Pages, which you can use to host your project’s page or blog right here from GitHub.

It's an immensely useful tool and one we encourage you to use here with Lanyon.

Find out more by [visiting the project on GitHub](https://github.com/mojombo/jekyll).

{% for post in site.categories.Dev %}
 <li><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
