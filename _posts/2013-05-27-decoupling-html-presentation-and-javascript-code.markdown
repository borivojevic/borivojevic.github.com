---
layout: post
subtitle: Decoupling HTML presentation and JavaScript code
tagline: Decoupling HTML presentation and JavaScript code
category: posts
draft: true
---

## The Problem

We have all been there sometime. You're doing light front-end programming and you have to generate and inject HTML code dinamically to the page through javascript. You don't want to use robust framework like Backbone or Ember as your requrements are light - e.g. dinamically adding form fields or updating chat history in comments application.

The easy way to append HTML in JavaScript is to use jQuery and append raw blob of HTML like:

{% highlight javascript %}
$(comments).each(function(index, comment) {
    $("#comments").append("<div class='comment'><div class='author'><img src='/user/"+comment.author.id+"/avatar' /><p class='author-name'>"+comment.author.name+"</p></div><p>"+comment.text+"</p></div>");
});
{% endhighlight %}

Although it's a straight-forward solution it's kind of dirty for couple of reasons:
- It's not very readable, right? You'll have hard time maintaining such mess.
- We'll can end up with same HTML blocks on several places so when we change it on one place we need to pay atention to change it on others as well
- We lost track of where we keep presentation logics as it got scatered accross views and javascript files.

## Solution

My rule of thumb here is to keep HTML presentation in view files and don't mix it up with JavaScript. There are couple of popular javascript templating engines out there though I have my favorites.

If I have to solve really simple thing I'd reach for [JavaScript Micro-Templating][] function from John Ressig (also known for author of jQuery javascript library). Though if HTML goes commplicated I'd reach form more powerfull JavaScript templating engine [Handlebars.js][]

I'll skip explaining them as there are tons of good contents and tutorials online and jump right into the demo.

## Demo

Let's take YouTube video player clone as a an example. User can write and post comments using textarea. On submit click we want to post the comment to server and then generate and add comment HTML to the list through JavaScript.

<center><img src="/images/blog/2013-06-20_0737-player.png" alt="Video Player Demo" /></center>

### Step 1. Load HandlebarJS library

Download and unpack handlebar.js lib in your application's javascript path and include it to page head section via script element. For this example we'll simply load handlebars via Cloudflare content delivery network.

{% highlight javascript %}
<script type="text/javascript" src="http://cdnjs.cloudflare.com/ajax/libs/handlebars.js/1.0.0-rc.4/handlebars.min.js"></script>
{% endhighlight %}

### Step 2. Create Handlebars template

We can now take HTML structure for comment and encapsulate it in Handlebars template. To organize script elements you can create separate view file to keep all of your templates or you can simply add it at the bottom of view file.

{% highlight javascript %}
<script id="entry-template" type="text/x-handlebars-template">
    <div class="entry {{position}}">
        <img class="comment-avatar" src="{{avatar}}"></img>
        <h2>By {{author}}</h2>
        <div class="body">
            {{body}}
        </div>
    </div>
</script>
{% endhighlight %}

### Step 3. Write JavaScript to dinamically add comments

To generate HTML code from Handlebars template we'll have to compile it and pass runtime variables.

{% highlight javascript %}
var addCommentLambda = function() {
    var comment = $("textarea#comment-text").val();
    if (comment === "") {
        return;
    }
    var commentsCount = $("div#comments .entry").length;
    var position = (commentsCount % 2 === 0) ? "left" : "right";
    var source = $("#entry-template").html();
    var template = Handlebars.compile(source);
    var result = template({
        position: position,
        avatar: "./img/avatar.png",
        author: ["John", "Bob", "Jack", "Alise"][commentsCount % 4],
        body: comment
    });
    $(result).hide().prependTo("div#previous-comments").slideDown("fast");
    $("textarea#comment-text").val("");
};
{% endhighlight %}

Finally we'll bind the function on submit button click. Aditionally we can post the comment to the server and then append it to the list in callback but I'll leave this part out as it's not important for this demo.

{% highlight javascript %}
$(document).ready(function() {
    $("button#add-comment").bind("click", addCommentLambda);
});
{% endhighlight %}

## Source

You can view working demo at http://mirkoborivojevic.com/handlebars-demo/ or browse full source code at https://github.com/borivojevic/handlebars-demo

[JavaScript Micro-Templating]: ejohn.org/blog/javascript-micro-templating/
[Handlebars.js]: http://handlebarsjs.com/