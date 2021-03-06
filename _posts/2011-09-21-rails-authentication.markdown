---
layout: post
title: "Rails authentication today: Options for 3.0 and 3.1"
excerpt: "There's no shortage of approaches to adding password-based security to your Rails applications. Here's a look at the current lineup."
tags: security
---

Early on in _Everyday Rails_, I outlined [three options for adding authentication to your Rails applications](http://everydayrails.com/2010/06/06/rails-authentication-options.html). It's still a popular subject&mdash;and with two major releases of Rails and a number of new authentication options hitting the streets since I wrote my original list, it's due for a refresh. Here's a look at the current Rails authentication landscape.

### Outsourced authentication: OmniAuth

If you don't want to mess with usernames and passwords&mdash;or perhaps as importantly don't want your users to have to mess with so many of them&mdash;[OmniAuth](https://github.com/intridea/omniauth) is your best bet. OmniAuth lets you pick from any number of login providers, from Facebook to Twitter to GitHub to Google to LDAP to OpenID to any combination of the above. See the README for [a full list of 60-plus providers](https://github.com/intridea/omniauth/blob/0-3-stable/README.md). With some additional work, you can also incorporate OmniAuth into an existing login mechanism.

Note that as of this writing there are two branches&mdash;a [stable 0.3 branch](https://github.com/intridea/omniauth/tree/0-3-stable) and a [work-in-progress 1.0 version](https://github.com/intridea/omniauth). The change is a major one, but [well-justified](https://github.com/intridea/omniauth/issues/451) for the long-term health of OmniAuth.

### Turnkey authentication: Devise

Last year I recommended [Devise](https://github.com/plataformatec/devise) for Rails authentication needs. Its biggest selling point then continues to be its biggest selling point now: Full-featured authentication, including signing up, signing in, password resets, account management, and testing helpers. With Devise, you can have all of that in under ten minutes. [Devise's documentation](https://github.com/plataformatec/devise/wiki) continues to be incredible, as does [the list of third party extensions](https://github.com/plataformatec/devise/wiki/Extensions) to provide alternative ORMs, encryption, external authentication, and other general functionality.

That said, I've moved away from Devise for most of my projects, because inevitably something I wanted to do didn't jive with how Devise wanted to do things. However, if you need full-featured authentication, won't be straying too far out of the box, and time is an issue, Devise is still tough to beat.

### Barebones authentication: Sorcery

[Sorcery](https://github.com/NoamB/sorcery) is this year's new kid on the block for authentication, and to be honest my exposure to it doesn't go much further than the recent [Railscasts episode](http://railscasts.com/episodes/283-authentication-with-sorcery) covering how to set it up. Its developer, Noam Ben Ari, stresses a "less is more" approach to authentication&mdash;giving you the tools you need to set up authentication relatively quickly, as well as room to adjust in the future as your application's authentication requirements change. In other words, Sorcery provides the authentication library, but you'll have to write the models, views, and controllers to make it work the way you want it to in your app.

In terms of complexity, Sorcery resides somewhere in the middle of Devise's kitchen sink approach and the do-it-all-yourself approach I'll talk about momentarily. As such, it should be a good option if you want to be able to get some quick authentication in place and still be able to customize down the road with minimal headaches.

### Authentication from scratch

If you want total independence from other developers' interpretations of how authentication systems work, it's really not that hard to write your own. Two excellent Railscasts episodes cover how to do this in [Rails 3.1](http://railscasts.com/episodes/270-authentication-in-rails-3-1) (using the new `has_secure_password` option) or [Rails 3](http://railscasts.com/episodes/250-authentication-from-scratch). Bonus points for [adding password reset and remember me functionality](http://railscasts.com/episodes/274-remember-me-reset-password); even more bonus points for [writing specs to make sure authentication works as planned](http://railscasts.com/episodes/275-how-i-test).

For the record, this is the approach we took on our current big project at my day job. It does take a little more time to implement&mdash;though not much. In the long term we've got a simple-but-solid solution that fits our exact authentication needs perfectly.

### Other players

* [Clearance](https://github.com/thoughtbot/clearance) is a solid authentication option that takes a simple approach, provides room for customization, and has the backing of the folks at thoughtbot. I've just never used it myself in a Rails project.
* [Authlogic](https://github.com/binarylogic/authlogic) should work in Rails 3 apps; not sure about Rails 3.1. Frankly, it's never been my favorite approach to Rails authentication, but I know it's had its fans over the years.
* [Restful Authentication](https://github.com/technoweenie/restful-authentication) shouldn't be used in new Rails apps. It served me well for a number of years, but time marches on.
* **Update September 22:** [letmein](https://github.com/GBH/letmein) is another barebones approach to authentication. Thanks to Ash McKenzie for sharing.
* There are a couple of other options in the [Ruby Toolbox authentication category](http://ruby-toolbox.com/categories/rails_authentication.html). I haven't tried them.