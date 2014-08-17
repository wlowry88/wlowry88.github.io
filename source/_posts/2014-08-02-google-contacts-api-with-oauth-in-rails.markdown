---
layout: post
title: "Google OAuth 2.0 Login in Rails 4"
date: 2014-08-02 23:06:27 -0400
comments: true
categories: 
---

The process of logging in with Oauth may seem complex the first time you do it, but it becomes easier the second time around. This article assumes some familiarity with Ruby and the structure of a Rails application (but not too much :)

<!--More-->

###Step 1: Set Up a Google App

First, go to this URL and 

1) Set up a developer account and
2) Create a new project:

https://code.google.com/apis/console

You will see that you now have access to your project on the Projects page. Click into your project and then go to the credentials tab (in the left menu).

Note that the client ID and client secret are important - we'll be using those later. In the meantime, make sure that your redirect uri is the following:

http://localhost:3000/auth/google_oauth2/callback

This is important because it's where Google will send info on the user who logged in.

###Step 2: Update your Gemfile 

Make sure your Gemfile includes the Google OmniAuth gem as well as the Figaro gem. Figaro is helpful in keeping your key and secret confidential (you can learn more about it [here](http://rubydoc.info/gems/figaro/0.7.0/frames)).

`gem "omniauth-google-oauth2"`
`gem "figaro"`

Run a bundle install and update. This will create a few files, including the ominauth.rb file in your initializers directory.

###Step 3: Configuring Routes

In your routes file, make sure that there is something that looks like the following:

`get 'auth/:provider/callback', to: 'sessions#create'`

After clicking your future "Log In With Google" button, Google will send back a hash with lots of information on the user. The information will be send using this route.

###Step 4: Generate Sessions Controller and Create Action

Run the following command:
`rails g controller sessions create`

Then, in your create action in the sessions controller, use the following:

```ruby
class SessionsController < ApplicationController
  def create
  	@user = User.from_omniauth(@auth)
  	@auth = env["omniauth.auth"]
  	session[:user_id] = @user.id
  	redirect_to root_path
  end
  ...
end
```
Now, when the information is sent to the URL we already specified, it will be routed to the create action of our Sessions controller. It will then use the hash returned by OmniAuth to create a user.

###Step 5: Update User Model and Define from_omniauth

As many of you astute readers will have realized, we will need to define a method called "from_omniauth" in our User model for the above code to work. Let's do that as well.

```ruby
	def self.from_omniauth(auth)
    where(auth.slice(:provider, :uid)).first_or_initialize.tap do |user|
      user.provider = auth.provider
      user.uid = auth.uid

      user.oauth_token = auth.credentials.token
      user.oauth_expires_at = Time.at(auth.credentials.expires_at)
      user.save
    end
  end

```

You may need to create a migration to add the `provider`, `uid`, `oauth_token`, and `oauth_expires_at` columns to your `users` table. The following line will do it for you if you haven't:

`rails g migration AddDetailsToUser provider uid oauth_token oauth_expires_at`

And then you simply migrate your database to reflect those changes:

`rake db:migrate`

###Step 6: Configure Ominauth.rb Initializer

So far, we have set up a mechanism to recieve the information back from Google when a user logs in. Now, we'll make sure we also configure the sending of the request.

You have a file called "omniauth.rb" in your 'config/initializers/' directory. Make the contents of the file look like this:

```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :google_oauth2, ENV[GOOGLE_KEY], ENV[GOOGLE_SECRET], {
    redirect_uri:"http://localhost:3000/auth/google_oauth2/callback"
  }
end
```
This makes sure that the OAuth request is built with the right id and secret. You're probably wondering what the hell "GOOGLE_SECRET" are. They look like pretty random uninitialized constants. Let's fix that.

###Step 7: Your application.yml File

When you included the Figaro gem, you were actually taking the first steps to protecting your app's secret information. You don't want to post that on Github - it makes it too easy for someone to come along and steal it. Instead, we're using Figaro, which allows you to put all your secrets in one file, application.yml, which is automatically "gitignored." This way, it won't be public. Your application.yml file should look like this:

```ruby
GOOGLE_KEY: "2asflskf103f_not_a_real_key_don't_try_and_use_it"
GOOGLE_SECRET: "2asl2tzz_ditto_on_this_one"
```
Restart your server if it's running - initializers are rerun upon server start!

###Step 8: Include Your Link or Google Button, and Enjoy!

Now you can include a link in your project anywere you want people to be able to sign in with Google. Some natural spots might be a login page or on your navbar, but it's up to you!

The url of the link should be `/auth/google_oauth2`. This doesn't have the `/callback` appended to it because the callback is for- you guessed it- the callback.

###Step 9: Dominate the World Vicariously Through Google

Tada! You're done!









