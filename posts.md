---
layout: default
title: "Sec Posts"
permalink: /posts/
---

# All Security Posts

{% for post in site.posts %}
## [{{ post.title }}]({{ post.url | relative_url }})

**{{ post.date | date: "%B %d, %Y" }}**

{% if post.categories.size > 0 %}
**Categories:** {% for category in post.categories %}{{ category }}{% unless forloop.last %}, {% endunless %}{% endfor %}
{% endif %}

{% if post.tags.size > 0 %}
**Tags:** {% for tag in post.tags %}{{ tag }}{% unless forloop.last %}, {% endunless %}{% endfor %}
{% endif %}

{% if post.excerpt %}
{{ post.excerpt }}
{% endif %}

[Read More →]({{ post.url | relative_url }})

---
{% endfor %}
