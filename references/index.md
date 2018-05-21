---
lcategories: [rfc, book, ppt, paper]
---
{% for cat in page.lcategories %}

<h2>
{{ cat }}
</h2>

<ul>
{% for p in site.pages %}
  {% for c in p.categories %}
    {% if c == cat %}
      <li><a href="{{ p.url }}">{{ p.title }}</a></li>
    {% endif %}
  {% endfor %}
{% endfor%}
</ul>
{% endfor %}
