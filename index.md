---
layout: page
name: Johann Huang
---

## Education
In August 2016,  
will be a *Graduate Student*,  
in *[School of Business, Economics and Law](http://handels.gu.se/english)*,  
**[University of Gothenburg, Sweden](http://www.gu.se/)**.  

## Experiences

### Web Projects (2015-2016)
<iframe width="800" height="600" src="http://ijohann.com/exhibition/" frameborder="0" allowfullscreen></iframe>
*The above iframe is scrollable*

### Wechat Projects (2015-2016)
[Wechat](http://www.wechat.com/en/) Official Development (CLI Development)
![Scan to Subscribe](/assets/images/wx-api/zwxx.jpg) 
*Scan with Wechat to Subscribe*  

- Response to specific keywords
- Employed a robot api for user to chat with
- Get random jokes after each trigger 
- Wechat custimized H5 page with user authentication


### Web Projects (2015)
- [Lyric Rolling Music Player](http://mylyric.ml/)
<iframe width="800" height="600" src="http://mylyric.ml/" frameborder="0" allowfullscreen></iframe>

### Android Project (2013)
- Keep Fit (Android Phone App)

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