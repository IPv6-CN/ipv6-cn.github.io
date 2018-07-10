---
layout: default
---

# IPv6 中文资料和培训站点

## 笔者的一些思考和想法
{%- for post in site.tags.reflection %}
  <h3>
    <a href="{{ post.url }}">{{ post.title }}</a>
  </h3>
  <blockquote>{{ post.excerpt | strip_html }}</blockquote>
  <p>&nbsp;</p>
{%- endfor %}

---

## [原创 IPv6 教程]({% link tutorials/index.md %})
<ul>
{%- for page in site.pages %}
  {%- for t in page.tags %}
    {%- if t == "tutorial" %}
<li><a href="{{ page.url }}">{{ page.title }}</a></li>
    {%- endif %}
  {%- endfor %}
{%- endfor %}
</ul>
## [IPv6 参考资料]({% link references/index.md %})

<ul>
{%- for page in site.pages %}
  {%- for t in page.tags %}
    {%- if t == "reference" %}
<li><a href="{{ page.url }}">{{ page.title }}</a></li>
    {%- endif %}
  {%- endfor %}
{%- endfor %}
<ul>