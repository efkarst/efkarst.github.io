---
layout: post
title:      "Building Forms with Associated Models - Ruby on Rails"
date:       2019-01-01 20:25:39 +0000
permalink:  building_forms_with_associated_models_-_ruby_on_rails
---


It's common to have user fill out forms associated with multiple models on the backend. For example, let's say you are building an recipe app where user can share recipes. When a user is creating a new Recipe, they provide the recipe title, instructions, ingredients and categories. This seems pretty straight forward, but if you want to use those ingredients and categories in other ways in your app (e.g. as search filters), you'll need to create Ingredient and Category models. 

In this case when a user fills out a form to create a new recipe, a new Recipe instance along with one or more Ingredient and Category instances will be created or associated on the back end. There are many permutations for how the user could provide category or ingredient data in a create recipe form. Let's look at a few examples:
* User can create a new category by name
* User can create or add to an existing category by name
* User can add their recipe to existing categories
* User can add several ingredients to the recipe by providing ingredient names and quantities


### Create a new category by name

If the user wants to create a new Category to add their recipe to, they should be able to do so by providing a Category name. To enable this, we simply need to add a `:category_name` field to our form

```
<%= form_for @recipe  do |f| %>
  <%= f.label :title%>
  <%= f.text_field :title%>

  <%= f.label :instructions%>
  <%= f.text_area :instructions%>

  <%= f.label :category_name, :category %>
  <%= f.text_field :category_name %>
  <%= f.submit %>
<% end %>
```

When a user submits this form, their `params` has will contain the following key-value pair:

```
{ :recipe => {
  :title => "recipe title",
  :instructions => "recipe instructions"
  :category_name => "category name"
  }
```

This looks great, because we can use mass assignment with the `params[:recipe]` value to create a new recipe. But there is one problem: there isn't a column in the recipes database table called `:category_name`, so Active Record won't know how to deal with it and you'll get an error. 

To solve this, a custom `#category_name= ` setter method can be added to the Recipe class. 

```
class Recipe < ActiveRecord::Base
  def category_name=(name)
    self.category = Category.find_or_create_by(name: name)
  end
 end
```

This will either associate an existing Category with the recipe, or create a new Category with the provided name if one does not exist. The setter method `#category_name=` is called whenever a Recipe is initialized with a `category_name` field. The `Recipe.create` action will now rely on this method rather than falling back to setting `category_name` through Active Record. We have successfully created a "virtual" attribute for Recipe, with a setter method that handles model associations.

Note that you'll need to permit this new `category_name` attribute in the Recipe controller for the user to successfully submit the form and create a recipe:

```
class RecipesController < ApplicationController
  def create
    Recipe.create(recipe_params)
  end
 
  private
 
  def recipe_params
    params.require(:recipe).permit(:category_name, :title, :instructions)
  end
end
```


### Create or add to an existing category by name

The `#category_name=` setter method above actually does allow the user to either create a new category, or associate their recipe with an existing one. However, it would be better if the user could see the full list of existing categories and choose from them as well!

HTML5 provides a new `datalist` element that is perfect for this scenario. This element allows for easy autocomplete. The user will see a "Category" text field like above, but when they begin typing they will see a drop down with autocomplete suggestions from a provided data list.

```
<%= form_for @recipe do |f| %>
  <%= f.text_field :category_name, list: "categories_autocomplete" %>
    <datalist id="categories_autocomplete">
      <% Category.all.each do |category| %>
        <option value="<%= category.name %>">
      <% end %>
    </datalist>
<% end %>
```

As a user begins typing, they will see suggested names from `Category.all` that they can use to auto-complete the text in the text field. They can also press on a down arrow in the text field to see the full list of category names before they begin typing.


### Add the recipe to many existing categories

In some cases you may not want users to be able to create new categories. You may have a very catered list of categories that you organize your Recipe site around and don't want users to change them. In that case, you may want user to be able to select one or more categories that their Recipe could fall under. Luckily, Active Record associations provide a method that makes this easy to do.

For this example assume that a recipe has many categories and a category has many recipes, so these models are associated through a join table, recipies_categories. Because a recipe has many categories, we have access to an Active Record method called `category_ids=` (note, this method would also be available if this were a many-to-one relationship rather than a many-to-many relationship). We'll use this method to create a collection of Category checkboxes that the user can choose from:


```
<%= form_for @recipe  do |f| %>
  <%= f.label :title%>
  <%= f.text_field :title%>

  <%= f.label :instructions%>
  <%= f.text_area :instructions%>

  <%= f.collection_check_boxes :category_ids, Category.all, :id, :name %>

  <%= f.submit %>
<% end %>
```


The collection_check_boxes method will generate a series of HTML checkbox strings:

`<input type="checkbox" value="1" name="recipe[category_ids][]" id="recipe_category_ids_1">`

One checkbox is generated for each category in Category.all. The :id and :name attributes in the form specify the value (:id) and string the user will see by the checkbox in the view (:name). Note that ActionView nests the array of category_ids in the recipe key of the params hash (to enable mass assignment).

If the user checked two categories with IDs 3 and 4, they would appear as follows in the params hash:
 
