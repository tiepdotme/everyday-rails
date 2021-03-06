---
layout: post
title: "Mobile Rails applications, part 1: An overview"
excerpt: "Looking for the best way to make a mobile-optimized version of your website? Here are three resources to get you started, and a look at next steps."
---

I've spent some time over the last week learning more about creating mobile versions of Rails applications. In the past, I've created mobile applications that cater to the lowest common denominator&mdash;that is, non-Webkit-based mobile browsers with limited or nonexistent support for CSS or Javascript. This time around, I want to maintain support for these older devices, but also take advantage of the fact that a rapidly growing number of mobile devices _do_ support these common web standards. Here's a summary of what I've looked at so far, in the hopes that (a) it helps others in the same boat as me, and (b) it will solicit advice from people who have accomplished this themselves.

### Mobile Fu

The [Mobile Fu](http://github.com/brendanlim/mobile-fu) plugin for Rails is an easy way to quickly get a mobile version of your application up and running. Mobile Fu detects if a user is coming from a mobile device, and if so, returns a mobile-friendly view and CSS instead of standard ones. You just need to add a line to your `application_controller.rb` file, add the `:iphone` MIME type, and create mobile-specific views. Mobile Fu will also render any platform-specific CSS you create. This means you can use separate CSS depending on whether your app is being viewed on an iPhone, iPad, Blackberry, etc.&mdash;very useful, potentially. This is the method I used to create a basic mobile version of a data collection tool at my day job, allowing school administrators to observe classrooms and record what they see using their phones. How to get this started is all lined out well in the plugin's README.

Moving beyond this basic functionality, though, became frustrating for me. As I mentioned, I want the next iteration of my app in question to be savvy about the capabilities of different mobile devices, and render accordingly. Basic issues aside, the documentation for Mobile Fu is unfortunately sparse. I found a few useful methods within the code, but they didn't work quite the way I wanted to. I never could get the plugin to work with the jQTouch mobile Javascript framework (more on that in a minute), and as noted in [an issue](http://github.com/brendanlim/mobile-fu/issues#issue/6) in Mobile Fu's GitHub repository (and [another](http://github.com/brendanlim/mobile-fu/issues#issue/5)), the plugin doesn't play nice with other MIME types such as JSON. This isn't an issue for me right now, but will be in future iterations. So I began looking elsewhere.

### Railscasts episode

In lieu of any other drop-in solutions, the next best route may be to roll your own mobile-enabling code. As it turns out, it's not that hard&mdash;a [Railscasts episode on mobile websites](http://railscasts.com/episodes/199-mobile-devices) covers the requirements, using largely the same approach as Mobile Fu, but adds some additional functionality that Mobile Fu doesn't explicitly provide. In particular, Ryan Bates shows how to allow users to switch between the mobile version and the standard version. It also looks like it will be easier to augment with my own code, such as:

* **Mobile detection:** Ryan's regular expression might be worth revisiting. It may be possible to borrow from Mobile Fu's detection to make this more thorough, and hopefully return more specific device types.
* **Check for Javascript-ready browsers:** I'd like to know whether I can deliver a browser a fancy version or just return a no-frills mobile format. Maybe I should just stick to WebKit for the fancy version?
* **Check for iPads and tablets:** I see this becoming more important moving forward. Depending on the app, I may not even want to bother with a mobile version when rendering for a tablet's larger screen.

If these aren't issues for you, and you just want to create a mobile site that will work on Webkit-based devices, watch the Railscasts episode and you're well on your way.

### jQTouch

In the Railscasts episode above, [jQTouch](http://www.jqtouch.com/) is briefly mentioned and shown as a way to deliver native-like iPhone apps using a jQuery-based Javascript framework. Based on the recommendation, I spent some time hacking around with it, but it wasn't until I watched an hour-long [Peepcode episode dedicated to jQTouch](http://peepcode.com/products/jqtouch) that I began to figure things out. The episode is definitely worth the 9 dollars. As it turns out, jQTouch works best with AJAX, so I'll need to revisit my controllers and update as needed. Based on the examples and the tutorial, this extra work will be worth it.

There are other frameworks for mobile Javascript&mdash;jQTouch is just what I'm working with right now. Last week Mashable put together a list of [mobile Javascript frameworks](http://mashable.com/2010/08/18/mobile-web-app-frameworks/). This is just an overview, and it's not Rails-specific, but will give you an idea of the potential of each framework.

### What's next

My next steps, which I plan to document here in the coming days, are to refine some of my understanding of jQTouch with Rails (specifically form handling) and figure out what styles are missing from the default jQTouch templates (I'm using the Apple theme instead of the jQT theme; its brighter look will appeal more to my audience). From there I want to get into a more advanced mobile browser check to determine which version of my mobile site to send to a given device. To be continued, but if you have questions or suggestions in the meantime please share them.

<div class="alert alert-info" markdown="1">
**Update:** [part two of this series](/2010/08/29/mobile-rails-2.html) is now available.
</div>