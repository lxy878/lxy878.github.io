---
layout: post
title:      "Rails with Javascript Project"
date:       2020-10-07 14:32:55 +0000
description: 'To compare the rails project, the js/rails project structures a half of the rails framework as backend and another half of js and HTML as frontend.  In other words, I could not use some conveniences.....'
permalink: react_rails_projectrails_with_javascript_project
---

To compare the rails project, the js/rails project structures a half of the rails framework as backend and another half of js and HTML as frontend.  In other words, I could not use some conveniences that rails prevides during the development of precessing.  My Object is a simple website that allows users to post a recipe and to view all recipes.  At early development, I designed a litter bit complex relationship in the database.
```
Meal: has_many recipes
Recipe: has_many ingredients
Ingredients: has_many units, belongs_to recipe
Unit: belongs_to ingredient
```
This relationship has a one-to-many and a many-to-many.  If this kind of relationship is developed on a pure rails project, CRUD can be simple to handle because rails provides the functionalities that build interactions among models, views, and controllers. In the same situation, however, the CRUD in the js/rails object requires developers to do extra work between views and controllers.
One problem I had was that posting data by a form didn't work well with nested form, which models use 'accepts_nested_attributes_for'.  As we know, the views in the rails framework can directly talk to models without going through controllers.  The nested form can be handled and wrapped a hash in params, so creating a new recipe only passes params that includes meal and ingredients.  On the other hand,  in the js/rails object, since the frontend only talks the backend through js code, the data has to wrap a package such as JSON to send to the server.  I could not use the same params with a nested form to create a new recipe because the controller would not pre-create meal object and ingredient object.  Thus, I separated the inputs as recipe, meal, and ingredients.  Then, the controller created each object for meal, recipe, and ingredients.  The solution makes the rails less effective.  Form this problem, I had seen one of the downsides of mixed two different languages to develop a project.