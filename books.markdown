---
layout: page
title: "Libros"
permalink: /libros/
---

Aquí comparto reseñas y reflexiones de los libros que voy leyendo. 📚

<ul>
  {% for libro in site.libros %}
    <li>
      <a href="{{ libro.url }}">{{ libro.title }}</a>
      <small>— {{ libro.date | date: "%B %d, %Y" }}</small>
    </li>
  {% endfor %}
</ul>
