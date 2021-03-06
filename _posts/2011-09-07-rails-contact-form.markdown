---
layout: post
title: "MailForm: Easy e-mail contact forms for your Rails apps"
excerpt: "Get e-mail feedback from your site's visitors though this simple-to-implement web form."
---

Everyday Rails is back after an unexpected summer hiatus that included an overloaded work schedule and an unexpected move in residence. To get back in the swing of things, I'd like to share a handy gem I discovered and implemented a few times over the summer to accomplish the common task of sending comments from a form. While there are a few ways you could write your own, I recommend taking a look at MailForm for both getting the task done and learning more about working with Ruby and Rails constructs that fall outside of the typical scaffold approach.

### What is it?

[MailForm](https://github.com/plataformatec/mail_form) is a dead-simple, customizable gem to deliver mail to designated recipients from a Rails app via a web form. It was developed by Jos&#233; Valim and Carlos Antonio da Silva of Platforma Tec, makers of the popular Devise authentication gem. (Jos&#233; is also a core contributor to Rails.)

By the way, if you're interested in learning more about the internals of this gem, check out Jos&#233;'s [Crafting Rails Applications&#058; Expert Practices for Everyday Rails Development](http://amzn.to/nS90ZU) (no relation to this blog). It's a great way to get a better understanding of Ruby and Rails internals.

### Who's it for?

MailForm is for Rails developers who would like a basic, customizable contact form in their Rails applications, but don't need to store messages in the database (in which case a basic scaffold might be your best bet). You should be able to get MailForm up and running in your app, tests included, in an hour or less.

### How does it work?

First things first&mdash;make sure you've got outgoing mail configured to your liking. I use Google-hosted mail for production, and [MockSMTP or Mailcatcher](http://everydayrails.com/2011/05/26/rails-smtp-development.html) in development. (One gotcha for using Google Apps for Domains to deliver mail: Google will always change the sender's e-mail address to the address of the account you're using for SMTP. To my knowledge there's no way to change this, at least not with the free service level.)

Once you've got that out of the way, set up MailForm as [outlined on the project's GitHub page](https://github.com/plataformatec/mail_form). Specifically, you'll install the gem via Bundler, then create a model for your contact form. Note the model inherits from `MailForm::Base`, not ActiveRecord.

The GitHub instructions also include tips on localizing the form, validating that e-mail addresses are properly formatted, deterring spam, and adding your own fields. This is just a matter of adding attributes to your model (with corresponding fields in your form view, below). Once you've gotten this far you can test your new mail form in the Rails console&mdash;to implement your final interface, though, you'll need a controller and a couple of views. Here's how I did it.

First, the controller: I used the following generator to start the form, its views, and some starter specs:

{% highlight bash %}
  $ rails generate controller contact_form new create
{% endhighlight %}

Even though I'm only using the two create-related methods, I made it resourceful for easier routing and path generation:

{% highlight ruby %}
  # config/routes.rb
  resources :contact_forms
{% endhighlight %}

And my actual controller:

{% highlight ruby %}
  # app/controllers/contact_forms.rb
  class ContactFormsController < ApplicationController

    def new
      @contact_form = ContactForm.new
    end

    def create
      begin
        @contact_form = ContactForm.new(params[:contact_form])
        @contact_form.request = request
        if @contact_form.deliver
          flash.now[:notice] = 'Thank you for your message!'
        else
          render :new
        end
      rescue ScriptError
        flash[:error] = 'Sorry, this message appears to be spam and was not delivered.'
      end
    end
  end
{% endhighlight %}

One thing I decided to do here that may be atypical: I am not redirecting following a successfully sent message; rather, I've got a view for my `create` method that I'll render upon completion of the method.

My views are relatively basic. `new.html`, written here using Haml and [SimpleForm](https://github.com/plataformatec/simple_form) (another great Platforma Tec project), is doing a couple of things. First, it is specifying the fields are not required&mdash;this is for SimpleForm's sake; the validations in the model still do their thing. Second is the `hidden` CSS class I'm wrapping around my _nickname_ field. This field is used by MailForm to determine if a message is coming from a spambot. The theory goes that if the field is hidden to humans, and the field is filled in, the message is probably spam. So far this has kept unwanted messages from hitting my inbox for this project.

{% highlight haml %}
  # app/views/contact_forms/new.html.haml
  = simple_form_for @contact_form do |f|
    = f.input :name, :required => false
    = f.input :email, :required => false
    = f.input :message, :as => :text, :required => false
    .hidden
      = f.input :nickname, :hint => 'Leave this field blank!'
    = f.button :submit, 'Send message'
{% endhighlight %}

I don't have any custom fields, but if I did including them would just be a matter of adding a line for each to my form, using the appropriate SimpleForm helper.

If you're not using SimpleForm, just use a regular Rails `form_for` tag, and create your form's labels and fields accordingly.

My view for the `create` method is very simple.

{% highlight haml %}
  # app/views/contact_forms/create.html.haml
  %h1 Thank you for your message.

  %p
    We'll get back to you as soon as possible.

  %p
    = link_to 'Home', root_url, :class => 'button'
{% endhighlight %}

Finally, a quick note about tests/specs. In the app in question, we use RSpec Capybara, model specs, and request/integration specs to test our software. The model spec tests validations, while the request specs test actual implementation. Borrowing heavily from Ryan Bates' must-view Railscasts episode [How I Test](http://railscasts.com/episodes/275-how-i-test), here are some sample integration tests for MailForm:

{% highlight ruby %}
  # spec/requests/contact_forms.rb
  require 'spec/spec_helper'

  describe "ContactForm" do
    it "delivers a valid message" do
      visit new_contact_form_path
      fill_in 'Name', :with => 'Aaron Sumner'
      fill_in 'Email', :with => 'aaron@everydayrails.com'
      fill_in 'Message', :with => 'What a great website.'
      click_button 'Send message'
      page.body.should have_content('Thank you for your message')
      last_email.to.should include('help@everydayrails.com')
      last_email.from.should include('aaron@everydayrails.com')
    end

    it "does not deliver a message with a missing email" do
      visit new_contact_form_path
      fill_in 'Name', :with => 'Aaron Sumner'
      fill_in 'Message', :with => 'What a great website.'
      click_button 'Send message'
      page.body.should have_content("Email can't be blank")
      last_email.should be_nil
    end

    it "does not deliver spam" do
      pending "This does not appear to render the proper source but passes in a browser."
      visit new_contact_form_path
      fill_in 'Name', :with => 'Aaron Sumner'
      fill_in 'Email', :with => 'spammer@spammyjunk.com'
      fill_in 'Message', :with => "All the junk you'll never need."
      fill_in 'Nickname', :with => 'Want to buy some boots?'
      click_button 'Send message'
      page.body.should have_content('this message appears to be spam')
      last_email.should be_nil
    end
  end
{% endhighlight %}

### Conclusion

There you go, everything you need to add a straightforward, tested mail form to your Rails application. Thanks to Jos&#233; and Carlos for the useful gem, and thanks to you for coming back to Everyday Rails. I'll do my best to be more of a regular around here myself.