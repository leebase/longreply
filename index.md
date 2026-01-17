---
layout: default
title: "Index"
permalink: /
---

# Index

Use the buttons to switch between A–Z and newest-to-oldest sorting. Folders expand to show their pages.

Pages sort by the `date` field in front matter. Add `date: YYYY-MM-DD` to control newest-to-oldest order.

<div class="index-controls">
  <button type="button" class="index-toggle" data-sort="alpha" aria-pressed="true">
    A–Z
  </button>
  <button type="button" class="index-toggle" data-sort="date" aria-pressed="false">
    Newest → Oldest
  </button>
</div>

{% assign docs = site.collections.pages.docs %}
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

<div id="index-alpha" class="index-section">
  {% assign alpha_entries = "" | split: "" %}
  {% for dir_name in dir_names %}
    {% assign entry = dir_name | append: "||dir" %}
    {% assign alpha_entries = alpha_entries | push: entry %}
  {% endfor %}
  {% for doc in root_docs %}
    {% assign relative_path = doc.path | remove_first: "_pages/" %}
    {% assign entry = relative_path | append: "||file||" | append: doc.url %}
    {% assign alpha_entries = alpha_entries | push: entry %}
  {% endfor %}
  {% assign alpha_entries = alpha_entries | sort %}

  <ul>
    {% for entry in alpha_entries %}
      {% assign parts = entry | split: "||" %}
      {% assign name = parts[0] %}
      {% assign kind = parts[1] %}
      {% assign url = parts[2] %}
      {% if kind == "dir" %}
        <li>
          <details>
            <summary>{{ name }}</summary>
            {% assign dir_entries = "" | split: "" %}
            {% for doc in docs %}
              {% assign relative_path = doc.path | remove_first: "_pages/" %}
              {% assign path_parts = relative_path | split: "/" %}
              {% if path_parts.size > 1 and path_parts[0] == name %}
                {% assign dir_entry = relative_path | append: "||" | append: doc.url %}
                {% assign dir_entries = dir_entries | push: dir_entry %}
              {% endif %}
            {% endfor %}
            {% assign dir_entries = dir_entries | sort %}
            <ul>
              {% for dir_entry in dir_entries %}
                {% assign dir_parts = dir_entry | split: "||" %}
                {% assign relative_path = dir_parts[0] %}
                {% assign item_url = dir_parts[1] %}
                {% assign label = relative_path | remove_first: name | remove_first: "/" | replace: ".md", "" %}
                <li><a href="{{ item_url | relative_url }}">{{ label }}</a></li>
              {% endfor %}
            </ul>
          </details>
        </li>
      {% else %}
        {% assign label = name | replace: ".md", "" %}
        <li><a href="{{ url | relative_url }}">{{ label }}</a></li>
      {% endif %}
    {% endfor %}
  </ul>
</div>

<div id="index-date" class="index-section" hidden>
  {% assign date_entries = "" | split: "" %}

  {% for dir_name in dir_names %}
    {% assign dir_dates = "" | split: "" %}
    {% for doc in docs %}
      {% assign relative_path = doc.path | remove_first: "_pages/" %}
      {% assign path_parts = relative_path | split: "/" %}
      {% if path_parts.size > 1 and path_parts[0] == dir_name %}
        {% assign doc_date = doc.date | date: "%Y-%m-%d %H:%M:%S" | default: "0000-00-00 00:00:00" %}
        {% assign dir_dates = dir_dates | push: doc_date %}
      {% endif %}
    {% endfor %}
    {% assign dir_dates = dir_dates | sort %}
    {% assign dir_date = dir_dates | last | default: "0000-00-00 00:00:00" %}
    {% assign entry = dir_date | append: "||" | append: dir_name | append: "||dir" %}
    {% assign date_entries = date_entries | push: entry %}
  {% endfor %}

  {% for doc in root_docs %}
    {% assign relative_path = doc.path | remove_first: "_pages/" %}
    {% assign doc_date = doc.date | date: "%Y-%m-%d %H:%M:%S" | default: "0000-00-00 00:00:00" %}
    {% assign entry = doc_date | append: "||" | append: relative_path | append: "||file||" | append: doc.url %}
    {% assign date_entries = date_entries | push: entry %}
  {% endfor %}

  {% assign date_entries = date_entries | sort | reverse %}

  <ul>
    {% for entry in date_entries %}
      {% assign parts = entry | split: "||" %}
      {% assign date_key = parts[0] %}
      {% assign name = parts[1] %}
      {% assign kind = parts[2] %}
      {% assign url = parts[3] %}
      {% if kind == "dir" %}
        <li>
          <details>
            <summary>{{ name }} <span>({{ date_key | date: "%Y-%m-%d" }})</span></summary>
            {% assign dir_entries = "" | split: "" %}
            {% for doc in docs %}
              {% assign relative_path = doc.path | remove_first: "_pages/" %}
              {% assign path_parts = relative_path | split: "/" %}
              {% if path_parts.size > 1 and path_parts[0] == name %}
                {% assign doc_date = doc.date | date: "%Y-%m-%d %H:%M:%S" | default: "0000-00-00 00:00:00" %}
                {% assign dir_entry = doc_date | append: "||" | append: relative_path | append: "||" | append: doc.url %}
                {% assign dir_entries = dir_entries | push: dir_entry %}
              {% endif %}
            {% endfor %}
            {% assign dir_entries = dir_entries | sort | reverse %}
            <ul>
              {% for dir_entry in dir_entries %}
                {% assign dir_parts = dir_entry | split: "||" %}
                {% assign item_date = dir_parts[0] %}
                {% assign relative_path = dir_parts[1] %}
                {% assign item_url = dir_parts[2] %}
                {% assign label = relative_path | remove_first: name | remove_first: "/" | replace: ".md", "" %}
                <li><a href="{{ item_url | relative_url }}">{{ label }}</a> <span>({{ item_date | date: "%Y-%m-%d" }})</span></li>
              {% endfor %}
            </ul>
          </details>
        </li>
      {% else %}
        {% assign label = name | replace: ".md", "" %}
        <li><a href="{{ url | relative_url }}">{{ label }}</a> <span>({{ date_key | date: "%Y-%m-%d" }})</span></li>
      {% endif %}
    {% endfor %}
  </ul>
</div>

<script>
  (function () {
    const buttons = document.querySelectorAll('.index-toggle');
    const sections = {
      alpha: document.getElementById('index-alpha'),
      date: document.getElementById('index-date')
    };

    const setView = (mode) => {
      Object.keys(sections).forEach((key) => {
        sections[key].hidden = key !== mode;
      });

      buttons.forEach((button) => {
        button.setAttribute('aria-pressed', button.dataset.sort === mode);
      });
    };

    buttons.forEach((button) => {
      button.addEventListener('click', () => setView(button.dataset.sort));
    });

    setView('alpha');
  })();
</script>
