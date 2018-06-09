---
layout: new
title: "BA"
description: "Business Administration"  
---
<div class="container articles-page">
    <div class="row">
        <div class="col-xs-12 col-md-12 col-lg-12">
            <div class=" domaker infomation">
                <div class="content text-center">
                    <h1>BA</h1>
                    <p><b>Business Administration · 工商管理学习笔记</b></p>
                </div>
            </div>
            {% for category in site.categories %}
            {% if category[0] == "ba" %}
            {% for post in category[1] %}
            <div class="panel">
                <div class="panel-body">
                    <h1><a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a></h1>
                    <div class="article-attrs">
                        <span class="pull-left">
                            <i class="fa fa-fw fa-calendar"></i> {{ post.date | date: "%Y-%m-%d" }}
                        </span>
                        <span><i class="fa fa-fw fa-tags"></i> 
                            {% for tag in post.tags %}
                                <a href="/2_tags/#{{ tag }}" title="{{ tag }}">{{ tag }}</a>
                            {% endfor %}
                        </span>
                    </div>
                    <p>{{ post.excerpt }}</p>
                </div>
            </div>
            {% endfor %}
            {% endif %}
            {% endfor %}
            <!-- Pager -->
            {% if paginator.total_pages > 1 %}
            <ul class="pager">
                {% if paginator.previous_page %}
                <li class="previous">
                    <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">上一页</a>
                </li>
                {% endif %}
                {% if paginator.next_page %}
                <li class="next">
                    <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">下一页</a>
                </li>
                {% endif %}
            </ul>
            {% endif %}
        </div>
    </div>
</div>