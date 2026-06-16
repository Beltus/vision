---
layout: archive
permalink: /blog/
author_profile: true
title: "List of Articles"
header:
     image: "https://cdn-images-1.medium.com/max/800/1*dJevYvXpMboY7_OlYmNWQw.jpeg"
layouts_gallery:
  - url: https://beltus.github.io/vision/sc/
    image_path: https://cdn-images-1.medium.com/max/800/1*btSkbsTKQSgTciaJBWL8xw.png
  - url: https://beltus.github.io/vision/ml/
    image_path: https://cdn-images-1.medium.com/max/800/1*2TOOMsFHjdPv0FGnvLF-hQ.jpeg
---

I have only 2 things I can offer you on this blog.  
1. If you're interested in becoming the best version of yourself through continuous self-improvement, then I got you.

2. If you have even the tiniest curiosity for Artificial Intelligence and Machine learning related concepts, then you're at the right place.

## Articles Grouped by Category

<div class="grid__wrapper">
{% include gallery id="layouts_gallery" class="full" layout="half"%}
</div>

## List of all Articles
List of all the articles in this blog is provided below.

<div class="grid__wrapper">
  {% assign collection = 'blog' %}
  {% assign posts = site[collection] | reverse %}
  {% for post in posts %}
    {% include archive-single.html type="grid" %}
  {% endfor %}
</div>

<div style="clear: both;"></div>

<div style="margin-top: 2rem;">
  <p>Don't forget to subscribe and be part of an amazing journey.</p>
  <div id="sender-signup-form"></div>
  <script>
    (function(s,e,n,d,er){
      s['Sender']=er;s[er]=s[er]||function(){(s[er].q=s[er].q||[]).push(arguments)};
      var a=e.createElement(n);var m=e.getElementsByTagName(n)[0];
      a.async=1;a.src=d;m.parentNode.insertBefore(a,m);
    })(window,document,'script','https://cdn.sender.net/accounts_resources/universal.js','sender');
    sender('bootWithTitle', 'aKrjOM');
  </script>
</div>