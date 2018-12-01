---
layout: post
title:      "Password Encryption in a Sinatra App"
date:       2018-12-01 23:00:24 +0000
permalink:  password_encryption_in_a_sinatra_app
---

Passwords can be secured in Sinatra apps using the Ruby gem 'bcrypt'. Bcrypt provides a sophisticated and secure hash algorithm for encrypting passwords in a Ruby application.

### Setting up password encryption
To set up password encryption in a Sinatra app:

1. Include the bcrypt gem in your gemfile: "gem 'bcrypt'"

2. Include Active Record's *has_secure_password* macro in the user model. This macro provides access to three new methods, detailed below.
    ```
		class User < ActiveRecord::Base
		  has_secure_password validations: false
		end
		```
		
		
3. Include a *password_digest* column in the "users" database table. Here's an example of how you might create a table with this column using an ActiveRecord migration:
    ```
		create_table :users do |t|
		  t.string username
			t.string password_digest
		end
		```
		

### Using has_secure_password
The has_secure_password macro provides access to three new methods:

**password=(unencrypted_password)**

```
# encrypts the password into the password_digest attribute, if the new password is not empty

class User < ActiveRecord::Base
  has_secure_password validations: false
end

user = User.new
user.password = nil
user.password_digest # => nil
user.password = 'mUc3m00RsqyRe'
user.password_digest # => "$2a$10$4LEA7r4YmNHtvlAvHhsYAeZmk/xeUVtMTYqwIvYY76EW5GUqDiP4."
```

**password_confirmation=(unencrypted_password)**

```
# optional confirmation field for password

user = User.new(name: 'Davey', password: 'password', password_confirmation: 'password')
user.save
```


**authenticate(unencrypted_password)**

```
# returns self if the password is correct, or false if it is not correct

class User < ActiveRecord::Base
  has_secure_password validations: false
end

user = User.new(name: 'david', password: 'mUc3m00RsqyRe')
user.save
user.authenticate('notright')      # => false
user.authenticate('mUc3m00RsqyRe') # => user
```

### Creating a new user with a secure password
With these methods, creating a user account with a secure password is simple. Here is an example of how you might create a user with signup data from a signup form in a Sinatra app:

```
@user = User.new(params[:user])
@user.password = params[:password])
@user.save
```

An encrypted password is saved to the database rather than the unencrypted password the user entered.

### Authenticating a user a login
The has_secure_password macro also makes it easy to authenticate and log in a user. Here is an example of how you might grab login data from a login form in a Sinatra app and authenticate a user:

```
user = User.find_by(username: params[:username])

# if the user exists and is authenticated with the entered password, log in is successful
if user && user.authenticate(params[:password])
  session[:user_id] = user.id
  redirect '/success'
else
  redirect '/failure'
end
```

And that's all there is to a simple, encrypted authentication system!

###### Sources
* [Github](https://github.com/codahale/bcrypt-ruby)
* [Rails Guides](https://api.rubyonrails.org/classes/ActiveModel/SecurePassword/InstanceMethodsOnActivation.html)
* [Learn.co](https://learn.co/tracks/full-stack-web-development-v6/sinatra/activerecord/securing-passwords-in-sinatra)
* [Engine Yard](https://www.engineyard.com/blog/password-security-part-2)


