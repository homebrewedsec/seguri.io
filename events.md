---
title: Upcoming Events
layout: splash
permalink: /events/
classes: wide
description: "Stay informed on upcoming cyber security events hosted by Seguri. Get insights, network with industry professionals, and stay ahead in cyber defense."
header:
  teaser: /assets/images/home-header.webp
---

{% assign current_date = site.time | date: "%Y-%m-%d" %}
{% assign upcoming_events = site.events | where_exp: "event", "event.event_date" | where_exp: "event", "event.event_date >= current_date" | sort: "event_date" %}

<div class="grid__wrapper">
  {% for event in upcoming_events %}
    <div class="grid__item">
      <article class="archive__item" itemscope itemtype="https://schema.org/CreativeWork">
        {% if event.header.teaser %}
          <div class="archive__item-teaser">
            <img src="{{ event.header.teaser | relative_url }}" alt="{{ event.title | escape }}">
          </div>
        {% endif %}
        <h2 class="archive__item-title no_toc" itemprop="headline">
          <a href="{{ event.url | relative_url }}" rel="permalink">{{ event.title }}</a>
        </h2>
        {% if event.event_date %}
          <p class="archive__item-date">
            <i class="fas fa-calendar-alt" aria-hidden="true"></i>
            <time datetime="{{ event.event_date | date_to_xmlschema }}">{{ event.event_date | date: "%B %-d, %Y" }}</time>
          </p>
        {% endif %}
        {% if event.location %}
          <p class="archive__item-location">
            <i class="fas fa-map-marker-alt" aria-hidden="true"></i>
            {{ event.location }}
          </p>
        {% endif %}
        {% if event.excerpt %}
          <p class="archive__item-excerpt" itemprop="description">{{ event.excerpt | markdownify | strip_html | truncate: 160 }}</p>
        {% endif %}
      </article>
    </div>
  {% endfor %}
</div>

{% if upcoming_events.size == 0 %}
  <div style="text-align: center; padding: 2rem; color: rgba(255, 255, 255, 0.7);">
    <i class="fas fa-calendar-times" style="font-size: 3rem; margin-bottom: 1rem; color: #9aceeb;"></i>
    <h3>No Upcoming Events</h3>
    <p>Check back soon for new events and conference announcements.</p>
  </div>
{% endif %}
