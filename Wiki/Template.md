---
year: {{date | format("YYYY")}}
tags: note/source
authors: {{authors}}{{directors}}
---

Title:: {{title}}
URL: {{url}}
Zotero Link: {{pdfZoteroLink}}

{% for annotation in annotations %}
	{%- if annotation.annotatedText -%}
		{{annotation.annotatedText}}
	{% endif %}
	{% if annotation.comment %}
		{{annotation.comment}}
	{% endif %}
{% endfor %}

---

{{bibliography}}
{% if abstractNote %}