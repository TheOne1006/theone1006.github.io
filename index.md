---
layout: default
title: Theone's blog
---

# 首页

# 最新项目

<div class="card-container">
{% for project in site.data.projects %}
  <div class="project-card">
    <a href="{{ project.link }}">
      {% if project.image %}
      <img src="{{ project.image }}" alt="{{ project.title }}">
      {% endif %}
      <div class="card-content">
        <h3>{{ project.title }}</h3>
        <p>{{ project.description }}</p>
      </div>
    </a>
  </div>
{% endfor %}
</div>


<style>
.card-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
  padding: 20px 0;
}

.project-card {
  background: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 5px rgba(0,0,0,0.1);
  overflow: hidden;
  transition: transform 0.3s ease;
}

.project-card:hover {
  transform: translateY(-5px);
}

.project-card img {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-content {
  padding: 15px;
}

.card-content h3 {
  margin: 0 0 10px 0;
}

.card-content p {
  color: #666;
  margin: 0;
}

.project-card a {
  text-decoration: none;
  color: #333;
}

.project-card a:hover {
  color: #0366d6;
}
</style>
