---
layout: post
title:      "Bookshelf - A Ruby on Rails App"
date:       2019-01-24 12:34:13 -0500
permalink:  bookshelf_-_a_ruby_on_rails_app
---


Bookshelf is a Rails app I am building for finding, tracking and reviewing books.

Curious about my reading habits, I've logged every book I've read in a spreadsheet since I was 16. I love having data about what genres I've trended towards in various phases of my life (no surprise - my sci-fi reading went way up when I met my now husband, a sci-fi lover), and of course I get a thrill watching my total page count tick up year over year (now over 55k!). My grandma initially inspired this tracking, something she's been doing for over 50 years. She has read an impressive number of books... I have a long way to go to catch up with her!

While I admit my love of reading and book tracking are a selfish motivation for building Bookshelf, my primary motivation is to spread a love of reading an learning. In an era where we are increasingly immersed in a digital world, I believe it is all the more important for people to make time to read, learn, reflect and sometimes simply enjoy a good story that somebody dedicated their life to writing. 

Beyond finding and tracking books, the value of an app like Bookshelf will be its ability to recommend books a user will love or will help them best learn something new. Using data from great sources like Google, Amazon and GoodReads, apps like Bookshelf can build intelligent recommendation systems to help people find what they love and avoid what they don't. 


### The Technology

Bookshelf is still in its infancy, and I look forward to building on and refactoring this project as I learn JavaScript in the coming months. Here's an overview of the technology used for Bookshelf thus far:
* Developed an object-oriented, MVC Rails app using Active Record for relational database management and validations.
* Call Google Books API to provide information on books related based on keyword searches from user.
* Utilized omniauth-google-oauth2 gem to enable user authentication via Google and bcrypt for standard authentication.
* Implemented HTML interface with CSS for styling.
* Used Capybara to implement end-to-end feature tests.



Bookshelf's current models and their associations are outlined here:

