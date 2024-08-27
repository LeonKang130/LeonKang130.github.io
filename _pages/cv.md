---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* M.S. in Computer Science, University of California, Santa Barbara, 2026 (expected)
* B.E. in Computer Science and Technology, Tsinghua University, 2024
  
Skills
======
* C++
* Python
* OpenGL
* CUDA

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
