---
tags: rails, omniauth, oauth, authentication, wip
language: ruby
resources: 1
---

# Rails Blog: Signing in using Omniauth

This is the sixth iteration of our Blog App.

In our last iteration, we used Sessions and built our own log in system. Next, we're going to refactor that and create a log in system using OmniAuth. A user should be able to log in / sign up using their Github account.

1. The [omniauth](https://github.com/intridea/omniauth) and [omniauth-github](https://github.com/intridea/omniauth-github) gems are in the Gemfile. Be sure to read the readmes for both.

##Registering our App

2. Before we do anything, we should register our app with Github, which will generate a `client id` and `client secret`, two strings that will let our application talk to Github via OAuth. Register your app [here](https://github.com/settings/applications/new) in your Github settings. Be sure to provide a callback URL, which will be a route/method on your controller that you'll be building out to make your user authorization call to Github.

Your application registration should look something like this:

##Figaro

3. We do not want our client_id and client_secret available for everyone to see in our codebase, yet it's information that is necessary for our app to work. One way we can handle this is through [Figaro](https://github.com/laserlemon/figaro).

4. Run `rails generate figaro:install`. This creates the `config/application.yml` file and adds it to your .gitignore. Add your client id and client secret to this file.

##Setting up OmniAuth

5. In `initializers`, create a file called `omniauth.rb` which will include the `OmniAuth::Builder` class as middleware to our application. Omniauth can provide authentication via multiple providers (like Facebook, Twitter, Google+, and Github). In this case, we're only going to use Github.

Here's an example of how this should look, from the Omniauth gem README:

```ruby
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :twitter, ENV['TWITTER_KEY'], ENV['TWITTER_SECRET']
end
```

##Routes

6. Next, we need to build a route that will redirect users to `/auth/:provider` (provider being declared in `omniauth.rb`).

7. Once a user is authenticated, Omniauth sets a special hash. In order to get back that data, we need to set up an endpoint that matches the callback URL (which we set up when we registered our application on Github) which will be handled by `sessions#create`.

##Controllers

We will need to refactor our sessions and users controllers to include methods to handle our omniauth request and response.

###Sessions

###Users

##Changing our Users Table

1. Before, our app had a column for a password digest, which held a users encrypted password. Instead, we're going to remove that column and instead store the following, which comes back in the hash via the omniauth callback we set up:

## Resources

[Ruby 003 Manuel's blog post on OAuth 2.0](http://manu3569.github.io/blog/2013/11/06/oauth-2-dot-0-what-you-need-to-know-about-it-for-building-your-next-app/)

[Auth-Hash-Schema](https://github.com/intridea/omniauth/wiki/Auth-Hash-Schema)
