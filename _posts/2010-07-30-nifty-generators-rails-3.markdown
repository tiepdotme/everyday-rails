---
layout: post
title: "Use Nifty Generators in Rails 3"
excerpt: "The very handy Nifty Generators gem works in Rails 3 with a few minor usage changes."
---

I've talked a lot about the Nifty Generators gem, created by Ryan Bates of Railscasts (see my introductions to [nifty_layout](http://everydayrails.com/2010/05/25/nifty-generators.html), [nifty_config](http://everydayrails.com/2010/05/27/nifty-config.html), and [nifty_scaffold](http://everydayrails.com/2010/06/01/nifty-scaffold.html)). When I began experimenting with Rails 3 I noticed that the gem was [not yet Rails 3 compatible](http://www.railsplugins.org/plugins/154-nifty-generators) according to RailsPlugins.org, but I'm happy to report (along with RailsPlugins.org users eggie5 and dgerton) that it's working fine&mdash;you just need to change a few things about how you use it. Here's what you need to do to get rolling:

### 1. Include Nifty Generators in your Gemfile

Unlike apps written in previous versions of Rails, your Rails 3 app needs to know you're using Nifty Generators. Easy enough: Add the following line to your Gemfile:

{% highlight ruby %}
  gem 'nifty-generators', '>= 0.4.0'
{% endhighlight %}

then install it if necessary, using Bundler:

{% highlight bash %}
  $ bundle install
{% endhighlight %}

### 2. Know the command line changes

This is the biggest difference, and it's actually pretty minor. Replace the underscore in the old way of using the generator with a colon. So the old way of generating a scaffold, like

{% highlight bash %}
  $ script/generate nifty_scaffold book title:string author_id:integer description:text
{% endhighlight %}

becomes

{% highlight bash %}
  $ rails generate nifty:scaffold book title:string author_id:integer description:text
{% endhighlight %}

For a list of all four Nifty Generators' new syntax, just type

{% highlight bash %}
  $ rails generate -h
{% endhighlight %}
