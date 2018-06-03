---
lcategories: {rfc: "IETF RFC", book: "书籍", ppt: "演示文稿", paper: "论文"}
---

{% for cat in page.lcategories %}

<h2>
{{ cat[1] }}
</h2>

<ul>
{% for p in site.pages %}
  {% for c in p.categories %}
    {% if c == cat[0] %}
      <li><a href="{{ p.url }}">{{ p.title }}</a></li>
    {% endif %}
  {% endfor %}
{% endfor%}
</ul>
{% endfor %}
