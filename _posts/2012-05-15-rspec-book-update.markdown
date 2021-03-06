---
layout: post
title: "Update on the RSpec book"
excerpt: "I've been busy completing the book and incorporating your feedback. Here's where things stand at the moment, along with answers to some questions I've received."
tags: rspec rspec-book
---

It's been a little over a week since I made _Everyday Rails Testing with RSpec_ [available in beta book form](http://leanpub.com/everydayrailsrspec). Thank you to everyone who's purchased a copy. The response has been incredible, and I hope you all get something positive out of the book.

If you haven't downloaded the update I posted last Friday, I encourage you to sign into [Leanpub](http://leanpub.com/) and do so now. The biggest change is the availability of the sample Rails application I promised, but with a twist. Originally, I'd planned to provide one sample--a completed project with a full suite of RSpec examples. However, as I read through feedback and worked my way through the code, I realized a better approach to sharing the code would be to do so incrementally. The code samples follow this pattern: Each chapter gets its own version of the sample application, with relevant changes applied for you to study.

All of this has changed up my originally planned schedule quite a bit. Instead of hopping around as I write, as I was originally doing, following along with each stage of the code more or less requires me to write each chapter to coincide with that stage of code. In other words, as I write my controller specs, I'm writing the chapter on controller specs--and, as it turns out, this is necessitating me to merge authentication testing, which I'd intended to be a separate chapter, into the controller specs chapter. So instead of said chapter being a mostly direct copy of the original blog post, it's largely new material. So that's taking a little longer to write, but I think it's worth it. The book will wind up flowing better from chapter to chapter, and will better represent the techniques I use for much of my testing.

In addition to the expanded code samples, I'm planning on at least one additional chapter for the book. It's content I'd planned to include but have decided to expand--so _Testing the Rest_ will be its own chapter and include other components of your application you may wish to test with RSpec.

My rough schedule for the next few weeks:

* May 18: Finish rewrite of controllers chapter; possibly expand into a second chapter on managing controller specs.
* May 25: Rewrite of request specs chapter; write _Testing the Rest_ chapter; write _Improving Specs chapter_.
* June 1: Write _Toward Test-driven Development_ chapter; revise _Parting Advice_.

Finally, I'd like to quickly respond to a few general questions I've received so far:

### What is the intended level of experience for the book?

Definitely not for beginners. Most likely not for advanced testers (though if you've purchased a copy, you may pick up on some new ideas). If you're a beginner, or have minimal exposure to testing (say, you skipped that chapter in the popular Rails tutorials) you may be better served by Michael Hartl's _Rails 3 Tutorial_. If you're more interested in knowing every detail of RSpec, refer to _The RSpec Book_ by Chelimsky et al.

I've done a lot of refactoring in the book, as I mentioned, and have put a lot of work this past week in explaining examples and why I make certain decisions. But the book still moves pretty quickly. However, if you feel like I'm glossing over something too much (or, conversely, am spending way too much time on a concept) please let me know. This is my first book; I value your feedback.

### When will the book be done?

Two part answer: I intend to have all the content for testing a Rails 3.2 application done and (mostly) edited by the end of May. As typos, bugs, or changes to the Rails and RSpec landscape (as they apply to Rails 3.2) emerge.

When Rails 4.0 is released, I am going to update the sample code and book as needed, and release that as the final version. The reasoning behind this is I want you to have something you can use now with Rails 3.2, but won't be obsolete when Rails 4.0 comes out.

### What if I find an error or have a suggestion?

The best way to let me know is to send me an email at aaron _at_ everydayrails _dot_ com. If you'd prefer you can let me know by leaving a comment here, or on the book's product page on Leanpub, or on Twitter. I know email isn't the most transparent tool, but right now I'm focused on getting content done and as correct as possible.

### When will the book be available in my country?

Or, _why do I have to use Paypal to purchase the book?_ Leanpub is a Canadian country, so some payment gateways (read: Stripe) aren't available to them at this time. Once the 4.0 version of the book is finished I may pursue other channels, but while it's still in heavy development I'm afraid we're stuck with Paypal--or at least until Leanpub is able to use another gateway. I do know this is high on their wish list.

### How's your experience with Leanpub been?

I've been really happy with Leanpub as a publishing tool. It's very programmer-friendly--I'm using TextMate and Markdown for everything, and Leanpub's Dropbox integration is slick. I definitely recommend checking it out if you've got a book you've been dying to write, but haven't known where to start.

### Are you for hire?

Maybe? Let's talk. Shoot me an email.