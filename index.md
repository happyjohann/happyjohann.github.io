---
layout: page
name: Johann Huang
---

# [Johann Huang](http://i.ijohann.com)

## Education
Since August 2016,  
being a *Master Program Exchange Student*,  
in *[School of Business, Economics and Law](http://handels.gu.se/english)*,  
**[University of Gothenburg, Sweden](http://www.gu.se/)**.  

## Experiences

### Personal All-in-one Site (2016.07-)
My all-in-one site adopting techniques such as Python Flask, Node, D3.js and a lot of others under link <http://www.ijohann.com>.
<iframe width="800" height="450" src="http://www.ijohann.com/sitemap.html" frameborder="0" allowfullscreen></iframe>
*This iframe is scrollable*

### Personal Photography Site (2016.07-)
My photography site, new version adopts Python as back-end language and also as Photo Processing language under <http://www.rhophotos.com>.
<iframe width="800" height="450" src="http://www.rhophotos.com/" frameborder="0" allowfullscreen></iframe>
*This iframe is scrollable*

### Web Project Index
For time-saving sake, I made an index page for some of my sites under link <http://development.ijohann.com/>.  
<iframe width="800" height="600" src="http://development.ijohann.com/" frameborder="0" allowfullscreen></iframe>
*This iframe is scrollable*

### MarkDown Blog (2016.11-)
For the simplicity of writing and sharing, I made a MarkDown blog to replace this blog site under link <http://articles.ijohann.com> (currently shipping to Python back-end, php version under <http://php.articles.ijohann.com>).
<iframe width="800" height="450" src="http://articles.ijohann.com/" frameborder="1" allowfullscreen></iframe>
*This iframe is scrollable*

### Wechat Project (2015-2016)
Wechat Official Account Development (CLI Development) under link <http://weixin.ijohann.com>. One of my wechat official accounts can be accessed via following QR code.  
![Scan to Subscribe](/assets/images/wx-api/zwxx.jpg)
*Scan with Wechat to Subscribe*  

- Response to specific keywords
- Employed a robot api for user to chat with
- Get random jokes after each trigger
- Wechat customized H5 page with user authentication

### Web Project (2015)
- [Lyric Rolling Music Player](http://development.ijohann.com/lyricplayer/)
<iframe width="800" height="600" src="http://development.ijohann.com/lyricplayer/" frameborder="0" allowfullscreen></iframe>

### Android Project (2013)
- [Keep Fit (Android Phone App)](https://github.com/happyjohann/andriod-keepfit)

### iOS Projects (2012 - 2013)
- [iHome(iPhone App)](https://www.youtube.com/embed/LcO6RqZ2LnY)
<iframe width="800" height="600" src="https://www.youtube.com/embed/LcO6RqZ2LnY" frameborder="0" allowfullscreen></iframe>
- [Story of Effendi(iPad App)](https://youtu.be/gCIzlXu7pn4)
<iframe width="800" height="600" src="https://www.youtube.com/embed/gCIzlXu7pn4" frameborder="0" allowfullscreen></iframe>

### Undergraduate Courses
- C++ Language Programming (98/100)
- Python Programming (92/100)
- Visual Basic Language Programming(90/100)
- Web Development (91/100)
- Data Structure (100/100)
- Object Oriented Programming (92/100)
- Database Technology and Application (90/100)
- Database Management and Optimization (95/100)
- Computer Network Technology and Application (95/100)
- Computer Network Design (92/100)
- Information System Development (97/100)
- XML (93/100)

### High School Coding
- C Language Algorithm Programming, [National Olympiad in Informatics](http://www.noi.cn/about/summary) Advance Group 2rd Prize
- Major Study Field
	+ Greedy Algorithm
	+ Dynamic Programming
	+ Searching Programming
	+ Sorting Algorithm
	+ Data Structure

## More about Me as Programmer
[My Journey As A Programmer](./about-me/2015/10/27/My-Journey-As-A-Programmer.html)  

<hr>

## Recent Posts

**My recent posts are published on another site under <http://articles.ijohann.com>.**

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
