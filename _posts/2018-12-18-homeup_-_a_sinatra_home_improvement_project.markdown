---
layout: post
title:      "homeUp: A Sinatra Home Improvement Project"
date:       2018-12-18 22:42:18 -0500
permalink:  homeup_-_a_sinatra_home_improvement_project
---


[homeUp](https://github.com/efkarst/homeup) is my first full stack web development project. homeUp allows you to plan, track and budget for home improvement projects. Users can create projects for each room in their home and track their notes, material, time and budget for each project and room. 

### How it was built
homeUp is a MVC Sinatra application that uses Active Record for relational database management. It is built with three models: User, Room and Project. Each user can have many projects through many rooms. Each room has one user and many projects. Each project belongs to one room and one user through its room. These relationship and the characteristics of each class are described in the following structural UML diagram: 

![Model](https://unne2a.bn.files.1drv.com/y4mu7J5PZ-1atKll871ER7qV2sv1aofoylxIv8GHMOj2JNJCzgp5eZdxR-pHvDGPDSQvFsV31xQhShWldTU_rXrXWgg654gNdI8G8OUcTtPgkLeKHbx4hVeeAJFEdlbC8jqnveJ8bDjaRT4a-KLY4lGcz-_vXu8wszVVJZzuVgNwZaVynxoqKCcK86y19aKlbmfhznXGBQcWtX2fa6UL18T8Q?width=660&height=285&cropmode=none)

Object data is validated using Active Record Validations before any new object data is saved to the database. For example, when a new project is created Active Record Validations are used to ensure that the user has provided a name (scoped to that user's project collection) and room for the project. I also verify that values for cost and duration are numeric. Additional validations are in place for the User and Room classes.

homeUp also provides an experience for users to sign up for an account, log in and log out. Users are able to provide a name, username (validated for uniqueness with Active Record Validations), and a password (encrypted using the Ruby gem bcrypt and Active Record's *has_secure_password* macro). Once logged in, the user's unique ID is added to the session hash so they remain logged in for the duration of their session. During a session, a user can create, read, edit and destroy their projects. In addition, they can read projects created by others but are unable to edit or delete them. 

### User Experience

homeUp allows a user to perform basic CRUD actions on their projects (create, read, update and destroy). 

Once logged in, the user is routed to their home page where they can see an overview of their projects organized by rooms: 

![Home Page](https://hrythq.bn.files.1drv.com/y4m-dzxXzKe6p6JebL8UoZCg3A-eRj8RyOSP0wObhf8jlsMoIkkthDApzr0aVv91O4NVvBvdOPlpLz5bDOitHU51M6o3xD2Ey_OSr30mmVF5_UpZ6AC0o6Lj6tVf3z2tKC6oDK-MA9N1rFDLcnVYYs_yfx1KLws1Lz3dGfP-JiWeXVHtB7lKX2dI-js5CwqwpEctTl35uarcaxv6WLXn0aXZw?width=660&height=488&cropmode=none)


The user can click the "new project" button at the top of the screen to create a new project:

![New](https://wpwoka.bn.files.1drv.com/y4mAmD5jULXErb3Hr9HzPAjyZkZ-YvZ5Feo0aXF1aHAqk7OSHH3N90R5WWUOQWjt88I3qFNTHMmoPQboWBm_Q3yX1f-HPqg5vpGzJYnFKOUi77xjW_qSSBaSHAROaNy24AvZJakbOdgqAZESbiCCTjx6d_fIR_4ViOUCsQ0mKYOjbfKFM4zU_Ols9zjponriHZIM1Fq-agR8C2lC3Ynq8kb4Q?width=660&height=481&cropmode=none)


And finally, they can click on a project to see its details. From the detail page a user can click the icons by the project title to edit or delete it from their collection:

![detail](https://60fseg.bn.files.1drv.com/y4mzdacVcP5RGGGABgCKjXRQMytiYB_uCsjlvgIaeMmkw7mS9QGK_agfXbz6I-Ld2dPSSLuQ6ibAmIxv3QTZQFnjShsIL2i2fz0OwDTQShm180BbaLMz8qGWHgHIvXYeY_0PFTqXDt0IYwuq_jASCrPgc1leQe1niv-DT15qV_FilydA2YP9wSv753Yn1sLALvW8AJJj94u9oyG0hubxwC4pA?width=660&height=482&cropmode=none)


### Learnings

This was my first experience building a full stack MVC app, so naturally I learned a ton about how to use Sinatra, Active Record, and ERB views to build the app. As a student developer, I've grown comfortable Googling how to find Ruby logic to solve a specific problem or read the Active Record documentation to learn about methods that are available to me with this framework. 

While it was great to continue becoming more comfortable exploring and playing with code, this project really taught me the importance of two things:

1. **If your code feels wrong it probably is**: When first writing controller actions to create and edit projects, I wound up with 30+ lines of code for data validation. This immediately felt wrong. I figured it might be OK to have 1-2 lines of logic in a controller action, but not 30. This clearly wasn't embodying good separation of concerns. This forced me to consider if there were other places to put data validation. After asking instructors and Googling I was introduced to Active Record Validations and discovered ways I could write data validation classes or include data validation methods in existing classes. It took a bit of refactoring to get parity with my original code, but overall my code is much easier to read and understand. I have clear methods and classes for validation, and am confident that I have good separation of concerns between my Controllers and Models.

2. **Writing DRY code makes everything better**: On several occasions I found myself writing duplicative code when building homeUp. The first time this happened I was building a panel that displayed the total cost and time to complete all projects in a room (if the user was looking at a room detail page), or their entire house (if they were looking at their home page). Originally I built logic into both the Room and User classes to show these two variations of the same content. At one point I needed to refactor the logic, and was instantly annoyed that I had to do it twice. I immediately took the time to break the logic out into a Module that both the User and Room classes could inherit from, and I felt happier and less overwhelmed by my code. It's amazing how complex a simple web app can become. Before beginning this project I conceptually understood that writing DRY code would make my code simpler, easier to read and update, and easier to debug. But only after I built homeUP did I understand the extent to which duplicative or non-DRY code can occur (even in a simple app) and how significantly it impacts your ability to efficiently refactor or debug. It's always worth the time to refactor and simplify code (often multiple times over). 
