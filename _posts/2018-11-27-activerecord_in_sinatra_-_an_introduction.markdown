---
layout: post
title:      "ActiveRecord in Sinatra - An Introduction"
date:       2018-11-28 02:45:26 +0000
permalink:  activerecord_in_sinatra_-_an_introduction
---


Object Relational Mapping, or ORM, is a technique that connects rich objects to tables in a relational database. Active Record is a dynamic ORM, and is a standard convention for accessing relational databases with Ruby.

### Why use Active Record? 
ORMs like Active Record help us cut down on repetitious code and implement conventional, organized patterns. Object properties can easily be stored and retrieved from a database without writing SQL and less overall database access code. Active Record has become convention for Ruby developers in need of an ORM - it is extremely robust and heavily used.

At the most basic level, Active Record is used to represent and validate models and data, as well as model associations and inheritance hierarches. Models and data are accessible via robust methods inherited from Active Record, including object-oriented methods to aid with CRUD actions and object association.


### How to use Active Record with Sinatra
Here's a set of key steps I have found useful when starting to build a Sinatra app using Active Record: 
1. Make the Ruby gem available in your app by running 'gem install activerecord' and including it in a Gemfile. To use ActiveRecord with Sinatra you will also need the following gems: 'activerecord', 'sinatra', 'sinatra-activerecord', 'rake', and a database gem like 'sqlite3'.
2. Establish a connection with a database
3. Create models and database tables using Active Record conventions. Ensure rake or Rails database migrations inherit from ActiveRecord::Migration, and Ruby classes inherit from ActiveRecord::Base.
4. Take time to understand the new methods made available to you with Active Record. Robust CRUD actions and object association methods are available by default if you follow conventions.


### Establishing a connection with a database
In an app's environment file, the following code can be added to establish a connection to a database. Here you can see a sqlite3 database used for development. You can imagine creating additional connections to 'db/test.sqlite' for testing or other production databases.

```
ActiveRecord::Base.establish_connection(
	:adapter => "sqlite3",
	:database => "db/development.sqlite"
)
```

"SQLite3" is a common adapter, though others like "MySQL" can be used. The :database should point to the path where the database will be stored - by convention this is in a "db" folder in the root of an app.


### Active Record conventions
When mapping an app to a database with Active Record, we equate classes with database tables and instances of those classes with table rows. By doing this and following a set of conventions, we get an enormous amount of functionality for Ruby objects from Active Record.

