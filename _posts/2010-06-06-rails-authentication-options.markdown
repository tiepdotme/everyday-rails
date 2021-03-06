---
layout: post
title: "3 Rails authentication options"
excerpt: "It's almost inevitable that your Rails application will ultimately need some sort of login mechanism to protect access to certain parts. This post begins a series of looks at various options for authorization in Rails."
---

Today's post serves mostly to set up what I'm planning to write about next in _Everyday Rails_. Upon request, I'll be covering how I handle authentication and authorization in my Rails applications. These are topics that have evolved a great deal from the early days of Rails, and even if you've got a go-to solution for either it's worth spending a little time to check on the latest and greatest.

<div class="alert alert-info" markdown="1">
**What's the difference between authentication and authorization?** _Authentication_ tells your app who you are. _Authorization_ tells your app what you can do based on who you are.
</div>

My plan is to spend some time this week covering authentication, then dovetail from there into authorization. To set the tone, here are three authentication tools I've used or am getting ready to use:

## Restful Authentication

As a Rails developer, I've had the most experience with the venerable [Restful Authentication](http://github.com/technoweenie/restful-authentication) plugin/gem. It's been around since the early days of Rails, and I'll admit that's why I'm guilty of continuing to use it for as long as I have. However, I'll also admit that it's getting a little long in the tooth (I'm not sure if there are plans to continue supporting it for Rails 3 apps), and may not do things the way you'd expect a modern Rails app to. It's also not a feature-complete authentication system--you'll need to do some work to add some features you'll definitely want in your app (more on that in a second).

Restful Authentication still has some things going for it. It's very easy to get started--`script/generate authenticated user sessions`, then juggle an `include AuthenticatedSystem` line from the generated controllers into `app/controllers/application_controller.rb`, and your app technically has authentication. Don't forget to include the following line in any controller you want to require a user to log in to use:

{% highlight ruby %}
  before_filter :login_required
{% endhighlight %}

The [Railscast on Restful Authentication](http://railscasts.com/episodes/67-restful-authentication) is almost three years old, but the instructions should still work. That's how stable this option has been over the years.

Restful Authentication has three big holes you'll need to plug:

1. Lack of password reset functionality: What do you do if you've forgotten your password? There's nothing out of the box to help in this situation, but luckily the [forgot_password](http://github.com/greenisus/forgot_password) generator creates the necessary code (a controller, model, and mailers) to take care of the dirty work.
2. Lack of account editing: The Restful Authentication generator gives you `new` and `create` methods in `users_controller.rb`--what happens if a user wants to change his password after he's logged in (or other information, if you've extended the default fields in the users database table)? This takes a little extra work. Later this week I'll share how I've addressed this in the past, with the help of authorization.
3. Anyone can create an account: This is the default setup for Restful Authentication, but what if you're building a closed system (say, a company intranet) and don't want just anyone signing in? There are workarounds for that as well, which I'll touch upon this week as well.

Ultimately, though, I don't want to spend a lot of my time (or yours) on Restful Authentication, mainly because I think Devise is currently the best way to go for adding authentication to your Rails apps. More on Devise in a minute.

## Authlogic

[Authlogic](http://github.com/binarylogic/authlogic) was the cool new kid on the Rails authentication scene about a year ago. I'm pretty sure that it was written in response to Restful Authentication's tendency to sprinkle code throughout your app. It's arguably more straightforward, but you have to do _much_ more of the work yourself. For the _Everyday Rails_ type (i.e., we've got a job to do) this may not be an attractive method. However, Authlogic is designed to be extensible--changes don't come off as hacks because it's designed to be customized (you can also add other authentication methods such as OpenID fairly readily). I've used it in a couple of projects and it does work fine as an alternative to Restful Authentication.

This is as far as I'm going to go with Authlogic. There's a good [Railscast on Authlogic](http://railscasts.com/episodes/160-authlogic) if you want a tutorial, and the [Authlogic example application](http://github.com/binarylogic/authlogic_example) is useful as well.

## Devise

The latest and possibly greatest Rails authentication system yet is [Devise](http://github.com/plataformatec/devise). Devise looks to be modern (it's all set for Rails 3) and ready to go for a quick authentication solution (Rails magic done right). This week I'll be working with it for the first time at my day job, so expect a post with what I've learned soon. In the meantime [check out the Railscast on Devise](http://railscasts.com/episodes/209-introducing-devise) (as well as the [episode on customizing it for your app](http://railscasts.com/episodes/210-customizing-devise)). to hopefully get a feel for why I'm excited to give it a try. 
