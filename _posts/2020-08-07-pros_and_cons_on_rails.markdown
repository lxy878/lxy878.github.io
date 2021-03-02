---
layout: post
title:      "Pros and Cons on Rails"
date:       2020-08-07 14:32:55 +0000
permalink: pros_and_cons_on_rails
---

Compared with Sinatra, the rails provides a lot of convenient tools like validations, authentications, etc.  Those tools solve some problems I had while I did Sinatra project.  For Instance, one of the biggest concerns I kept in mind was what if users inputted an empty string.  The solution by Sinatra was repeatedly checking the input string whether empty or not, so the code was difficult to keep DRY.  However, now the solution by Rails can be simpler which is to set the presence validation as true.  Then, I don't need to worry about this problem anymore.  Rails is a very powerful framework.

Rails contains many helpers to developers' lives easy, but sometimes problems are caused by those helpers.  For example, one of the requirements for the rails project is to display validation errors.  The way to do so is simply to use "render" which can store the current object in cache and load the object to an action in a controller.  However, using "render" will not help us achieve the requirement in the case that the next route responds both actions as normal url and nested url ( ex. posts/new && users/:id/posts/new).  Since we don't get control which url is sent by render, the only option we have is "redirect_to" which can relocate the page base on a url (not an action) but without any previous data.  The solution I did in my project was to store the error messages into the "Flash" cookies and then redirect to a specific url.  

Rails is a powerful and convenient framework for web development.  Its tools and helpers make the developing process a lot easier.  However,  it is not perfect all the time.   In some situations, developers need to think out of the box rather than to rely on rails.