###### Naming Conventions
Active Record model and table naming conventions are simple. Model classes have singular names, and use CamelCase for models with multiple words. Database tables are the plural of their respective model class, with underscores separating words. For example, a Hike class has an associated hikes table. A HikeClub class has an associated hike_clubs table. The built-in pluralization is very robust, and handles complex pluralization like class Mouse to table mice (though it's best to verify complex pluralizations work as expected).

###### Schema Conventions
Foreign key columns should be named table_name_id where table_name is the singular name of the table the foreign key refers to. For example, a foreign key for a hike's region would be region_id. A plural foreign key uses underscores like hike_club_id. Active Record will look for these columns when associating objects. See the associations section below for more detail on how to configure model associations using foreign keys and Active Record macros.

Primary keys are created by default as an integer named id. The id column is automatically create when using Active Record Migrations.


### Creating models and tables

To create Active Record models with Sinatra, simply subclass ActiveRecord::Base 

```
class Hike < ActiveRecord::Base
end
```
	
and create a table in the database with a migration that inherits from ActiveRecord::Migration. 

```
class CreateHikes < ActiveRecord::Migration
  def change
    #create table here
  end
end
```


The following rake command can be used to create a new migration in your 'db' directory.
	
```
rake db:create_migration NAME=create_hikes 
```

The name should describe what the migration will do, using the plural table name and underscores between words. By saying "NAME=create_hikes", the migration class is automatically named "CreateHikes".  
You could write a SQL statement to create your desired table. However, ActiveRecord::Migration provides a series of methods that allow you to write more user friendly migration code in lieu of SQL statements.

To create a table, use the create_table method. Use this method to specify a table name (:hikes below) and the columns you want in that table within the method block. Each column should specify its datatype as shown below. Note that you do not need to create a primary key column -- Active Record does this automatically when a table is created.

```
class CreateHikes < ActiveRecord::Migration
  def change
    create_table :hikes do |t|
      t.string :name
      t.string :region
      t.integer :miles
    end
  end
end
```

Here's the equivalent SQL statement for comparison:

	CREATE TABLE hikes (
	   id INTEGER PRIMARY KEY,
	   name TEXT,
	   region TEXT,
	   miles INTEGER
	);


Additional migrations can be written to update and destroy tables with a variety of Active Record methods detailed [here](https://edgeguides.rubyonrails.org/active_record_migrations.html).


Once classes and tables are created and inherit from the appropriate ActiveRecord models, you can interact with classes and properties in an object oriented fashion. For example, creating a new hike and adding properties can be written as:

```
hike = Hike.new
hike.name = "The Enchantments"
hike.region = "Central Cascades"
hike.length = 18
hike.save
```

This will create a new hike instance, add name, region and miles properties, and finally save the hike to the database. Active Record does the heavy lifting to convert table column names into friendly, object-oriented methods you can use with your classes.


### CRUD Methods
Active Record provides a set of handy methods for object CRUD actions (create, read, update, destroy). Below I have outlined some of the more common methods with a sample Hike class.

###### Create Methods

```
# instantiate an instance of a hike
hike = Hike.new       

# save the hike
hike.save

# instantiate and save an instance of a hike simultaneously
hike = Hike.create  
```

	
###### Read
Active Record provides a variety of methods for accessing database data. A few commonly used methods are below using the same hike example, with more detailed [here](https://guides.rubyonrails.org/active_record_querying.html).

```
# return a collection of all hikes
hikes = Hike.all

Equivalent SQL:
SELECT * FROM hikes


# return the hike with primary key (id) 13
hike = Hike.find(13)

Equivalent SQL:
SELECT * FROM hikes WHERE (hikes.id = 13) LIMIT 1


# return the first hike named "The Enchantments"
enchantments = Hike.find_by(name: 'The Enchantments')

Equivalent SQL:
SELECT * FROM hikes WHERE (hikes.name = 'The Enchantments') LIMIT 1


# return all hikes in the Central Cascades
hikes = Hike.where(region: 'Central Cascades')

Equivalent SQL:
SELECT * FROM hikes WHERE (hikes.region = 'Central Cascades')


# return the first hike
hike = Hike.first

Equivalent SQL:
SELECT * FROM hikes ORDER BY hikes.id ASC LIMIT 1


# return the last hike
hike = Hike.last

Equivalent SQL:
SELECT * FROM hikes ORDER BY hikes.id DESC LIMIT 1

```

###### Update
There are two main ways to update object properties:

```
# query for an object instance, use property methods to update a property, and then save the instance.

hike = Hike.find_by(name: 'The Enchantments')
hike.name = 'Enchantments'
hike.save
	
# use Active Record's #update method

hike = Hike.find_by(name: 'The Enchantments')
hike.update(name: 'Enchantments')

```

###### Destroy
One or more object instances can be deleted with Active Record methods:

```
# destroy an object instance
hike = Hike.find_by(name: 'The Enchantments')
hike.destroy

# destroy all object instances
Hike.destroy_all
	
```

### Associations

Active Record associations enable us to associate models and their tables without having to write a ton of new code. To use Active Record associations you must:
1. Create tables with the appropriate associations. For example, if you are creating a relationship between a Hike model and a Region model where a Hike belongs to a region and a Region has many hikes, you must include a region_id in your hikes table.
2. Use Active Record macros in your models.

The most commonly used macros are belongs_to, has_many, and has_many through:.

Let's say you're building a hiking app with a model for hikes *and* a model for regions. A hike belongs to a region, and a region has many hikes. To create this relationship, add a foreign key for region (a region_id column) to the hikes table

```
class CreateHikes < ActiveRecord::Migration
  def change
	  create_table :hikes do |t|
		  t.string :name
			t.integer :region_id
			t.integer :miles
		end
	end
end
```	

and add the appropriate macros to the classes.

```
class Region < ActiveRecord::Base
  has_many :hikes
end

class Hike < ActiveRecord::Base
  belongs_to :region
end
```

Note the pluralization used in the macro arguments above. When using the has_many macro, the argument provided is plural (:hikes). When using the belongs_to macro, the argument provided is singular (:region). This convention simply helps the code read more naturally. The methods made available to you with these macros are listed below.
	

###### belongs_to
The belongs_to macro provides six new methods you can use in your class:

```
association
association=(associate)
build_association(attributes = {})
create_association(attributes = {})
create_association!(attributes = {})
reload_association
```

In all methods, association is replaced with the symbol based in the argument you pass. For example, above we used the belongs_to macro with the symbol :region as the argument. So in this example we will have access to the following methods:

```
# read the hike's region
hike.region

# set the hike's region
hike.region=(region)

# instantiate a new region associated with the hike (but not saved)
hike.build_region(attributes= {})

# instantiate and save a new region associated with the hike
hike.create_region(attributes = {})

# instantiate and save a new region associated with the hike, and raise ActiveRecord::RecordInvalid if the record is not valid
hike.create_region!(attributes = {})

# reload a region
hike.reload_region
```

###### has_many
The has_many macro provides 17 new methods you can use in your class:
```
collection 
collection<<(object, ...) 
collection.delete(object, ...) 
collection.destroy(object, ...)
collection=(objects)
collection_singular_ids
collection_singular_ids=(ids)
collection.clear
collection.empty?
collection.size
collection.find(...)
collection.where(...)
collection.exists?(...)
collection.build(attributes = {}, ...)
collection.create(attributes = {})
collection.create!(attributes = {})
collection.reload
```

In all methods, collection is replaced by the symbol passed as the has_many argument. In our example, collection is replaced with hikes.

```
# see the region's hike collection
region.hikes

# add one or more hikes to the region's collection
region.hikes << (hike, ...)

# remove one or more hikes from the collection by setting their foreign keys to null
region.hikes.delete(hike,...)

# remove one or more hikes by running destroy on specified hike instances
region.hikes.destroy(hike,...)

# makes the collection only contain the supplied hikes, by adding and deleting as appropriate. Changes are automatically persisted to the database.
region.hikes=(objects)

# return an array of the ids of the hikes in the collection
region.hikes_singular_ids

# updates collection to only contain hikes identified by the supplied primary key values
region.hikes_singular_ids=(ids)

# remove all hikes from the collection
region.hikes.clear

# return true if the collection is empty, or false if it is not empty
region.hikes.empty?

# return the number of hikes in the collection
region.hikes.size

# find a hike in the collection by ID (input integer ID)
region.hikes.find(...)

# find one or more hikes in the collection that meet supplied conditions (input hash of conditions)
region.hikes.where(...)

# check whether a hike meeting supplied conditions exists in the collection
region.hikes.exists?(...)

# creates but does not save one or more hikes with specified attributes. Hikes are linked to the region through their foreign key.
region.hikes.build(attributes={}, ...)

# creates and saves one or more new hikes associated with the region. Hikes are linked to the region through their foreign key.
region.hikes.create(attributes = {})

# creates and saves one or more new hikes associated with the region, and raises ActiveRecord::RecordInvalid if the hike is invalid
region.hikes.create!(attributes = {})

# reload hikes array
region.hikes.reload
```
	
	
Details on these methods and more complex associations can be found [here](https://guides.rubyonrails.org/association_basics.html).
	
### Sources
* [Learn.co](https://learn.co/tracks/full-stack-web-development-v6/sinatra/activerecord/activerecord-associations-in-sinatra)
* [Rails Guides](https://guides.rubyonrails.org/)

