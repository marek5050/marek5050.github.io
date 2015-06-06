---
layout: page
title: Resume
permalink: /resume/
---


### Work Experience
{% for item in site.my_resume %}
{% if item.type == "Work Experience" %}
**{{item.subheading}}** at [{{item.heading}}]({{item.link}}) <span class='pull-right'>( {{item.duration}} , {{item.location}})</span>


{{item.content}}


{% endif %}
{% endfor %}

### Personal Projects
{% for item in site.my_resume %}
{% if item.type == "Personal Project" %}

**[{{item.heading}}]({{item.link}})** -- {{item.subheading}}

{{item.content}}
{% endif %}
{% endfor %}

### Education
{% for item in site.my_resume %}
{% if item.type == "Education" %}
**{{item.subheading}}** <span class='pull-right'>( {{item.duration}} , {{item.location}})</span>

[{{item.heading}}]({{item.link}})
{% endif %}
{% endfor %}