---
layout: page
title:  "Food Blog"
bigimg: 
  - "/image/foodblog/biryani1.jpg" : "Red Chicken Briyani"
  - "/image/foodblog/chicken2.jpg" : "Chicken Roast and Curry"
  - "/image/foodblog/dinner1.jpg" : "Food Feast"
  - "/image/foodblog/khasi1.jpg" : "Goat Curry"
  - "/image/foodblog/khasta1.jpg" : "Khasta"
  - "/image/foodblog/lalmohan1.jpg" : "Gulab Jamun"
  - "/image/foodblog/momo1.jpg" : "Mo:Mo: Dumplings"
  - "/image/foodblog/momo2.jpg" : "Mo:Mo: Dumplings"
  - "/image/foodblog/momo3.jpg" : "Mo:Mo: Dumplings"
  - "/image/foodblog/pizza1.jpg" : "Homemade Pizzzza"
  - "/image/foodblog/salad1.jpg" : "Salad"
  - "/image/foodblog/samosa1.jpg" : "SAMOSAA"
---


<!-- site body (untouched)--> 
<div class="posts-list">
  {% for post in paginator.posts %}
  <article class="post-preview">
    <a href="{{ post.url | prepend: site.baseurl }}">
    <h2 class="post-title">{{ post.title }}</h2>

    {% if post.subtitle %}
    <h3 class="post-subtitle">
      {{ post.subtitle }}
    </h3>
    {% endif %}
    </a>

    <p class="post-meta">
      Posted on {{ post.date | date: "%B %-d, %Y" }}
    </p>

    <div class="post-entry-container">
      {% if post.image %}
      <div class="post-image">
        <a href="{{ post.url | prepend: site.baseurl }}">
          <img src="{{ post.image }}">
        </a>
      </div>
      {% endif %}
      <div class="post-entry">
        {{ post.excerpt | strip_html | xml_escape | truncatewords: site.excerpt_length }}
        {% assign excerpt_word_count = post.excerpt | number_of_words %}
        {% if post.content != post.excerpt or excerpt_word_count > site.excerpt_length %}
          <a href="{{ post.url | prepend: site.baseurl }}" class="post-read-more">[Read&nbsp;More]</a>
        {% endif %}
      </div>
    </div>
   
  
    {% if post.tags.size > 0 %}
    <div class="blog-tags">
      Tags:
      {% if site.link-tags %}
      {% for tag in post.tags %}
      <a href="{{ site.baseurl }}/tags#{{- tag -}}">{{- tag -}}</a>
      {% endfor %}
      {% else %}
        {{ post.tags | join: ", " }}
      {% endif %}
    </div>
    {% endif %}

   </article>
  {% endfor %}


{% if paginator.total_pages > 1 %}
<ul class="pager main-pager">
  {% if paginator.previous_page %}
  <li class="previous">
    <a href="{{ paginator.previous_page_path | prepend: site.baseurl | replace: '//', '/' }}">&larr; Newer Posts</a>
  </li>
  {% endif %}
  {% if paginator.next_page %}
  <li class="next">
    <a href="{{ paginator.next_page_path | prepend: site.baseurl | replace: '//', '/' }}">Older Posts &rarr;</a>
  </li>
  {% endif %}
</ul>
{% endif %}
<style>
div.gallery {
  border: 1px solid #ccc;
}

div.gallery:hover {
  border: 1px solid #777;
}

div.gallery img {
  width: 100%;
  height: 100%;
}

div.desc {
  padding: 15px;
  text-align: center;
}

* {
  box-sizing: border-box;
}



.responsive {
  padding: 0 6px;
  float: left;
  width: 24.99999%;
}

@media only screen and (max-width: 700px) {
  .responsive {
    width: 49.99999%;
    margin: 6px 0;
  }
}

@media only screen and (max-width: 500px) {
  .responsive {
    width: 100%;
  }
}

.clearfix:after {
  content: "";
  display: table;
  clear: both;
}
</style>
</head>
<body>

<center> <h2>Welcome to Our Food Blog</h2> </center>
<p>In this section of the website, I will be sharing traditional Nepalese, Indian and American style cooking that from my mom, wife and myself.</p>

  <hr>
  
<div class="responsive">
  <div class="gallery">
    <a target="_blank" href="/image/foodblog/biryani1.jpg">
      <img src="/image/foodblog/biryani1.jpg" alt="Cinque Terre" width="600" height="400">
    </a>
    <div class="desc">Red Chicken Biryani</div>
  </div>
</div>


<div class="responsive">
  <div class="gallery">
    <a target="_blank" href="/image/foodblog/momo1.jpg">
      <img src="/image/foodblog/momo1.jpg" alt="Forest" width="600" height="400">
    </a>
    <div class="desc">Nepalese Mo:Mo: dumplings</div>
  </div>
</div>

<div class="responsive">
  <div class="gallery">
    <a target="_blank" href="/image/foodblog/samosa1.jpg">
      <img src="/image/foodblog/samosa1.jpg" alt="Northern Lights" width="600" height="400">
    </a>
    <div class="desc">Samosa</div>
  </div>
</div>

<div class="responsive">
  <div class="gallery">
    <a target="_blank" href="/image/foodblog/pizza1.jpg">
      <img src="/image/foodblog/pizza1.jpg" alt="Mountains" width="600" height="400">
    </a>
    <div class="desc">Vegetarian cheese pizza</div>
  </div>
</div>

<div class="clearfix"></div>

<div style="padding:6px;">
  <p> Recepies coming soon...</p>
</div>

