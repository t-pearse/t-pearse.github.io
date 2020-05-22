---
layout: post
title:  "Manage administrators in Rails - with a simple and secure task"
date:   2020-05-18 09:00:20 +1300
categories: rails
description: "How to manage adminisators with devise and a simple rake task, accessible from the console. Promote/demote and list current admins."

---

I have always wondered about the best, simplest and most secure way of assigning administrator privileges to users in a production environment while using [Devise](https://github.com/heartcombo/devise).

The generally accepted method is to have a boolean *admin* field in the user model. However in order to toggle this setting you can either seed administator records to your database, or temporarily have an editable field. You can also easily use an admin (like [activeadmin](https://github.com/activeadmin/activeadmin)) interface to manage user permissions. 

This seemed a bit crude. On a recent project I setup a task to promote/demote and list users to have administrator privileges.

~~~~
/lib/tasks/admin.rb

namespace :admin do
    email = ENV['email']
    
    task :promote => :environment do
        @user = User.find_by(email: email)
        @user.admin = 1
        @user.save!
        p ENV['email'] + ' promoted to administrator'
    end

    task :demote => :environment do
        @user = User.find_by(email: email)
        @user.admin = 0
        @user.save!
        p ENV['email'] + ' demoted to administrator'
    end

    task :list => :environment do
        @users = User.where(:admin => 1)
        p "Listing all administrators:"
        @users.each do |u|
            p   u.email
        end
    end
end
~~~~

From the console you can now easily run:

~~~~
rake admin:promote email=testuser@website.com
testuser@website.com promoted to administator

rake admin:list
Listing all administrators:
testuser@website.com
~~~~

Obviously this isn't perfect, but seems robust and secure enough for a small project.