---
layout: default
title: "Site Menu"
permalink: /menu
---

# Site Menu

Welcome to the site menu. Below is a structured directory of all published pages.

{% assign docs = site.collections["pages"].docs %}
{% if docs == nil or docs == empty %}
  {% assign docs = site.pages %}
{% endif %}

{% assign filtered_docs = "" | split: "" %}
{% for doc in docs %}
  {% if doc.path contains "_pages/" %}
    {% if doc.url != "/menu" and doc.url != "/menu/" %}
      {% assign filtered_docs = filtered_docs | push: doc %}
    {% endif %}
  {% endif %}
{% endfor %}
{% assign docs = filtered_docs %}

{% assign dir_names = "" | split: "" %}
{% assign root_docs = "" | split: "" %}

{% for doc in docs %}
  {% assign relative_path = doc.path | remove_first: "_pages/" %}
  {% assign parts = relative_path | split: "/" %}
  {% if parts.size > 1 %}
    {% assign dir_name = parts[0] %}
    {% unless dir_names contains dir_name %}
      {% assign dir_names = dir_names | push: dir_name %}
    {% endunless %}
  {% else %}
    {% assign root_docs = root_docs | push: doc %}
  {% endif %}
{% endfor %}

{% assign dir_names = dir_names | sort %}

## Categories & Folders

{% for dir_name in dir_names %}
### {{ dir_name | replace: "-", " " | capitalize }}
{% assign dir_docs = "" | split: "" %}
{% for doc in docs %}
  {% assign relative_path = doc.path | remove_first: "_pages/" %}
  {% assign path_parts = relative_path | split: "/" %}
  {% if path_parts.size > 1 and path_parts[0] == dir_name %}
    {% assign dir_docs = dir_docs | push: doc %}
  {% endif %}
{% endfor %}
{% assign sorted_dir_docs = dir_docs | sort: "title" %}
<ul>
{% for doc in sorted_dir_docs %}
  <li><a href="{{ doc.url | relative_url }}">{{ doc.title | default: doc.name }}</a></li>
{% endfor %}
</ul>
{% endfor %}

---

## Published Articles & Pages

{% assign sorted_root_docs = root_docs | sort: "title" %}
<ul>
{% for doc in sorted_root_docs %}
  <li><a href="{{ doc.url | relative_url }}">{{ doc.title | default: doc.name }}</a></li>
{% endfor %}
</ul>
