---
tags: rails, sessions, WIP
language: ruby
resources: 1
---

# Rails Blog: Authentication from Scratch using Sessions

This is the fifth iteration of our Blog App.

We built out a user model a while back, but we haven't been doing much with it yet. Let's build out a signup and login system that prevents a user from writing a blog post or a comment without logging in. For now, we're going to build our own log in system using sessions.
 
## Signing up

1. Create a new migration for our users table that makes the following columns: email and password_digest (both strings).
2. Add the 'bcrypt' gem to our Gemfile to use the Active Record [has_secure_password method](http://api.rubyonrails.org/classes/ActiveModel/SecurePassword/ClassMethods.html#method-i-has_secure_password), which adds methods to set and authenticate against BCrypt passwords. We don't need to create validations for password presence, because it's included within this method.
3. Now we have password and password_confirmation attributes (which we can permit in params on the users controller). Note that these aren't columns on our database, but attributes handled by the bcrypt gem.
4. Let's fix our routes to have a route for signup that points to the new method on our users controller.
5. On the create method in the users controller, point a successful sign up to a root_path (posts index page) that we'll make in our routes file.
6. Update our form for creating a new user to include email, password, and password confirmation. The passwords should have a password_field input.
7. Let's also build out a _header partial that we render in application.html.erb that will have a Sign Up link, and other helpful navigation links.

## Logging in with Sessions

Now that we have our sign up process built out, we can expand it into a log in system. A user who is signed up should be able to log in to see certain pages. We'll be using sessions, which allow an application to keep track of a user and allow them to perform actions that are remembered by the application without authenticating on every request. A session is comprised of a hash of values, usually a user's id and other information, as well as a session id. Learn more from the documentation on [sessions in rails](http://guides.rubyonrails.org/security.html#sessions).

1. Generate a Sessions controller. We'll make the views later.
2. Our sessions controller will handle two actions, creating a new session and destroying it. A new session is generated when a user logs in or signs up (which is handled on the users controller by the create method.
3. In the create method, you will need to find a user by their email and authenticate it (calling `.authenticate`) on the password from params. Then assign the `session[:user_id]` to the user.id. Check out this blog post on [authentication](http://www.millwoodonline.co.uk/blog/using-hassecurepassword-for-rails-authentication) for more info.
4. Build out the appropriate routes for these actions, and include a resource for sessions.
5. Add to your _header nav links for logging out and loggin in, and render a form as a form_tag for logging in under sessions (make it a partial and render it in a new view); it should post to the sessions_path. 

### Bonus

1. In the application controller, make a private `helper_method` called `current_user` that will find a user by their session id and assign to an instance variable `@current_user`. This will help us in our header to render info about the logged in user, and create some view logic to only render a logout link if the user is logged in. Have the current user's name displayed too to let them know they're logged in.