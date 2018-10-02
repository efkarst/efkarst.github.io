---
layout: post
title:      "Washington Hikes: An Intro to OO Ruby"
date:       2018-10-01 23:44:40 -0400
permalink:  washington_hikes_-_an_intro_to_oo_ruby
---


For my debut coding project, [Washington Hikes](https://github.com/efkarst/washington-hikes-cli), I combined two of my favorite things: hiking and coding. I live near the mountains of Washington and find myself browsing for new hikes nearly every weekend, so figured I'd build a tool to make my life easier. Washington Hikes is a CLI app that lets users browse the most popular hikes across Washington or in a specific region. It provides the hike length and elevation gain at a glance, and lets users dive into a detailed description of any hike they choose. I can proudly say that I successfully found a new hike last weekend with Washington Hikes, and it was *awesome* (the hike...and the fact that I found it with my own app).

The hardest part of this project for me was not succumbing to feature creep. As a hiking enthusiast I wanted to see *all* of the details, and be able to filter hikes by length, gain, features and rating among other things. Yes, I could have built a lot more functionality into my app. However, my goal in building this app was to get a solid grounding in the fundamentals of Object Oriented Ruby, not manipulate and filter scraped web data for days on end. Having accomplished my goal, I will leave Washington Hikes to be continued another day. 

When I started Washington Hikes, I felt that I had a pretty good understanding of the nuts and bolts of object oriented programming (e.g. variables and methods, instance vs. class scope, self, etc.). I was less comfortable with the process of designing good object oriented systems from scratch. To help my future self any any other beginners who may read this, here's four easy steps to design basic object oriented programs:

#### 1. Identify Objects
The first thing I did when starting Washington Hikes was identify the objects I needed to build. An object should either help you model something tangible (like a hike) or accomplish a specific job (like scraping web data). I learned an easy trick from my instructor to determine which objects you need for a project. Simply write down what you want the user to do in English and extract the nouns and verbs - these are your objects. Nouns are things you will model, and verbs are specific jobs you will do. Following that advice I wrote: "I want the user to be able to **find** **hikes** in a specific **region** of Washington from **scraped data**." Using my instructors model, I needed four classes: CLI (to find hikes for the user), Hike (to model hikes), Region (to model regions), and Scraper (to scrape web data).


#### 2. Identify Object Responsibilities and Relationships
I'm a kinetic thinker, so I decided to sketch out a rough domain model for Washington Hikes to ground myself in what I needed to build before I started to code. I mapped out the basic relationships between classes, and the responsibilities and properties of each. This was super helpful because as I wrote code it was easy refer back to this model when determining where a piece of logic shoud live. I iterated on my model through the course of my project as I learned more, and ultimately here's where I landed: 

![](https://ydy0ga.bn.files.1drv.com/y4m5zyJpFWwadIadK_wTa0RU5hx8Eum1yn20zUXXCmfI9SpNK2ZwAtGoKS55L6xCG0drkCLL-7KQVsb5dZ8JKZtdTSNxKDWBoLma2IdkZYSKLq01ZZKeCOMWgXm-ExFZUxklXNUxOQG4z2h9cSpb4X6m2t_bKl9T1AJ7uHZjXTyVFWs1MH-Jdgg4c2LJSb9rbt9Al4biMeSd1ZeWzKOxfy2dw?width=921&height=412&cropmode=none)


#### 3. Design the Interface Between Objects (with Hard Coded Methods and Data)
A good place to start here is to stub out the user interaction model. I started Washington Hikes by creating the CLI methods I needed to provide options and interpret input from users. This includes:
* A #welcome method to greet the user and determine what they want to do first
* A #choose_region method to promt the user to choose a region
* A #list_regions method to list regions the user can choose from
* A #choose_hike method to prompt the user to choose a hike
* A #list_hikes method to list hikes the user can choose from
* A #list_hike_details method to show the user details on a hike they choose
* A #what_next? method to understand what the user wants to do after they choose a hike

At first I simply hard coded data and inputs for these methods in my CLI class. This enabled me to create the entire user interface and verify what data I thought I needed before doing the hard work to create Hike and Region objects and scrape web data.


#### 4. Replace Hard Coded Methods and Data with Real Methods and Data
I started this step by creating Hike and Region objects. At first I simply moved hard coded data that should belong to a hike or region into methods in those classes, and ensured my CLI continued to work as expected. I then created my Scraper class to scrape hike and region data from the web, and slowly plumbed real data through Washington Hikes method by method. This is the longest and hardest step, but I find lighting up every incremental piece of data to be very satisfying.  Once this is done, all I had to do was a bit of code cleanup and [publish my first gem](https://rubygems.org/gems/washington_hikes)!



