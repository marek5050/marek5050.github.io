---
layout: page
title: Resume
permalink: /resume/
---



## Work Experience
{% for item in site.my_resume %}
{% if item.type == "Work Experience" %}
**{{item.subheading}}** at [{{item.heading}}]({{item.link}}) <span class='pull-right'>( {{item.duration}} , {{item.location}})</span>


{{item.content}}


{% endif %}
{% endfor %}

## Personal Projects
{% for item in site.my_resume %}
{% if item.type == "Personal Project" %}

**[{{item.heading}}]({{item.link}})** -- {{item.subheading}}

{{item.content}}
{% endif %}
{% endfor %}

## Education
{% for item in site.my_resume %}
{% if item.type == "Education" %}
**{{item.subheading}}** <span class='pull-right'>( {{item.duration}} , {{item.location}})</span>

[{{item.heading}}]({{item.link}})

{{item.content}}
{% endif %}
{% endfor %}



This is the base Jekyll theme. You can find out more info about customizing your Jekyll theme, as well as basic Jekyll usage documentation at [jekyllrb.com](http://jekyllrb.com/)

You can find the source code for the Jekyll new theme at: [github.com/jglovier/jekyll-new](https://github.com/jglovier/jekyll-new)

You can find the source code for Jekyll at [github.com/jekyll/jekyll](https://github.com/jekyll/jekyll)