![data_model](https://udm0ra.bn.files.1drv.com/y4mnSV1-PhEaFwNyqrV4b5oyca3eOCscj3iFGKioERAgb7_d5SjjFW5COZ_dL-59phK2rbnb_RvbMMjZjsuMD2QItYzyZH6SGvMNn6wcDl7QEnTdl73LPXfIdEYmm0DW1G2aLygt-kN3x9JRRGKFnLptIlPN3vOSusYhzs2QAmrlsS_inFJN08IaEC8HijOpBzD6ZV7meyLQpgOi4oJun7X6w?width=1019&height=632&cropmode=none)


### The Experience
There is much more to Bookshelf than what you'll see below, but here are a few highlights of the app:

###### User Home Page
The user lands on their home page upon sign-in. Here they can see their books, organized by shelves if they have created any. 

![](https://ghu9xa.bn.files.1drv.com/y4mMyrUqeF2W16I3pEn5W5iBfVr_FqpIZ00DpD5UQ4diT9i-cZ6Fc1xNE7l7bRAQqWqx6RPbjQ05KKuBJlFRwXhVnzvCnSUsjPzSmWbm4OD9qxnwZO_5YlrvIVpJQ4Qm0zzcQ8vpMMtw9K1gXebs5HvHoODATnE7mKa9qKoBW_rCiVjWHjne2X_6ZkHVKgy-pl817stdObXuE6HgarEOIa7Hw?width=1024&height=544&cropmode=none)

###### Search Results for "Lonesome Dove"
At any time a user can search for a book to add to their collection using the searchbox at the top of the screen. Here a user searches for "Lonesome Dove" and sees the top 10 results from Google Books. 

![](https://gqbkqw.bn.files.1drv.com/y4moaVySHjhUGhXCsWu5Dqc1xafamiVQ5NqkhrOLVQRcnAtt8KhGAqK87jt6MqZuFFwel81tWCp3QV9gP7US-g-jFEbskFbQFjcNV-svBXDdq0VYrHYsiL_OLTb9FOqURZy9lj6zRIGnq2uUAEYOdvmmNPTQJRA-cRnulACkPCaXGxXV5oLpfUok-Y6iIuU3SirZX_MpHrhniYJ59iJN0Jvug?width=1024&height=542&cropmode=none)

###### Book Details for "Lonesome Dove"

When a user clicks on a book they can see more details, including the genre, pages, rating and a description.
![](https://5qywug.bn.files.1drv.com/y4mHTojR3Pl_zl4aa7LIM-RMMEIiIIXl1xevgE_UJWCj4e0qxsRz079NQr_RhwOxBY09tAfXNVPo8QiEl6bCDwKRDZ2s1rR1KNd24gSEWcfD01kVs76-UOaqUfNFwLhc8adU9BbsqurNxyTYtJn0SIBScEknVrioWCrrZTA2WUirREPEjq7gidyVzhNNjeV7hBIfhUFrPmgYxnQL55N--O_Lg?width=1024&height=438&cropmode=none)


If they scroll down further on the book description page, they can see their activity around this book. A user can mark the book as read, enter the date they finished it, add it to bookshelves, or write a review. They can also see other user's reviews of the book.


![](https://7e1b5w.bn.files.1drv.com/y4mEBzAVJ4XiUsgNcnnBvdN2rkrzRPTCp5QpBE448YRKf7BNaDQUUQH4fiRBHkUHDRb8VqPaWcFkt0wbXTSXZg6sfQcvVrZWwWCHdS7x1AZ1SuymEDCyPzZdfcD2DF_2Q-V37uy9OjTxenj_0OWDgvtsj0As3dpsAQMM_gPvpF2oypMSoyc6Hj-lXD9o-Jbk-wsPiTd2-oK3rKC8y2xn_ce4w?width=429&height=660&cropmode=none)


### What I've Learned 

Bookshelf is my first Ruby on Rails app, and though I have just scratched the surface of what Rails can do I have learned a ton. I won't dive into everything I have learned about using Rails because everything about Rails is new to me. Every concept and line of code is a new learning, and Google continues to be my best friend for understandings new concepts.

If I were to start this project over, I would do four things differently:

1. Start smaller than you think you should. Even small Rails projects and features can be complex. It's important to have big goals, but equally important to take the time to scope and stage your work. It's easy to get overwhelmed in the big picture. I've found that keeping a running to-do list of smaller tasks (which are ordered as I add them to the list) helps me keep my project moving without feeling too overwhelming.

2. Try to be RESTful, but don't force it if it doesn't work. Using RESTful routing incorrectly is probably worse than not being RESTful at all. When implementing search functionality with the Google Books API, I initially thought I could just repurpose the RESTful #new and #create actions to instantiate and save new books. After implementing it this way I realized that it felt wrong - not only was I breaking the design patterns I had learned, but my "default" #new and #create actions were not extensible for creating books through any path but a Google search. I decided to refactor into search-specific paths (that could be extended to search with other APIs, like Amazon or GoodReads), and leave out the default #new and #create actions until I wanted to use them more intentionally. This means my book model doesn't have fully RESTful routes, but the routes I have are much more descriptive of what's actually happening.

3. Clean up and comment as you go. It's amazing how quickly I was able to forget how a method I wrote worked, or why I wrote it. After having to remind myself what my own code did on several occasions in the middle of my project, I took the time to go back through and comment all of my code. This made it much easier to add new functionality moving forward because I could instantly understand why and how things were happening. Even after building this fairly small project on my own, I can empathize with the importance of documenting code at a company of any size. 

4. Keep your code DRY as you go. It is 100% worth the effort to build the appropriate helper methods and partials as they come up in the coding process. Sometimes I would skip refactoring my code fully into partials or helpers because I wasn't sure what other code might depend on the same partial or helper moving forward. This actually made it harder for me because when I finally did decide to create a helper or partial, I had to re-track down every piece of code that would need it. It would have been easier to just refactor one helper or partial that I had created from the start.


### Next Steps
My next focus will be refactoring my interface with JavaScript, which I will be learning from scratch in the next couple of months. From there I would love to integrate more API data (e.g. from Amazon or GoodReads), and begin playing with intelligent recommendation systems.

