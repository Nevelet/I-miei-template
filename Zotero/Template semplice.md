
{% for a in annotations %}
{{a.annotatedText}}
{% endfor %}

## {{title}}

### Bibliografia

{{bibliography}}
{% if abstractNote %}

### Abstract

{{abstractNote}}
{% endif %}