```
{"title"=>"New Post", "content"=>"Some great content!!", "category_ids"=>["3", "4"]}
```

When you actually create the recipe with `Recipe.create(recipe_params)`, the `category_ids=` method provided by Active Record's `has_many` macro will be used to associate the selected categories to the new recipe. A new recipe will be added to the recipes table, and two rows will be added to the recipies_categories join table with the two category ID's and the recipe ID (automatically, thanks to Active Record).

Note that you'll need to permit `category_ids` in the Recipe controller for the user to successfully submit the form and create a recipe. You'll need to specify that this method accepts an array of values.

```
class RecipiesController < ApplicationController
...
 
private
 
  def recipe_params
    params.require(:recipe).permit(:title, :instructions, category_ids:[])
  end
end
```


### Add ingredients with nested attributes

Adding ingredients with nested attributes (name, quantity) to a recipe is done in a very similar way to how we helped the user add a category by name in the first example above. We'll structure our form to nest each ingredient's attributes under the recipe key in the params hash, and build a custom setter method to actually build and associate the ingredients with the recipe. 

With nested attributes, we will want the our recipe params to have the following structure:

```
{
  :recipe => {
    :title => "recipe title",
    :instructions => "recipe instructions",
    :ingredients_attributes => {
      "0" => {
        :name => "ingredient name",
        :quantity => "ingredient quantity"
      }
      "1" => {
        :name => "ingredient name",
        :quantity => "ingredient quantity"
      }
    }
}
```

This allows you to nest a hash of attributes for each ingredient you add to the recipe (so you can access or create ingredients easily for each). This is also the default structure provided by the `form_for` ActionView form builder if you use the `fields_for` helper method, and is convention for using Active Record's `accepts_nested_attributes_for macro ` (which I will touch on below). 

`form_for` can be used to generate this params structure as follows:

```
<%= form_for @recipe do |f| %>
  <%= f.label :title %>
  <%= f.text_field :title %><br>

  <%= f.label :instructions%>
  <%= f.text_field :instructions %><br>

  <%= f.fields_for :ingredients do |ing| %>
    <%= ing.label :name %>
    <%= ing.text_field :name %><br>
 
    <%= ing.label :quantity %>
    <%= ing.text_field :quantity %><br>

  <% end %>

  <%= f.submit %>
<% end %>
```

This form will automatically generate fields *for the number of ingredients already associated with the recipe*. Upon creation, there are no ingredients associated with a recipe so no fields will appear. To work around this you can stub out the desired number of ingredients in the #new action of the Recipe controller. 

```
class RecipeController < ApplicationController
  def new
    @recipe = Recipe.new
    @recipe.ingredients.build()
    @recipe.ingredients.build()
  end
end
```

This will stub out two ingredients associated with the recipe, so the user will see two sets of ingredients fields in the new recipe form.

Now that the form is rendering correctly, we need to ensure there is a setter method for `:ingredients_attributes`. This can be done in a couple of ways. 

1 -- Use Active Record's `accepts_nested_attributes_for` macro in the Recipe model.This automatically generates a `#ingredients_attributes` setter method for the Recipe model.

```
class Recipe < ActiveRecord::Base
  has_many :ingredients
  accepts_nested_attributes_for :ingredients
end
```

2 -- If the default method provided by `accepts_nested_attributes_for` doesn’t meet your needs, you can write a custom setter method. One of the benefits of writing a custom setter method for this example is avoiding duplicate ingredient instances in a recipe.

```
class Recipe < ActiveRecord::Base
  has_many :ingredients
	def ingredients_attributes=(ingredient)
	  ing = self.ingredient.find_or_create_by(name: ingredient.name)
		ing.update(ingredient)
	end
end
```

Finally, you'll need to permit `ingredients_attributes` in the Recipe controller for the user to successfully submit the form and create a recipe with the associated ingredients. You'll need to specify that this method accepts an array of value, and what the permitted values are.

```
def recipe_params
  params.require(:recipe).permit(:title, :instructions, ingredients_attributes: [ :name, :quantity ])
end
```

### Take-Aways

In summary, you can build whatever model attributes you want as long as they have a corresponding setter method and are permitted or required by Strong Parameters. The above examples demonstrate some common conventions, but more generally here are the steps I follow when thinking about how to build forms that require me to create or update associated models:

1. Determine the fields you want in the form. What data should the user have to enter, and how does that data correlate to my models?
2. Design your form in a way that ensure the attribute data entered nests under the key of the primary model you are creating or updating (like we did for recipe, above). Nesting all model data under a primary model key enables mass-assignment. If you use `form_for` correctly, attributes should be nested under the key of the primary model by default. If using `form_tag` or writing raw HTML, you will need to pay attention to how the name field in the HTML tag is structured to ensure you nest params correctly.
3. Ensure you have setter methods that correlate to each attribute in your form. You can write custom methods directly in your Model to create "virtual" attributes (like `#category_name` and `#ingredients_attributes` above), or depend on setter methods from Active Record (this could be `#attribute= methods` that correlate to database table columns, methods from association macros like `has_many`, or methods from the `accepts_nested_attributes_for` macro). 
4. Ensure you permit all fields in your controller action with Strong Parameters.




