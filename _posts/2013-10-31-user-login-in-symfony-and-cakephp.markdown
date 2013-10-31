---
layout: post
subtitle: How to programmatically login user in Symfony2 and CakePHP
tagline: How to programmatically login user in Symfony2 and CakePHP
category: posts
---

## Or how I learned to stop worrying and love convention over configuration.

Today I stumbled on tutorial describing how to programatically login user in Symfony2. I was surprised how many preparation steps does it take to do simple thing that I'd otherwise accomplish in two lines of CakePHP.

## Symfony2 example

{% highlight php %}
<?php
use Symfony\Component\EventDispatcher\EventDispatcher,
    Symfony\Component\Security\Core\Authentication\Token\UsernamePasswordToken,
    Symfony\Component\Security\Http\Event\InteractiveLoginEvent;

$em = $this->getDoctrine();
$repo = $em->getRepository("UserBundle:User");
$user = $repo->loadUserByUsername($username);

// Here, "public" is the name of the firewall in your security.yml
$token = new UsernamePasswordToken($user, $user->getPassword(), "public", $user->getRoles());
$this->get("security.context")->setToken($token);

// Fire the login event
// Logging the user in above the way we do it doesn't do this automatically
$event = new InteractiveLoginEvent($request, $token);
$this->get("event_dispatcher")->dispatch("security.interactive_login", $event);
{% endhighlight %}

## CakePHP example

{% highlight php %}
<?php
$user = $this->User->findByUsername($username);
$this->Auth->login($user);
{% endhighlight %}

My intention here was not to offend anyone or to start another PHP framework flame war but to share the power of 'convention over configuration' design approach that powers CakePHP framework.

I'm certainly not a Symfony2 expert so if I made a mistake please share it in comments and let us all learn from it.

Peace ☮

## References
* [How to login a user programatically in Symfony2](http://hasin.me/2013/10/27/how-to-login-a-user-programatically-in-symfony2/)
* [How to programmatically login/authenticate a user?](http://stackoverflow.com/a/9550356)
* [CakePHP Cookbook](http://book.cakephp.org/2.0/en/core-libraries/components/authentication.html#manually-logging-users-in)
