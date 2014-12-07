---
layout: page
title: Greetings, from Michael Ladd!
---
{% include JB/setup %}

## Michael J. Ladd: <a href="/pages/resume.html">Resume</a> | <a href="http://github.com/mjladd">GitHub</a> | <a href="http://soundcloud.com/mjladd">SoundCloud</a> | <a href="https://www.linkedin.com/in/mjladd">LinkedIn</a>

I am Michael J. Ladd (mjladd-at-gmail). I work as a systems administrator for LeapfrogOnline. In a former life I attempted to be a musician. While studying music, I discovered that I was far better at troubleshooting technology than writing fugues and analyzing symphonies. I have a Bachelor of Music degree, a Masters of Music and graduated from the MSIT program at Northwestern University.

    
    
## Posts

The Whole Enchilada ...

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

