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


{%-
    set zoteroColors = {
        "#2ea8e5": "blue",
        "#5fb236": "green",
        "#a28ae5": "purple",
        "#ff6666": "red",
        "#ffd400": "yellow",
        "#f19837": "orange",
        "#aaaaaa": "grey",
        "#e56eee": "magenta"
    }
-%}

{%-
   set colorHeading = {
		"blue": "Blu",
		"green": "Verde",
		"purple": "Viola",
		"red": "Rosso",
		"yellow": "Giallo",
		"orange": "Arancione",
		"grey": "Grigio",
		"magenta": "Magenta"
   }
-%}

{%- set newAnnot = [] -%}
{%- set newAnnotations = [] -%}
{%- set annotations = annotations | filterby("date", "dateafter", lastImportDate) %}

{%- for annot in annotations -%}
    {%- if annot.color in zoteroColors -%}
        {%- set customColor = zoteroColors[annot.color] -%}
    {%- elif annot.colorCategory|lower in colorHeading -%}
    	{%- set customColor = annot.colorCategory|lower -%}
    {%- else -%}
	    {%- set customColor = "other" -%}
    {%- endif -%}
    {%- set newAnnotations = (newAnnotations.push({"annotation": annot, "customColor": customColor}), newAnnotations) -%}
{%- endfor -%}

{%- for color, heading in colorHeading -%}
{%- for entry in newAnnotations | filterby ("customColor", "startswith", color) -%}
{%- set annot = entry.annotation -%}

{%- if entry and loop.first %}
## <mark style="background-color: {{color}}">{{colorHeading[color]}}</mark>
{%- endif %}
{% if annot.annotatedText %}
{{annot.annotatedText | safe}} {% if annot.hashTags %}{{annot.hashTags}}{% endif -%}
([Page {{annot.page}}]({{annot.desktopURI}})) {% endif -%} 

{%- if annot.imageRelativePath %}
![[{{annot.imageRelativePath}}]] ([Page {{annot.page}}]({{annot.desktopURI}}))
{%- endif %}

{%- if annot.ocrText %}
{{annot.ocrText}}
{%- endif %}

{%- if annot.comment %}
- **{{annot.comment}}**
{%- endif %}

{%- endfor %}
{% endfor %}
