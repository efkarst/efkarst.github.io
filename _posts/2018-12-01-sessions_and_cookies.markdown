---
layout: post
title:      "Sessions and Cookies"
date:       2018-12-01 21:26:51 +0000
permalink:  sessions_and_cookies
---


Web apps use sessions and cookies to maintain state in an otherwise stateless series of HTTP requests and responses. When a user first connects to a web server the server creates a session, which is simply a object or hash used to store relevant data such as user login information or items in a shopping cart. The server copies this data to a cookie (another hash), which is sent back to the client and stored in the browser until it expires or is deleted. Cookies are sent back to the web server along with every subsequent HTTP request from the client. The web server compares cookie data with server-side session data to help the application determine what information to send back to the user for each request. The web server can update the cookie data as part of the HTTP response. Cookies are the client-side counterpart to server-side sessions. Their handshake enables persistence across HTTP requests and responses.

There are actually two types of cookies: 
1. Session cookies keep track of user data until they log out or navigate away from a website. They are stored in the users web browser. Session cookies enable a user to stay logged in or maintain a shopping cart for the duration of their time on a website.
2. Persistent cookies allow a website to remember user information for future visits by storing it on your computer. Persistent cookies enable things like faster page loading and automatic user login. 


### An Example: User Login and Logout
Here is a simple overview of how a developer can use session data for user login and logout in a Sinatra app.

First, in your application controller you must configure sessions:

```
class ApplicationController < Sinatra::Base
  configure do
    enable :sessions
    set :session_secret, "secret"
  end
end
```

In practice, you would replace "secret" with something more secretive.

**Login**: in the POST request from a user login form, you populate the session with the appropriate user data:

```
post '/users' do
    # Find the user in your database
	 @user = User.find_by(email: params["email"], password: params["password"])
	
	# Populate the user ID in the session
	session[:id] = @user.id
	
	# Redirect the user to their landing page
	redirect '/users/:id'
end
```

For the duration of the session, the browser will store a cookie containing an entry: 
```
{:id => ####}
```

where the value is the user ID specified by @user.id above. This is sent to the web server with each HTTP request, authenticating the user and enabling the web app that it send user data back to the user in the HTTP response.

**Logout**: simply clear the session hash.

```
get '/sessions/logout' do 
  # Clear the session hash
	session.clear
	
  # Redirect the user to the index page
	redirect '/'
end
```

Once you clear the session hash, a user will have to log in again to see any of their data.

### Sources
* [F5](https://www.f5.com/services/resources/white-papers/cookies-sessions-and-persistence)
* [Learn.co](https://learn.co/tracks/full-stack-web-development-v6/sinatra/sessions/sessions-and-cookies)

