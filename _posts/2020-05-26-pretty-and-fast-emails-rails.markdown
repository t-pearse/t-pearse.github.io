---
layout: post
title:  "Make pretty HTML emails with Rails using Sendgrid Templates"
date:   2020-05-26 09:00:20 +1300
categories: rails
description: "Supercharging actionmailer in Rails with Sendgrid email templates. HTML and mobile friendly emails at pace."

---

While working on a new Rails project I needed to start building out mailer functions. Previously my preferred method has been building all the views within Rails using a library like [Inky](https://github.com/foundation/inky-rb) or [Premailer-Rails](https://github.com/fphilipe/premailer-rails) and a lot of testing. Usually the resulting email views are below expectations but good enough.

I'd stumbled across [Sendgrid email templates](https://sendgrid.com/solutions/email-marketing/email-templates/) with easy _handlebar_ markup and easy to use theme editors. This presented a number of advantages:
- no more tedious html and inline-css template editing
- no-code editing to the templates (i.e. non-developers can tweak marketing emails)
- mobile/text/html emails all done
- quick and easy to setup (should take minutes to get a professional looking email)

I wrote this post as the documentation I had previously found online was a bit patchy and difficult to debug - particularly around authenticated senders and require fields in actionmailer.


#### Install gems

Install the following two gems to your app.

~~~~
/Gemfile
gem 'sendgrid-ruby'
gem 'sendgrid-actionmailer'
~~~~

Then run _bundle install_ to install the gems.

#### Get Sendgrid API keys

Get your [API keys](https://app.sendgrid.com/settings/api_keys) from Sendgrid (under the Email API button) and set an aptly named ENV variable using whatever method you use to manage your local environment variables.

~~~~
/.env file

SENDGRID_API_KEY: [put api key here]

~~~~


#### Update Actionmailer configuration

Update the environment configuration to tell Actionmailer to use the sendgrid_actionmailer gem and the API key we set earlier.

~~~~~~~
/config/environment.rb

  config.action_mailer.delivery_method = :sendgrid_actionmailer
  config.action_mailer.sendgrid_actionmailer_settings = {
    api_key: ENV['SENDGRID_API_KEY'],
    raise_delivery_errors: true
  }

~~~~~~~

#### Setup mailers

Run the following task to generate a mailer. In this case we are using a User notifier email and we will setup a welcome email.
~~~~~~
$ rails generate mailer UserNotifierMailer
~~~~~~

You will then need to create a view for this email, the view will be overwritten by the Sendgrid template, so it does not need to include anything.

~~~~~~
Create the file app/views/user_notifier_mailer/new_user.html.erb

[some text]
~~~~~~

#### Setup Sendgrid template

Setup a new dynamic template in Sendgrid. There are plenty of templates which can be modified and provide a good base template.

Handlebars can be used throughout the template, including the subject and pre-headers fields. 

Variables can be passed through from your app into the sendgrid template.

~~~~~~
{% raw %}

Hi {{userName}},

Welcome my site.

{{if (eq userCategory "business")}}
You have a business account.
{{#/if}}

{% endraw %}
~~~~~~

#### Authenticated Senders

You will need to add an authenticated sender in Sendgrid in order to successfully use the Sengrid API.

This setting is found under the settings tab [here](https://app.sendgrid.com/settings/sender_auth) within your Sengrid account.

This will be an email address you own and want to appear to your customers in the 'from' field. 


### Configure mailer

Replace sender@example.com with the authenticated sender configured.
Replace the template-id with the ID of the template you just created.

The body and subject are required for actionmailer but will be overwritten by the template settings within Sendgrid.

~~~~~~~
app/mailers/user_notifier_mailer.rb

class UserNotifierMailer < ApplicationMailer

    require 'sendgrid-ruby'
    include SendGrid

    def new_user
        @user = params[:user]
        mail(to: @user.email, from: 'sender@example.com', subject: 'Some subject', body: 'Some body', template_id: 'd-9f22c78fas7fa6f40ad826fasfasdc1371ec67', 
            dynamic_template_data: {
                userCategory: @user.category 
            }
        )
    end
end
~~~~~~~

#### All done

Monitor logs and monitor activity within Sendgrid to ensure all is working correctly.

In production, I have been using this configuration succesfully with Heroku. This was not using the Sendgrid add-on, rather just using the gems and configuration specified above.
