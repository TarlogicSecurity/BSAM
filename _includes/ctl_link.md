{%- assign ctl_data = site.pages | where: 'title', include.ctl | first -%}
[{{ include.ctl }} ({{ ctl_data.summary }})]({{ ctl_data.url | relative_url }})