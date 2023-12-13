---
layout: default
---

<h1>{{ page.summary }}</h1>
{% for tag in page.tags %}<p class="label label-blue">{{ tag }}</p>{% endfor %}

{{ content }}
