---
layout: default
title: My Space
permalink: /personal/
nav: true
nav_order: 5
display_categories: [Photography, Personal Blogs]
---

<div class="my-space">
  <h1>{{ page.title }}</h1>
  <p>Welcome to My Space! Explore content by category below.</p>

  {% if page.display_categories %}
    <!-- Display categorized content -->
    {% for category in page.display_categories %}
      <a id="{{ category }}" href="#{{ category | slugify }}">
        <h2 class="category">{{ category }}</h2>
      </a>
      {% assign categorized_posts = site.posts | where: "categories", category %}
      {% assign sorted_posts = categorized_posts | sort: "date" %}
      
      <div class="category-content">
        {% if category == "Photography" %}
          <!-- Display all images for Photography category -->
          <div class="pictures">
            {% for image in site.static_files %}
              {% if image.path contains 'assets/img/photography' and image.extname == '.jpg' or image.extname == '.jpeg' or image.extname == '.png' or image.extname == '.gif' %}
                <div class="picture-card">
                  <img src="{{ image.path | relative_url }}" alt="{{ image.name }}" onclick="openModal(this)">
                </div>
              {% endif %}
            {% endfor %}
          </div> 
        {% else %}

          <!-- Display posts for other categories -->
          <div class="row row-cols-1 row-cols-md-3">
            {% for post in sorted_posts %}
              <div class="post-card">
                <a href="{{ post.url | relative_url }}">
                  <h3>{{ post.title }}</h3>
                  <p>{{ post.excerpt }}</p>
                  <p class="meta">Posted on {{ post.date | date: "%B %d, %Y" }}</p>
                </a>
              </div>
            {% endfor %}
          </div>
        {% endif %}
      </div>
    {% endfor %}
  {% else %}
    <p>No categories defined. Please check your configuration.</p>
  {% endif %}
</div>

<!-- The Modal -->
<div id="imageModal" class="modal">
  <span class="close">&times;</span>
  <img class="modal-content" id="modalImage">
</div>

<script>
  function openModal(image) {
    var modal = document.getElementById("imageModal");
    var modalImg = document.getElementById("modalImage");
    modal.style.display = "block";
    modalImg.src = image.src;
  }

  // Close the modal
  var modal = document.getElementById("imageModal");
  modal.onclick = function () {
    modal.style.display = "none";
  };
</script>
