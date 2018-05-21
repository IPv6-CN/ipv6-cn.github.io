---
category: "rfc"
---
<ul>
  {% for p in site.pages %}
    {% for cat in p.categories %}
      {% if cat == page.category %}
        <li><a href="{{ p.url }}">{{ p.title }}</a></li>
      {% endif %}
    {% endfor %}
  {% endfor %}
</ul>
