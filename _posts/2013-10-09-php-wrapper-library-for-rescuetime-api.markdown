---
layout: post
subtitle: PHP wrapper library for RescueTime API
tagline: PHP wrapper library for RescueTime API
category: posts
---

[RescueTime][] is one of my favorite productivity tools that helps me track how I spend my time as a freelancer and and optimize it to be more productive. I've been using it for couple of months and I fell in love with it.

It's a free product so I wanted to show some love back by open-sourcing PHP wrapper library for their API.

At this point RescueTime API provides single endpoint to fetch detailed and complicated data. As a developer you'll have to understand basic RescueTime concepts like productivity , categories or activities. The data is read-only through the API.

The source code is available on Github at [borivojevic/rescuetime-api-php][]

## Installation

Recommend way to install this package is with [Composer][]. Add `borivojevic/rescuetime-api-php` to your composer.json file.

{% highlight json %}
{
    "require": {
        "borivojevic/rescuetime": "1.*"
    }
}
{% endhighlight %}

To install composer run:

{% highlight bash %}
curl -s http://getcomposer.org/installer | php
{% endhighlight %}

To install composer dependences run:

{% highlight bash %}
php composer.phar install
{% endhighlight %}

You can autoload all dependencies by adding this to your code:

{% highlight php %}
<?php
require 'vendor/autoload.php';
{% endhighlight %}

## Usage

The main entry point of the library is the `RescueTime\Client` class. API methods require to be signed with valid `api_key` parameter which you have to provide as a first argument of the constructor. You can obtain RescueTime API key on [API Key Management][] console page.

{% highlight php %}
<?php
$Client = new \RescueTime\Client($apiKey);

// Basic example
$activities = $Client->getActivities("rank");

foreach ($activities as $activity) {
    echo $activity->getActivityName();
    echo $activity->getProductivity();
}

// Fetch activities for past week
$activities = $this->Client->getActivities(
    "interval",
    "day",
    null,
    null,
    new \DateTime("-6 day"),
    new \DateTime("today")
);

// Fetch productivity data grouped by activity
$activities = $this->Client->getActivities(
    "interval",
    "day",
    null,
    null,
    new \DateTime("-6 day"),
    new \DateTime("today"),
    "activity"
);

// Fetch productivity data grouped by category
$activities = $this->Client->getActivities(
    "interval",
    "day",
    null,
    null,
    new \DateTime("-6 day"),
    new \DateTime("today"),
    "category"
);
{% endhighlight %}

You can build more complex queries and filter down the data by providing other query parameters:

{% highlight php %}
<?php
$this->Client->getActivities(
    <perspective>,
    <resolution_time>,
    <restrict_group>,
    <restrict_user>,
    <restrict_begin>,
    <restrict_end>,
    <restrict_kind>,
    <restrict_project>,
    <restrict_thing>,
    <restrict_thingy>
);
{% endhighlight %}

Each query parameter is explained in more details in official [HTTP Query Interface documentation][].

## Demo

For a working exaple of application that uses rescuetime-api-php library take a look at [borivojevic/rescuetime-statusboard][]. It is a simple web application build with [Silex][] that converts the data returned from API into the format that can be read by [Status Board][] iPad app.

Application provides a dashboard interface with couple key metrics:

- Summary panel
- Productivity By Day Panel
- Top Categories Panel
- Top Activities Panel

<center><img src="http://f.cl.ly/items/1p0n00010s052H3T0B1e/statusboard-productivity-by-day.jpg" alt="Productivity By Day Panel" /></center>

It's easy to install it for your own purposes but if you relly want to integrate your RescueTime account with Status Board it's even more convenient to enable it in [RescueTime directly](https://www.rescuetime.com/setuppanicstatusboard).

## I'd like to hear from you

Thank you for reading all the way down here and I really hope you got some value from it. If you have any questions or suggestions feel free to write me at <a href="mailto:mirko@mirkoborivojevic.com">mirko@mirkoborivojevic.com</a> or to leave a comment below. Also, patches and pull requests are always welcome.

[RescueTime]: https://www.rescuetime.com
[borivojevic/rescuetime-api-php]: https://github.com/borivojevic/rescuetime-api-php
[Composer]: http://getcomposer.org/
[API Key Management]: https://www.rescuetime.com/anapi/manage
[HTTP Query Interface documentation]: https://www.rescuetime.com/anapi/setup/documentation#http
[borivojevic/rescuetime-statusboard]: https://github.com/borivojevic/rescuetime-statusboard
[Silex]: http://silex.sensiolabs.org/
[Status Board]: http://panic.com/statusboard/