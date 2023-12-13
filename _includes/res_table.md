{%- capture res_header_description -%}
{%- if site.active_lang == "es" -%}
Descripción
{%- else -%}
Description
{%- endif -%}
{%- endcapture -%}
{%- assign res_arr = include.resources | split: "," -%}

| ID | {{ res_header_description }} |
|:---|:------------|
{% for res in res_arr %}{% assign res_data = site.pages | where: 'title', res | first %}| [{{ res }}]({{ res_data.url | relative_url }}) | {{ res_data.summary }} |
{% endfor %}
