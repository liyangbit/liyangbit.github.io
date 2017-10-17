---
layout: page
title: "博客文章分类"
meta_title: "Categories"
show_meta: false
teaser:
header:
   image_fullwidth: "wood_plank.jpg"
permalink: "/categories/"
---

<section class="container posts-content">
{% assign sorted_categories = site.categories | sort %}
{% for category in sorted_categories %}
<h3>{{ category | first }}</h3>
<ol class="posts-list" id="{{ category[0] }}">
{% for post in category.last %}
<li class="posts-list-item">
<span class="posts-list-meta">{{ post.date | date:"%Y-%m-%d" }}</span>
<a class="posts-list-name" href="{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ol>
{% endfor %}
</section>





<!-- ---
layout: default
title: 分类
header: Posts By Category
permalink: categories.html
--- -->

<!-- <div class="container docs-container">
  <div class="row">
    <div class="col-md-3">
      <div class="sidebar hidden-print" role="complementary">
        <div id="navigation">
        	<h1>目录</h1>
      		<ul class="nav sidenav">
				{% if site.categories.first[0] == null %}
					{% for category in site.categories %}
				    	<li><a href="#{{ category }}-ref">
				    		{{ category | join: "/" }} <span style="color: #999999;" >({{ site.categories[category].size }})</span>
				    	</a></li>
			    	{% endfor %}
			  	{% else %}
			    	{% for category in site.categories %}
				    	<li><a href="#{{ category[0] }}-ref">
				    		{{ category[0] | join: "/" }} <span style="color: #999999;" >({{ category[1].size }})</span>
				    	</a></li>
			    	{% endfor %}
			  	{% endif %}
          	</ul>
        </div>
      </div>
    </div>
    <div class="col-md-9" role="main">
      <div class="panel docs-content">
        <div class="wrapper">
          <div class="home">
			{% for category in site.categories %}
			  <h2 id="{{ category[0] }}-ref">{{ category[0] | join: "/" }}</h2>
			  <ul>
			    {% assign pages_list = category[1] %}  
			    {% include LessOrMore/pages_list %}
			  </ul>
			{% endfor %}
          </div>
        </div>
      </div>
    </div>
  </div>
</div> -->
