---
title: "{{title | escape}}"
year: {{date | format("YYYY")}}
authors: {{authors}}
tags: [{% for t in tags %}{{t.tag}}{% if not loop.last %}, {% endif %}{% endfor %}]
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
{% if annotation.color %}<mark style="background-color: {{ annotation.colorCategory | lower }}">“{{ annotation.annotatedText | safe }}”</mark>{% else %}“{{ annotation.annotatedText | safe }}”
{% endif %} [Pagina{{ annotation.pageLabel }}](zotero://open-pdf/library/items/{{ attachment.itemKey }}?page={{ annotation.pageLabel }}&annotation={{ annotation.id }}){% endif -%}

{%- if annotation.comment %}
💬 **Commento:** {{ annotation.comment | safe }}
{%- endif -%}

{%- if annotation.imageRelativePath %}
![[{{ annotation.imageRelativePath }}]]
👉 [Pagina {{ annotation.pageLabel }}](zotero://open-pdf/library/items/{{ attachment.itemKey }}?page={{ annotation.pageLabel }}&annotation={{ annotation.id }})
{% endif -%}

{%- if annotation.allTags %}
🏷️ **Tag:** {{ annotation.allTags }}
{% endif %}
{% endfor -%}
{% endfor -%}

## Note

{% for item in notes %}
{{item.note}}
{% endfor %}