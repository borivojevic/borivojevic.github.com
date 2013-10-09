---
layout: post
subtitle: PHP isset() vs array_key_exists()
tagline: PHP isset() vs array_key_exists()
category: posts
---

Take a look at following piece of code:

{% highlight php %}
<?php
if (isset($apiResponseJsonArray['error'])) {
    throw new \Exception("API returned error: " . $apiResponseJsonArray['error']);
}
{% endhighlight %}

What do you expect it to do? Can you smell any problems?

This functioning piece of code has an edge case for <code>$apiResponseJsonArray['error'] = NULL</code> in which exception is not being thrown.

This was an unexpected twist for me and I was quite puzzeled where was the problem. Digging into PHP documentation gave me the answer:

[Example #2][]
> isset() does not return TRUE for array keys that correspond to a NULL value, while array_key_exists() does.

Given this unpredictable behavior I was asking WHY would I ever use isset() except it's shorter to type. Does it have any performance benefit?

Well, apparently isset() is a bit faster. According to [benchmark][] from top of Google search results array_key_exists() is 2.5 times slower than isset().

Even with the slower performance array_key_exists() totaly wins the day for me and I'm committed never to use isset() again.

It's supprising and it's humbling when you discover something new in language you're using for so many years. That's why I decided to celebrate it and share this a-ha moment in the form of a blog post. Also, [enjoy my surprised face][] when I come to realization.

[Example #2]: http://php.net/manual/en/function.array-key-exists.php
[benchmark]: http://ilia.ws/archives/247-Performance-Analysis-of-isset-vs-array_key_exists.html
[enjoy my surprised face]: http://global3.memecdn.com/aha_o_1471219.webp