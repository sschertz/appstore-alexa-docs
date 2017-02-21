---
title: Amazon Developer Portalのドキュメント
permalink: index.html
sidebar: null
topnav: topnav_ja
toc: false
search: false
layout: homepage
type: homepage
---


<div class="container">
<div class="row">
<div class="col-sm-3">
<h2>Fire Devices</h2> 
{% assign productList = site.data.homepage_products_ja.products %}
<ul class="homepageLinks">
{% for product in productList %}
    {% if product.group == "Fire Devices" %}
<li><a class="noCrossRef" href="{{ product.link }}">{{ product.title }}</a></li>
    {% endif %}
{% endfor %}
</ul>


</div>
</div>
