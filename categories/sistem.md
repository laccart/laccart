---
layout: page
permalink: /blog/categories/sistem-guvenligi
---
 
<h3> Kategori : {{ page.title }} </h3>

<div class="card">
{% for post in site.categories.sistem-guvenligi %}
 <li class="category-posts"><span>{{ post.date | date_to_string }}</span> &nbsp; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endfor %}
</div>