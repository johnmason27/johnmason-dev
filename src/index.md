---
layout: default.liquid
---
## Posts

{% for post in collections.posts.pages %}
[{{ post.title }}]({{ post.permalink }})
{% endfor %}
