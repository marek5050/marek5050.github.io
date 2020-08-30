---
layout: resume
title: Resume
permalink: /resume/
---

##### Marek Bejda <small>![email](/static/emails.png){:.some-css-class style="width: 20px"}  marek.bejda@gmail.com ![email](/static/github.jpeg){:.some-css-class style="width: 20px"}  github.com/marek5050 ![email](/static/blog.svg){:.some-css-class.icon-linkedin style="width: 18px"} linkedin.com/in/marek5050 </small>

----
<style  media="print">
.container{
        width:100%;
        margin:0px!important;
        padding:0px!important;
        max-width:100%;
}
small{
    font-size:60%;
}
p{
margin-bottom:0px;
font-size:90%;
}
ul{
margin-bottom:5px;
}
hr{
margin:7px 0px;
}

h4{
font-weight:700;
font-size:95%;
 margin-bottom:2px;
}

</style>
<style>
.pull-right{
float:right}
h4{
font-weight:700;
}
strong{
font-weight:600;
}
</style>
#### Work Experience

{% for item in site.my_resume %}
{% if item.type == "Work Experience" %}
**{{item.subheading}}** at [{{item.heading}}]({{item.link}}){: target="_blank"} <span class='pull-right'><small>( {{item.duration}}, {{item.location}})</small></span>
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
**{{item.heading}} from {{item.subheading}}** <span class='pull-right'><small>( GPA {{item.gpa}}, {{item.duration}}, {{item.location}})</small></span>

<!-- [{{item.heading}}]({{item.link}}){: target="_blank"} -->
{% endif %}
{% endfor %}

----

#### Other

{% for item in site.my_resume %}
{% if item.type == "Report" %}
[{{item.heading}}]({{item.link}}){: target="_blank"}
<span class='pull-right'><small>( {{item.duration}}, {{item.location}})</small></span>

{{item.content}}

{% endif %}
{% endfor %}

----

#### Skills
**Languages**	Java, Javascript, Python, Go 
**Frameworks**	Spring, jUnit, Pytest, Pandas
**Systems** 	Kubernetes, AWS, Linux, Docker
**Tools**		IntelliJ, JIRA, TravisCI, git, vim
**Data**        Tableau, PostgreSQL, MongoDB, ELK
**Other**       DNS, Apache, nginx

<span class="pull-right">An interactive version of this resume can be found at marek5050.github.io/resume</span>

