---
layout: post
title:      "RESTful Routing with Sinatra"
date:       2018-12-01 20:40:44 +0000
permalink:  restful_routing_with_sinatra
---


Most dynamic web sites allow users to create, read, update and delete data. The internet would be a total free-for-all if there wasn't some convention for how to do this, making navigation and data manipulation hard to follow for users and developers alike. 

RESTful routing provides a nice convention for mapping HTTP verbs (get, post, put, delete, patch) to CRUD actions (create, read, update, delete). The key to REST is using conventional routes to do this mapping.

Here is an overview of the basic route conventions using an example site that allows users to create, read, edit, and delete recipes:

| **HTTP Verb** | **Route** | **Action** | **Use** |
| -------- | -------- | -------- |  -------- |
| GET     | '/recipes'      | index action     | Index page to display all recipes     |
| GET     | '/recipes/new'      | new action     | displays create recipe form     |
| POST     | '/recipes'      | create action     | creates a recipe     |
| GET     | '/recipes/:id'      | show action     | displays the recipe with the ID in the url     |
| GET     | '/recipes/:id/edit'      | edit action     | edit the recipe with the ID in the url     |
| PATCH     | '/recipes/:id'      | update action     | modifies the recipe with the ID in the URL     |
| PUT     | '/recipes/:id'      | update action     | replaces the recipe with the ID in the URL     |
| DELETE     | '/recipes/:id'      | delete action     | deletes the recipe with the ID in the URL     |


In Sinatra, a route is an HTTP method paired with a URL-matching pattern in an application controller. Each route is associated with a controller action block. Routes are matched in the order they are defined. Here are the Sinatra methods you would use for the same recipe example:

```
get '/recipes' do
  # render index page displaying all recipes
end

get '/recipes/new' do
  # render page with form to create a recipe
end

post '/recipes' do
  # create a new recipe and redirect to wherever developer chooses
end

get '/recipes/:id' do
  # render page that displays the recipe with the ID that matches the URL
end

get '/recipes/:id/edit' do
  # render page with form to edit the recipe with the ID that matches the URL
end

patch '/recipes/:id' do
  # modify the reciple with the ID that matches the URL, and redirect to /recipes/:id (or wherever developer chooses)
end

put '/recipes/:id' do
  # replace the recipe with the ID that matches the URL, and redirect to /recipes/:id (or wherever developer choses)
end

delete '/recipes/:id' do
  # delete the recipe with the ID that matches the URL, and redirect to /recipes (or wherever developer chooses)
end
```





###### Sources
* [Learn.co](https://learn.co/tracks/full-stack-web-development-v6/sinatra/activerecord/sinatra-restful-routes)
* [sinatrarb](http://sinatrarb.com/intro.html#Routes)
