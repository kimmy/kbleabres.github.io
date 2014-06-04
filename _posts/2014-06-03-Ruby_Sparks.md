---
layout: photo_post
title: "Ruby Sparks"
date: 2014-06-03 01:10:35 China Standard Time
description: "A blog post about my internship at ÆLOGICA"
category: 
tags: [internship, AELOGICA]
comments: true
---

{% include image.html url="/assets/RubySparksII.png" description="<em>Ruby Sparks poster, inspired by the Ruby Sparks movie poster</em>" %}

The end of my summer vacation is nearing, and the end of our summer internship is coming twice as fast. We're already on our 7th and last week here at **ÆLOGICA** and I'm sure we have learned so much in just one and a half months. 

During our internship, we are to experience developing web applications using Ruby on Rails. *Ruby on Rails*, or simply Rails, is a web development framework running on the Ruby programming language, something I have never used before. We are also expected to contribute features to open source projects like Gitlab and Discourse. Almost everything was new to me. 

I started learning by reading and following tutorials available online. Most of the links were given to us by the developers here at **AELOGICA**. They lend a hand whenever you needed help (and whenever they weren't busy with their own projects.) I was able to make a blog by following Michael Hartl’s tutorial. I also followed a Rails Girls tutorial and was able to make an app called *railsgirls*–that’s what they called their app in the tutorial. I also experienced deploying these apps via **Heroku**! Another first for me! 

The difference between the school and the work place is that no one is actually telling you how to do things. You have to discover things on your own. You do it the way you feel comfortable (but also keeping in mind the requirements given by the client, or conforming to the format of the codebase you're contributing to.) Unlike in school, where the professor introduces the stuff to you and then they give you examples to practice on. At work, you start with the "practice" and that's how you get introduced to the stuff you have to learn. 

There were a lot of tutorials and articles online that helped me with learning Rails. Other articles said that I didn't have to learn Ruby (the programming language Rails is running on) to learn Rails. Others said I should learn Ruby first. I did the latter. Learning Ruby and Rails was a bit hard, but also kind of easy. It was hard because like I said, I have never used them before so I had to figure out how to do things as I progress. But it is easy because basically, most programming languages have similar syntax. I liked Java a lot before, but typing `puts 'Hi'` rather than `System.out.println("Hi");` just to say "Hi" and typing `5.times { puts "Hi" }` instead of `for(i=0; i<5; i++) { System.out.println("Hi"); }` just to say "Hi" 5 times is a lot more efficient. And that's what Ruby offers: simplicity and productivity in object-oriented programming.

Learning how to use Rails means understanding the *MVC pattern or the Model-View-Controller pattern.* Working on the open source project helped me understand this. But I know I need more practice to master this. 

I worked on **AELOGICA**’s Brand Identity Style Guide together with Ricky before working on an open source contribution. I was already familiar with Photoshop before working on the project but I still learned a lot of new stuff while working on it. I figured that there were steps I used to do before that can be done in an easier manner.

Design. It is fundamental in any thing you see in this world, not just in applications. Being creative is also a skill any developer should have. Working on the project helped me become more creative. I think it also helped me code more beautifully than before. 

After about a week working on the project, our supervisor, Nestor, provided us a list of open source projects we can contribute to. 

>| Everyday gems | | Rails apps |
>|---  |---  |
>| + [Rails](https://github.com/rails/rails)    | | + [Gitlab](https://github.com/gitlabhq/gitlabhq)    |
>| + [LightService](https://github.com/adomokos/light-service)    | | + [Discourse](https://github.com/discourse/discourse)    |
>| + [Bundler](https://github.com/bundler/bundler)    |     |
>| + [Rubygems](https://github.com/rubygems/rubygems)    |     |

Karlo, who's a developer here, contributed to _Gitlab _when he was AELOGICA's only intern--which was last year. He implemented a version of Github's contributors graph for Gitlab. His contribution was so good it got merged and he was named as the MVP by the Gitlab team. I felt pressured because I didn't know if I could implement a feature as good as his contribution.

I planned to contribute to _Discourse_ instead because I thought if I contributed to the Gitlab app, my contribution would be compared to Karlo's contribution. But all the other interns decided to contribute to Gitlab, and they already had ideas as to what they wanted to implement: **Language Statistics **and** Commits Calendar**--both based on existing Github features. We decided we were going to do pair programming, since there were 4 of us here. 

So for our open source contribution, Denise, and I decided to work on the Commits Calendar feature for Gitlab. We followed the instructions on how to contribute to Gitlab and we had to setup `vagrant` to be able to work on a contribution. While setting up, I tried to look at how Github is implementing their own contributions calendar. I found **Cal-Heatmap**-–a javascript module using d3.js library that is used to create a calendar heatmap.

We successfully (at least we thought) set up our dev environment. We used Cal-Heatmap for the actual interface. e looked at how the contributors graph is getting all the data that it needs. We found out that we can retrieve the logs that contains the commit author’s email, name, and the number of commits, additions and deletions of the author used by the Contributors Graph feature from the repositories. We pried (`binding.pry`) into the codes to know where the data is coming from. 

Analysing the situation, we figured a user can only have commits in a repository if he is a member of the project. We first accessed the projects a user is a member of, and then accessed each of the project’s repositories to retrieve the logs. _User &gt; Projects &gt; Repositories &gt; Graph logs._ We can retrieve all the logs in each project repository, and only actually get the number of commits of the user--thanks Ruby's `Enumerable` methods!

We had to inject the data we retrieved from the repositories into the calendar interface. The data needed to be in a certain format. We looked at the documentation and studied the format of the data we need. Also, we checked on how Github is injecting their data to their calendar. And we needed to format our own data like that. Basically, we need a hash containing a timestamp of the commits, converted into UNIX format as the `key`, and the number of commits on that date as the `value`. Denise also found a gem that contains the cal-heatmap files. In the end, we have an `array` of `hashes` because there are many commits made by a contributor in a single project on different dates.

The use of a gem is more efficient and convenient compared to the vendor approach (putting the `cal-heatmap.js` and `cal-heatmap.css` files to the vendor folder of the application's root directory) because we can take advantage of the updates easily. We also customised the look of the calendar so it would look as close as possible to the Github calendar. With the help of other developers here at AELOGICA, we were able to refactor the code and make it look more appealing.

We then opened our first pull request. We were kind of nervous because the other pair's pull request was, sadly, rejected. And their implementation was really good! Fortunately, Randx, the lead developer of Gitlab commented on our pull request saying he likes <q>idea of feature</q> but we have to clean up our code. The other team started on a new project--drag and drop upload of images feature for every markdown area in Gitlab. Their contribution got merged right away--and the Gitlab team thinks they would be the MVP this time.

We refactored our code, and moved the logic of the code to a helper. We also created a spec for the controller.

We were experiencing errors, mostly database-related, while running the test. The errors were really troublesome, and we ended up reinstalling our dev environment--which was vagrant. It took a little while longer than our first installation, but we weren't experiencing the errors anymore. It worked fine afterwards. 

After everything, we updated our pull request and asked the Gitlab team for feedback. Randx asked us to refactor more and make the code mergeable and to provide a full page screenshot of our contribution.

While refactoring, Denise thought of adding more features to our calendar. We worked on adding `Ajax` or _Asynchronous Javascript and XML_  to our calendar: when a day in the calendar is clicked, it will show the user’s commit activity in each of the repositories the user has contributed to in that day. We had to get the logs again and group the commits per project, and then per date. So now, we also have a hash whose value is also a hash in addition to the array of hashes we already have.

We refactored our code again, and again, and again. Refactoring helps us break down the code without breaking the code. We refactor code little by little to reduce the risk of making errors and breaking the code. Our initial code looked very different from what it is now. We also moved the code from the helper to the lib folder. Our code looked very different from what it initially looked like, but it is definitely cleaner now.

We're still waiting for their decision whether they'll merge our pull request or not, but we do hope that they do.

What’s challenging about this project is that everything was, like I said, new to me. So everything new I know by now, I learned from working on this project because of this internship. Working on this project didn't just help us learn Rails, it also helped us learn Git, Unix commands, and the Ruby programming language. It enhanced our front-end skills by contributing to an open source Rails application. I experienced pair programming here at AELOGICA, and I liked it. Before, I wasn’t much of a fan of pair programming, because I feel shy whenever someone’s watching me type the code I have in mind. They might make fun of me inside their heads. But I figured that pair programming makes two people work together on something and it helps both people to improve their skills. 

The developers here at AELOGICA share their knowledge to us willingly and we really learn a lot from them and at the same time, it helps us make sure that our code is not just some crappy stuff and that the method names helps us understand what exactly your code does. It makes your code quality production level. 

The people at **AELOGICA** are awesome. They’re really helpful and they assist us in what we’re doing. Sometimes, they’re the ones who come to us to ask us if we needed any help. 

Everyday, I learn new stuff here at **AELOGICA** and I’m just sad that I won’t be coming back (for now, I hope) after this week. There’s still too much to learn!

I would definitely recommend **UST-ICS** students to be an intern here because there's just too much learning experience you wouldn't want to not experience.

Thank you for having me, and for giving me a wonderful experience **AELOGICA**!
