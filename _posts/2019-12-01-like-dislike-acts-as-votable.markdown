---
layout: post
title:  "Like and dislike buttons - using acts_as_votable"
date:   2019-12-01 09:00:20 +1300
categories: rails
description: "How to use the acts_as_votable gem to quickly implement like/dislike buttons"
---

I needed to implement a set of simple like/dislike actions to a Rails project and was inspired by the simple like and dislike buttons on [ProductHunt](https://www.producthunt.com). I struggled to find a well written guide on how to implement [acts_as_votable](https://github.com/ryanto/acts_as_votable) asynchronously with Rails 6, so have put the following quick guide together which I hope gives a fairly complete example.

See below for a gif demonstration. For some reason the cursor isn't visible - but hopefully self explanatory.

![Like and dislike GIF](/images/likedislike.gif)


### Gemfile

Using [Bootstrap](https://github.com/twbs/bootstrap-rubygem) and [Font Awesome](https://fontawesome.com/icons) in the code below.

```ruby
 gem 'acts_as_votable'
 gem 'devise'
 gem 'font_awesome5_rails'
 gem 'bootstrap', '~> 4.3.1'
```

Ensure you install and run the migration for any new gems.

### Model

I've used a _Comment_ model. You can use whichever model you want - but make sure to configure it appropriately.

### View

Fairly self explanatory. Using a helper function (see below) to manage the html class of the button. I have an identical one for the dislike action (opposite of like).

The _comment.get_likes.size_ returns the number of comments. You can wrap this up in a helper to only show a count if >0.

I'd recommend using actioncable to get the vote count to automatically update.

```ruby
<%= link_to like_comment_path(comment.id), remote: true, id: "like_#{comment.id}", class: "btn #{liked_comment(comment)} btn-sm btn-block" do %>
    <%= fa_icon "smile" %> 
    <%= comment.get_likes.size %>
<% end %>
```

### Controller

Take note of the format.js

```ruby
def like
    if @comment.liked_by current_user
        respond_to do |format|
        format.html { redirect_to :back }
        format.js
        end
    end
end
```
### Router

Unlike is the route I've used for un-liking a comment. Dislike and undislike are for disliking a post and un-disliking (opposite of disliking).
These routes are essentially 'nested' under the comments resource.

```ruby
  resources :comments do
    member do
      get 'like'
      get 'unlike'
      get 'dislike'
      get 'undislike'
    end
  end
```

### Helper

I used a helper to set the html class of the button on page load. For example, this will return a 'solid' button when the button is clicked and the user has either voted_up or voted_down on the particular comment. You can skip this functionality if you would prefer to do in view or just not have this functionality.

```ruby
module CommentsHelper
    def liked_comment(comment)
            return 'btn-liked' if user_signed_in? && (current_user.voted_up_on? comment)
            'btn-like'
    end
   
    def disliked_comment(comment)
        return 'btn-disliked' if user_signed_in? && (current_user.voted_down_on? comment)
        'btn-dislike'
    end
end
```

