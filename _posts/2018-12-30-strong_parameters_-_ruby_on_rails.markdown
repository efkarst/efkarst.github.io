---
layout: post
title:      "Strong Parameters - Ruby on Rails"
date:       2018-12-31 01:30:39 +0000
permalink:  strong_parameters_-_ruby_on_rails
---


Strong parameters were introduced to Ruby on Rails (starting in Rails 4+) to protect attributes from end-user assignment by requiring that developers whitelist parameters that are permitted when sending form data to the database. Without this type of mass-assignment protection, it is fairly easy for an end user to hack a form to manipulate model attributes. For example, an end user could update the balance in their bank account by hacking a form in a view of their bank's website if attributes like account balance aren't protected. 

Strong parameters also make developers happy by moving mass-assignment protection out of the model and into the controller, allowing the controller to do it's job controlling the flow of data between the user and the application (including authentication, authorization and access control). 

### Whitelisting attributes

If you submit a form with attributes that have not been whitelisted, you will see a ForbiddenAttributesError. The Rails default is to let nothing through. Rails developers have two methods available to whitelist attributes in params: #require and #permit.

Let's say we're building a recipe app that allows users to share recipes. When creating a recipe, we must explicitly permit users to add a recipe name, instruction, and ingredients.

```
def create
  @recipe= Recipe.new(params.require(:recipe).permit(:name, :instructions, 
	        ingredients_attributes: [:name, :quantity]))
  @recipe.save
  redirect_to recipe_path(@recipe)
end

```

The *#require *method requires that the params that get passed must contain a key called "recipe". If there is not a "recipe" key then it fails you get an ActionController::ParameterMissing error. The *#permit* method means that params may have a name, instructions, and ingredients_attributes, but does not require them for the user to successfully submit the form. If a non-permitted attribute, like equipment_needed, is added to params you will see a ForbiddenAttributesError.

You may have noticed that the :ingredients_attributes looks a bit different. This is an example of how to use the *accepts_nested_attributes_for macro* with Strong Parameters. In this example, a recipe has many ingredients (where each ingredient is an instance of an Ingredient model). To permit nested Ingredient attributes, we must both permit the ingredients_attributes (a Recipe writer method provided by *accepts_nested_attributes_for*) and individual ingredient attributes like name and quantity.

You can see [ActionController::StrongParmameters ](https://edgeapi.rubyonrails.org/classes/ActionController/StrongParameters.html) for more information on Strong Parameters and their associated methods.

### DRY Strong Parameters Convention

Permitting attributes quickly becomes repetitive across controller actions. Thus, it is convention to create a private method in the controller to whitelist parameters once. In this example, we create a *#recipe_params* method. It is convention to create this private method named *model_params* where 'model' is the singular, lowercase name of the model whose attributes are being created or updated.

```
def create
  @recipe= Recipe.new(recipe_params)
  @recipe.save
  redirect_to recipe_path(@recipe)
end
 
def update
  @recipe= Recipe.find(params[:id])
  @recipe.update(recipe_params)
  redirect_to recipe_path(@recipe)
end
 
private
 
def recipe_params
  params.require(:recipe).permit(:name, :instructions, ingredients_attributes: [:name, :quantity])
end

```

This makes for much DRYer code, and saves you from updating multiple lines of code across create and update actions if the database schema changes. 


To prevent users from updating certain attributes, like the recipe name, after they initially created a Recipe you can specify which recipe_params are permitted as follows:

```
def update
  @recipe= Recipe.find(params[:id])
  @recipe.update(recipe_params(:instructions, ingredients_attributes: [:name, :quantity]))
  redirect_to recipe_path(@recipe)
end

```


### Sources

* [Learn](https://learn.co/tracks/full-stack-web-development-v6/rails/crud-with-rails/strong-params-basics)
* [weblog.rubyonrails.org](https://weblog.rubyonrails.org/2012/3/21/strong-parameters/)
* [edgeapi.rubyonrails.org](https://edgeapi.rubyonrails.org/classes/ActionController)

