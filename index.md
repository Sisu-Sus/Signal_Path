---
layout: default
title: "Home"
---

## Current Projects

{% if site.posts.size > 0 %}
{% for post in site.posts limit:3 %}
<div style="background: #2a2a2a; padding: 1.5rem; margin: 1rem 0; border-left: 4px solid #4CAF50; border-radius: 4px;">
  <h3><a href="{{ post.url | relative_url }}" style="color: #ffffff; text-decoration: none;">{{ post.title }}</a></h3>
  <p style="color: #888; margin: 0.5rem 0;">{{ post.date | date: "%B %d, %Y" }}</p>
  {% if post.excerpt %}
  <p style="color: #ccc;">{{ post.excerpt | strip_html | truncatewords: 30 }}</p>
  {% endif %}
  <a href="{{ post.url | relative_url }}" style="color: #4CAF50; text-decoration: none; font-weight: bold;">Read More →</a>
</div>
{% endfor %}
{% else %}
<p style="color: #888; font-style: italic;">No posts yet. Create your first post in the _posts directory!</p>
{% endif %}

<div style="text-align: center; margin: 2rem 0;">
  <a href="{{ '/currents/' | relative_url }}" style="background: #4CAF50; color: white; padding: 0.8rem 2rem; border-radius: 5px; text-decoration: none; font-weight: bold;">View All Posts</a>
</div>

## 🔬 Previous Research

{% if site.research.size > 0 %}
{% for research in site.research limit:2 %}
<div style="background: #2a2a2a; padding: 1.5rem; margin: 1rem 0; border-left: 4px solid #FF6B6B; border-radius: 4px;">
  <h3><a href="{{ research.url | relative_url }}" style="color: #ffffff; text-decoration: none;">{{ research.title }}</a></h3>
  <p style="color: #888; margin: 0.5rem 0;">{{ research.date | date: "%B %d, %Y" }}</p>
  {% if research.severity %}
  <span style="background: #FF6B6B; color: white; padding: 0.2rem 0.5rem; border-radius: 3px; font-size: 0.8em;">{{ research.severity | upcase }}</span>
  {% endif %}
  {% if research.excerpt %}
  <p style="color: #ccc; margin-top: 1rem;">{{ research.excerpt | strip_html | truncatewords: 25 }}</p>
  {% endif %}
  <a href="{{ research.url | relative_url }}" style="color: #FF6B6B; text-decoration: none; font-weight: bold;">Read Analysis →</a>
</div>
{% endfor %}
{% else %}
<p style="color: #888; font-style: italic;">No research papers yet. Create your first research paper in the _research directory!</p>
{% endif %}

<div style="text-align: center; margin: 2rem 0;">
  <a href="{{ '/research/' | relative_url }}" style="background: #FF6B6B; color: white; padding: 0.8rem 2rem; border-radius: 5px; text-decoration: none; font-weight: bold;">View All Research</a>
</div>

---

<div style="text-align: center; color: #888; margin-top: 3rem;">
  <p><strong>{{ site.author.name }}</strong></p>
  <p>{{ site.author.bio }}</p>
  <p>{{ site.author.location }} | <a href="mailto:{{ site.author.email }}" style="color: #4CAF50;">{{ site.author.email }}</a></p>
</div>
