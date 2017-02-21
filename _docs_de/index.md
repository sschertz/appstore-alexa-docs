---
title: Nächste Version des Dokumentationsthemas für Jekyll
permalink: index.html
sidebar: null
topnav: topnav_de
toc: false
search: false
layout: homepage
type: homepage
---


<div class="filter-options">
<input type="radio" name="product" data-group="all" checked="checked" />All 
 {% for group in site.data.homepage_products.groups %}
<input type="radio" name="product" data-group="{{group}}" />{{group}} 
{% endfor %}

</div>    

<div id="grid" class="row">

{% assign productList = site.data.homepage_products.products | sort: 'title' %}
{% for product in productList %}

<div class="col-xs-6 col-sm-4 col-md-4" data-groups='["{{product.group}}", "all"]'>
<div class="panel panel-default">
    <div class="panel-heading"><a href="{{product.link}}">{{product.title}}</a></div>
    <div class="panel-body">
        {{product.description}}
        <ul>

        </ul>
    </div>
</div>
</div>
{% endfor %}


  <!-- sizer -->
<div class="col-xs-6 col-sm-4 col-md-1 shuffle_sizer"></div>          


</div><!-- /#grid -->

