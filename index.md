---
layout: home
---

## My Projects

### Personal All-in-one Site (2016.07-)
My all-in-one site adopting techniques such as Python Flask, Node, D3.js and a lot of others under link <http://www.johannhuang.com>.
<iframe width="800" height="450" src="http://www.johannhuang.com/" frameborder="0" allowfullscreen></iframe>
*This iframe is scrollable*

### Personal Photography Site (2016.07-)
My photography site, new version adopts Python as back-end language and also as Photo Processing language under <http://www.rhophotos.com>.
<iframe width="800" height="450" src="http://www.rhophotos.com/" frameborder="0" allowfullscreen></iframe>
*This iframe is scrollable*


### MarkDown Blog (2016.11-)
For the simplicity of writing and sharing, I made a MarkDown blog to replace this blog site under link <http://articles.johannhuang.com>.
<iframe width="800" height="450" src="http://articles.johannhuang.com/" frameborder="1" allowfullscreen></iframe>
*This iframe is scrollable*

### Wechat Project (2015-2016)
Wechat Official Account Development (CLI Development) under link <http://weixin.ijohann.com>. One of my wechat official accounts can be accessed via following QR code.  
![Scan to Subscribe](/assets/images/wx-api/zwxx.jpg)
*Scan with Wechat to Subscribe*  

- Response to specific keywords
- Employed a robot api for user to chat with
- Get random jokes after each trigger
- Wechat customized H5 page with user authentication


### Android Project (2013)
- [Keep Fit (Android Phone App)](https://github.com/johannhuang/andriod-keepfit)

### iOS Projects (2012 - 2013)
- [iHome(iPhone App)](https://www.youtube.com/embed/LcO6RqZ2LnY)
<iframe width="800" height="600" src="https://www.youtube.com/embed/LcO6RqZ2LnY" frameborder="0" allowfullscreen></iframe>
- [Story of Effendi(iPad App)](https://youtu.be/gCIzlXu7pn4)
<iframe width="800" height="600" src="https://www.youtube.com/embed/gCIzlXu7pn4" frameborder="0" allowfullscreen></iframe>

<hr>

## Recent Posts

**My recent posts are published on another site under <http://articles.johannhuang.com>.**

<div class="posts">
  {% assign count = 0 %}
  {% for post in site.posts %}
	{% if post.list != 'hide' %}
	{% assign count = count | plus: 1 %}
		<div>
			<h4>
				<a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
				<span style="font-size: 0.7em;">&raquo; {{ post.date | date: "%Y-%m-%d" }}</span>
			</h4>
			<p>{{ post.excerpt }}</p>
		</div>
	{% endif %}
	{% if count == 3 %} {% break %} {% endif %}
  {% endfor %}
</div>
