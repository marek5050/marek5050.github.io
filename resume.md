---
layout: page
title: 
permalink: /resume/
---

###Marek Bejda <small>![email](/static/emails.png){:.some-css-class style="width: 20px"}  marek.bejda@gmail.com ![email](/static/github.jpeg){:.some-css-class style="width: 20px"}  github.com/marek5050 ![email](/static/twitter.jpg){:.somes_stu style="width: 20px"}@marek5050 ![email](/static/wordpress.png){:.some-css-class style="width: 20px"} AModernStory.com </small>

----

#### Work Experience

{% for item in site.my_resume %}
{% if item.type == "Work Experience" %}
**{{item.subheading}}** at [{{item.heading}}]({{item.link}}){: target="_blank"} <span class='pull-right'>( {{item.duration}} , {{item.location}})</span>

{{item.content}}


{% endif %}
{% endfor %}

----

#### Personal Projects

{% for item in site.my_resume %}
{% if item.type == "Personal Project" %}

**[{{item.heading}}]({{item.link}}){: target="_blank"}** -- {{item.subheading}}

{{item.content}}
{% endif %}
{% endfor %}

----

#### Education

{% for item in site.my_resume %}
{% if item.type == "Education" %}
**{{item.subheading}}** <span class='pull-right'>( {{item.duration}} , {{item.location}})</span>

[{{item.heading}}]({{item.link}}){: target="_blank"}
{% endif %}
{% endfor %}

----

#### Reports

{% for item in site.my_resume %}
{% if item.type == "Report" %}
<span class='pull-right'>( {{item.duration}} , {{item.location}})</span>

[{{item.heading}}]({{item.link}}){: target="_blank"}
{{item.content}}

{% endif %}
{% endfor %}
