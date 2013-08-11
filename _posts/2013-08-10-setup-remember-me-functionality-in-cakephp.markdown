---
layout: post
subtitle: Setup remember me functionality in CakePHP 2.0
tagline: Setup remember me functionality in CakePHP 2.0
category: posts
---

Internet is full of opinions and how-tos for implementing "remember me" login functionality in CakePHP. While most of the them suggest adding custom implementations I want to show you how to implement "remember me" functionality using only standard CakePHP components.

Let's start with [Blog Tutorial][] from CakePHP manual and build remember me functionality on top of it.

## Setup Cookie Authentication Adapter

Install [Authenticate plugin][] for CakePHP by Ceeram. Plugin implements additional adapters for AuthComponent like CookieAuthenticate to login with a cookie.

You can follow installation manual or you can simply download [CookieAuthenticate.php][] and place it in `app/Controller/Component/Auth/CookieAuthenticate.php` if you think you don't need other authentication adapters.

## Setup Authentication Component

Add following code in your AppController::beforeFilter:

{% highlight php %}
<?php
$this->Auth->authenticate = array(
	'Cookie' => array(
		'fields' => array(
			'username' => 'username',
			'password' => 'password'
		),
		'userModel' => 'User',
	),
	'Form'
);
{% endhighlight %}

## Setup Cookie component

For enhanced security for Cookie authentication enable AES symmetric encryption of cookie. Add following line to AppController::beforeFilter:

{% highlight php %}
<?php
public function beforeFilter() {
	parent::beforeFilter();
	$this->Cookie->type('rijndael'); //Enable AES symetric encryption of cookie
}
{% endhighlight %}

Open `app/Config/core.php` and update session configuration. This is particularly important if you want login session to expire after web browser closes. That said user will be forced to login next time he comes to website unless he checked 'remember me' when logging in.

{% highlight php %}
<?php
Configure::write('Session', array(
	'defaults' => 'php',
	'cookieTimeout' => 0 // expires when browser closes
));
{% endhighlight %}

## Update login view

Open app/View/Users/login.php and add 'Remember me' checkbox

{% highlight php %}
<div class="users form">
<?php echo $this->Session->flash('auth'); ?>
<?php echo $this->Form->create('User'); ?>
	<fieldset>
		<legend><?php echo __('Please enter your username and password'); ?></legend>
		<?php
			echo $this->Form->input('username');
			echo $this->Form->input('password');
			echo $this->Form->checkbox('remember_me'); // added this line
		?>
	</fieldset>
<?php echo $this->Form->end(__('Login')); ?>
</div>
{% endhighlight %}

## Update login action

Next, update login action to store user's session in cookie if user selected remember me box.
Add following code to UsersController::login action:

{% highlight php %}
<?php
public function login() {
	if ($this->request->is('post')) {
		if ($this->Auth->login()) {
			$this->_setCookie($this->Auth->user('id'));
			$this->redirect($this->Auth->redirect());
		} else {
			$this->Session->setFlash(__('Invalid username or password, try again'));
		}
	}
	if ($this->Auth->loggedIn() || $this->Auth->login()) {
		return $this->redirect($this->Auth->redirectUrl());
	}
}
{% endhighlight %}

Also add UsersController::_setCookie method:

{% highlight php %}
<?php
protected function _setCookie($id) {
	if (!$this->request->data('User.remember_me')) {
		return false;
	}
	$data = array(
		'username' => $this->request->data('User.username'),
		'password' => $this->request->data('User.password')
	);
	$this->Cookie->write('User', $data, true, '+2 week');
	return true;
}
{% endhighlight %}

That's all. We setup 'remember me' functionality without a line of custom code.

To see all changes we made at one place take a look at [following changeset][] on Github. You can also clone entire [demo application][] if you need it.

[Blog Tutorial]: http://book.cakephp.org/2.0/en/tutorials-and-examples/blog/blog.html
[Authenticate plugin]: https://github.com/ceeram/Authenticate
[CookieAuthenticate.php]: https://github.com/ceeram/Authenticate/blame/master/Controller/Component/Auth/CookieAuthenticate.php
[following changeset]: https://github.com/borivojevic/cakephp-remember-me-demo/commit/e49de595d019530dd92d0e38ffd908b4d4de3815
[demo application]: https://github.com/borivojevic/cakephp-remember-me-demo