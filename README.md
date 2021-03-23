# starter

1. [Generate with the same files and folders](https://github.com/rundocs/starter/generate) from this repository
2. Set up your GitHub Pages to source(`/`)
3. Now you can view your documentation in your site

## site.pages

<!-- prettier-ignore-start -->

| source          | link                                                           |
| --------------- | -------------------------------------------------------------- |
{% for page in site.pages -%}
| {{ page.path }} | [{{ page.url | relative_url }}]({{ page.url | relative_url }}) |
{% endfor %}

<!-- prettier-ignore-end -->


