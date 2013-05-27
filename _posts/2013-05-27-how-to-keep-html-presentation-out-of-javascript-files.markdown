---
layout: post
title: Blog
subtitle: How to keep HTML presentation out of JavaScript files
category: posts
---

## The Problem

Imagine you're writting web application and you have to add more HTML content on page through JavaScript. E.G. you are fetching a bunch of comments from server through ajax call and you have to append them to comments section.

The easy way to do it would be to travese through comments in javascript and just append raw htlm trhough javascript like:

{% highlight javascript %}
$(comments).each(function(index, comment) {
    $("#comments").append("<div class='comment'><div class='author'><img src='/user/"+comment.author.id+"/avatar' /><p class='author-name'>"+comment.author.name+"</p></div><p>"+comment.text+"</p></div>");
});
{% endhighlight %}

Although it's a straight-forward solution it's kind of dirty for couple of reasons:
- It gets very hard to maintain as HTML block is hardly readable
- We'll can end up with same HTML blocks on several places so when we change it on one place we need to pay atention to change it on others as well
- We lost track of where we keep presentation logics as it got scatered accross views and javascript files.

## Solution

Rule of thumb here is to keep HTML presentation in view files and don't mix it up with javascript. There are couple of popular javascript templating engines out there though I have my favorites.

If I had to solve like really simple thing I'd go reach for [JavaScript Micro-Templating][] function from John Ressig (also known for author of jQuery javascript library). Though if things go commplicated I'd reach form more powerfull javascript templating engine [Handlebars.js][]

I'll skip the intro as there's ton of good content about it online and jump right into the demo.

## Demo


[JavaScript Micro-Templating]: ejohn.org/blog/javascript-micro-templating/
[Handlebars.js]: http://handlebarsjs.com/