---
layout: default
title: "Security Research"
permalink: /research/
---

# Research Papers

{% for paper in site.research %}
## [{{ paper.title }}]({{ paper.url | relative_url }})

**{{ paper.date | date: "%B %d, %Y" }}**

{% if paper.severity %}
**Severity:** {{ paper.severity | upcase }}
{% endif %}

{% if paper.categories.size > 0 %}
**Categories:** {% for category in paper.categories %}{{ category }}{% unless forloop.last %}, {% endunless %}{% endfor %}
{% endif %}

{% if paper.excerpt %}
{{ paper.excerpt }}
{% endif %}

[Read Full Analysis →]({{ paper.url | relative_url }})

---
{% endfor %}
