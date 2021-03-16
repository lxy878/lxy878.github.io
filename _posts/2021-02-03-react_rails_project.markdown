---
layout: post
title:      "React/Rails Project"
date:       2021-02-03 14:32:55 +0000
description: 'Throughout the whole process of development with react, I would say that using Reactjs is more convenient than directly using Javascript but less convenient than using Ruby on Rails.....'
permalink:  react_rails_project
---


Throughout the whole process of development with react, I would say that using Reactjs is more convenient than directly using Javascript but less convenient than using Ruby on Rails.  The project is required the simular structure like the previous one that separates the application to a client-site and a server-site.   The server-site is still using rails api and the server-site is React instant of Javascript.  So, I choose to create a online-store app. The main features of the app are user authorizations, selling products, and purchasing products.

The most challenging part during the development of my project was implementing multiple libraries.  These libraries caused some unexpecting errors when I was gluing them into a place.  For example, Once I tried to create a navbar that can switch its contents between user view and guest view.  The common idea is that the most top-level component(App.js) must check if the user id exists in the local storage.  If the user id exists, the navbar shows user features.  Otherwise, it shows user login or register.  

Base on the idea, I created a function that checked the local storage and then put it into render().  Since React framework is SPA (single-page application), I used react-router-dom to create multiple pages.  Also,  I used Redux to trace user login status.  So if the state of Redux is changed, the top-level component will reload the navbar.  However,  the app didn't run what I excepted.  After I entered the user account, the navbar stayed the same, even the user id saved in the local storage.  After running several tests, I found out that when a route was accessed, the data in Redux or state was removed.  So,  I added a boolean value into the state in the App.js and wrapped it into a setter function. Then, the setter was passed as props to child components.  When one of the child components modified the boolean value, the App.js re-checked the local storage and reloaded the application.

Applying multiple libraries or gems can make the development process quicker and simpler.   However, if we overuse them, our projects would become extra complicated and buggy.
