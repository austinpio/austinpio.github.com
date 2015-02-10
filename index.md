---
layout: page
title: Austin's Blog
tagline: Random Thoughts
---
{% include JB/setup %}


## Author Attributes

    author :
      name : Austin Pio
      email : austinpioj@gmail.com
      github : austinpio
      twitter : austinpioj

The theme should reference these variables whenever needed.
    
## Blog Posts

Here's the posts list.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



