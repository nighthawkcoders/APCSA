---
layout: page
permalink: /schedule2/
title: Schedule2
search_exclude: true
---

{% assign apcsa = "https://nighthawkcoders.github.io/APCSA" %}
{% assign issues = "https://github.com/nighthawkcoders/APCSA/issues" %}
{% assign code-org = "https://studio.code.org/courses/csa-2022?section_id=4160330" %}
{% assign canvas = "https://poway.instructure.com/courses/127262" %}
{% assign ap-class = "https://apclassroom.collegeboard.org/8/home" %}

## Introduction to Tools and Resources
> To learn Java and build skills for Career Technical Education students will quickly immerse into Tools and Resources for Java Development and Fastpages Blogging.  The early weeks will focus on the Development Environment, Fastpages Blogging platform, Code.org resources, AP Classroom resources, and Programming Java with Jupyter Notebooks.

<table>
{% assign intro = null | compact %}

{% for i in (0..5) -%}
  {% assign pt = "" | split:"" %}

  {% assign ap = null | compact %}
  {% assign tt = null | compact %}
  {% assign hm = null | compact %}
  {% assign uk = null | compact %}

  {% for post in site.posts %}
    {% assign week = post.week | plus: 0 %}
    {% assign title = post.title | compact %}
    {% assign url = post.url | compact %}

    {% if i == week %} 
      {% if post.type == "plan" %}
          {% assign pt = pt | push: title %}
          {% assign pt = pt | push: url %}
      {% elsif post.type == "ap" %}
          {% assign ap = ap | concat:title | concat:url  %}
      {% elsif post.type == "pbl" %}
          {% assign tt = tt | concat:title | concat:url  %}
      {% elsif post.type == "human" %}
          {% assign hm = hm | concat:title | concat:url  %}
      {% else %}
          {% assign uk = uk | concat:title | concat:url  %}
      {% endif %}
    {% endif %}
  {% endfor %}

  {% assign intro = intro | concat:pt | concat:ap | concat:tt | concat:hm %}

  <tr>
  <td> {{i}} </td> 
  <td> <a href="{{site.baseurl}}/{{pt[1]}}">{{pt[0]}}</a> <br/> </td>
  <td> {%for a in ap%} {{a}} <br/> {% endfor %} </td>
  <td> {%for t in tt%} {{t}} <br/> {% endfor %} </td>
  <td> {%for h in hm%} {{h}} <br/> {% endfor %} </td>
  </tr>

{% endfor %}

</table>


{% comment %}

  Title : {{post.title}}, Categories : {{post.categories}}
  {% if post.tags.size > 0 %} 
    Tags :
    {% for tag in post.tags %}
    {{tag}} 
    {% endfor %}
  {% endif %}
  
    URL : {{ post.url }}, 
    Date : {{ post.date | date: date_format }}
    Page : {{ post.content }}

{% endcomment %}