{%- assign res_data = site.pages | where: 'title', include.res | first -%}
[{{ include.res }} ({{ res_data.summary }})]({{ res_data.url | relative_url }})