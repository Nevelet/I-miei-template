---
title: "{{title | escape}}"
year: {{date | format("YYYY")}}
authors: {{authors}}
tags: 
  - zotero
  - {% for t in tags %}{{t.tag}}{% if not loop.last %}, {% endif %}{% endfor %}
citekey: {{citekey}}
---

> [!meta]- Metadata
> - Abstract: {{abstractNote}}
> - Zotero PDF Link: {{pdfZoteroLink}}
> - URL: {{url}}
> - Bibliography: {{bibliography}}
> - Correlazioni: {% for relation in relations | selectattr("citekey") %} [[{{relation.citekey}}]]{% if not loop.last %}, {% endif%} {% endfor %}


## Note
{% persist "notes" %}{% if isFirstImport %}
Scrivi qui le tue note!
{% endif %}
{% endpersist %}


## Annotazioni
{%- for attachment in attachments %}
{%- if attachment.annotations and attachment.annotations.length > 0 %}
---
### 📄 {{ attachment.title }}
{%- endif %}
{% for annotation in attachment.annotations -%}
{%- if annotation.annotatedText %}

{%- set colorMap = {
  "yellow": "#fff59d",
  "green": "#a5d6a7",
  "blue": "#90caf9",
  "pink": "#f48fb1",
  "red": "#ef9a9a",
  "purple": "#ce93d8",
  "orange": "#ffcc80",
  "gray": "#eeeeee"
} -%}

{%- set hexColor = colorMap[annotation.colorCategory | lower] or "#e0e0e0" -%}

{% if annotation.color %}
<mark style="background-color: {{ hexColor }}">“{{ annotation.annotatedText | safe }}”</mark>{% else %}“{{ annotation.annotatedText | safe }}”
{% endif %} [Pagina {{ annotation.pageLabel }}](zotero://open-pdf/library/items/{{ attachment.itemKey }}?page={{ annotation.pageLabel }}&annotation={{ annotation.id }}){% endif -%}

{%- if annotation.comment %}
💬 **Commento:** {{ annotation.comment | safe }}
{%- endif -%}

{%- if annotation.imageRelativePath %}
![[{{ annotation.imageRelativePath }}]]
👉 [Pagina {{ annotation.pageLabel }}](zotero://open-pdf/library/items/{{ attachment.itemKey }}?page={{ annotation.pageLabel }}&annotation={{ annotation.id }})
{%- endif -%}

{%- if annotation.allTags %}
🏷️ **Tag:** {{ annotation.allTags }}
{%- endif %}
{% endfor -%}
{% endfor -%}

---
## Note

{% for item in notes %}
{{item.note}}
{% endfor %}