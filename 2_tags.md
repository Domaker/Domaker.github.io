---
layout: new
title: "Tags"
description: ""
---
<div class="container tags-page">
	<div class="row">
		<div class="col-xs-12 col-md-8">
		  	{% for tag in site.tags %}
			  	<div id="{{ tag[0] }}">
					<div class="panel panel-info">
						<div class="panel-heading">
							<span>
								{{ tag[0] }} 
							</span>
						</div>
						<div class="panel-body">
							{% for post in tag[1] %}
							<div class="article-li">
								<a href="{{ post.url }}">
								<span style="display: inline-block;">
									{{ post.title }} -- {{ post.date | date: "%Y-%m-%d" }}
								</span>
								</a>
							</div>
							{% endfor %}
						</div>
					</div>
				</div>
			{% endfor %}
		</div>
		<div class="col-md-4">
			<div class="list-group">
				{% for tag in site.tags %}
				  	<a href="#{{ tag[0] }}" class="list-group-item">
				    	{{ tag[0] }} 
				    	<span class="label label-info pull-right">{{ tag[1].size }}</span>
				  	</a>
			  	{% endfor %}
			</div>
		</div>
	</div>
</div>

