---
layout: default
title: "Current Projects"
permalink: /currents/
---

# All Current Projects

{% for currents in site.currents %}
## [{{ currents.title }}]({{ currents.url | relative_url }})

**{{ currents.date | date: "%B %d, %Y" }}**

{% if currents.categories.size > 0 %}
**Categories:** {% for category in currents.categories %}{{ category }}{% unless forloop.last %}, {% endunless %}{% endfor %}
{% endif %}

{% if currents.tags.size > 0 %}
**Tags:** {% for tag in currents.tags %}{{ tag }}{% unless forloop.last %}, {% endunless %}{% endfor %}
{% endif %}

{% if currents.excerpt %}
{{ currents.excerpt }}
{% endif %}

[Read More →]({{ currents.url | relative_url }})

---
{% endfor %}
