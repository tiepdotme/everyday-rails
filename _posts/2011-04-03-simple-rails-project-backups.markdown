---
layout: post
title: "Simple Rails site backups"
excerpt: "How do you back up production data for your Rails projects? A new gem called Backup may be all you need."
---

Today's post won't be specifically about Rails, but it will be about an important component of deploying productional Rails applications: Backing up your application's data. Regardless of whether you've already got a system in place or if you're hoping your hosting provider has your back, here's an elegant way to gain some peace of mind and have data that are easy to recover should the unthinkable happen.

A little about my server setup: I've got several Linux-based virtual private servers running at work; each one with one to five Rails applications being served from it. I like to have data backups of each application (both database data and any file uploads an app may have) in addition to traditional server backups, just in case. In my situation, it would be faster to redeploy an application on a new virtual server than it would be to recover an existing virtual server, especially if I only need to recover data from one of the apps.

Traditionally, I've used a hodgepodge set of bash scripts to dump MySQL databases and copy data directories over to another server, but with the [Backup](https://github.com/meskyanichi/backup) gem by Michael van Rooijen my setup is a little less hodgepodge and a lot more elegant. Here's a quick look at how to get things set up.

<div class="alert alert-info" markdown="1">
As mentioned in the README, Backup won't work on Windows computers.
</div>

The Backup gem is incredibly easy to install, assuming you've got shell access to your server (I haven't tried this on the likes of Heroku or Engine Yard). Follow the instructions in the [README](https://github.com/meskyanichi/backup/blob/develop/README.md) and the [project wiki](https://github.com/meskyanichi/backup/wiki) to get going; assuming you've got data to back up and somewhere to back it up to you'll be up and running in no more than 15 minutes. My only problem: I selected scp as a file transfer option, since I have an existing backup server, and had to install a couple of extra dependencies to get things to work. The good news is the Backup gem does a nice job reporting missing dependencies and setting you on the right path (and I like that it doesn't install a bunch of gems I wouldn't have otherwise used). Afterward I noticed there's a convenient way to see what your configuration's dependencies will be:

{% highlight bash %}
  $ backup dependencies --list
{% endhighlight %}

### Next steps

* You'll probably want to set up automated execution of your new backup system. Van Rooijen and I both recommend using the handy [Whenever](https://github.com/javan/whenever) gem to create cron jobs for this purpose.
* You can set up notification via e-mail, Twitter, or Campfire. See [the project wiki's page on notifications](https://github.com/meskyanichi/backup/wiki/Notifiers) for details.
* Replace your current Rails backup solution (the one that possibly involves duct tape and baling wire) with Backup. OK, maybe that's just a next step for _me_.