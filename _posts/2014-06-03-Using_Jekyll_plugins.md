---
layout: post
title: "On using Jekyll, and plugins"
date: 2014-06-03 21:30:22 China Standard Time
description: "Post about how to make plugins work with Jekyll"
category: 
tags: [Jekyll, plugins, tutorial]
comments: true
---

It's only been a few weeks since I first started to get familiarized with *Ruby on Rails*, and almost everything related to it, like *Github!* Where you can store your open source project code. I found out about *Jekyll* after stumbling upon a lot of blogs about Rails. Jekyll is used to build static websites that can run on Github pages. 

Now this, [kbleabres.github.io](https://kbleabres.github.io), is using Jekyll and surprisingly, I am enjoying Jekyll. Surprisingly because, despite the fact that most people think this is less work than other kind of blogs, I personally think this requires a lot more work. Create a new post. Add front matter *(though I handled this already via Rakefile.)* Save. Then `git add .` `git commit -m "New blog post"` `git push -u origin master`  rathen than a simple Click text, type your blog, Post. Yes, I am talking about *Tumblr*.
 
Of course, I wanted my site to look a little better everyday. Or at least every week. But ever since I started using Jekyll, I have been avoiding posts about using plugins because I wanted my website to be as simple as possible. For me: *Plugins* **=** *more features* **=** *more things to maintain and handle.* So basically, adding plugins means adding more work. It's tiring.

And I was right. I didn't know what changed my mind. Maybe because I have seen a lot of blogs that have this feature and I wanted it for my own blog too. It's the "reading time" stuff. Where it will tell you about how much of your time will be spent reading a post. It's quite cool if you ask me.

So yeah, I decided to add a plugin to my Jekyll blog. And it was working perfectly while I was viewing it via `localhost:4000` so I continued typing my blog post and after, I pushed to my repo. When I viewed my site online.. **oh my god the horror**

Then I found out that GitHub renders pages with safe mode enabled. Therefore, my custom plugin was disabled because it was a third party plugin.

I then asked Google how to make these plugins work. I followed a set of instructions that told me to rename my current directory to `something` and create a new directory named `something_something` and copy the contents of `something` to `something_something` and `git rm -r` my `something` directory. After that, it made me do `jekyll build` on `something_something` and then made me copy the contents of `something_something` to `something` and then create a `.nojekyll` file in the `something` directory. Then `git add --all :/` `git commit -m "Built site locally"` (because this set of steps is making you build your site locally) and finally `git push -u origin master` 

After following that set of instructions **oh god the horror x 10000** my website looked a lot more worse than before I did those set of steps. Basically, the layout was stripped out of my blog and the tags weren't working anymore. I was pretty sure I followed the instructions very well. I was sure.

So I just undid my push, and looked for another set of instructions. Now this one doesn't make me do anything other than pasting a long block of code into my `Rakefile` and then running `rake blog:publish`. I hoped that this would work but an error about permission was appearing. I asked Google yet again but there were no answers fit for my problem.

So I searched and searched and I stumbled upon [ixti's solution](http://ixti.net/software/2013/01/28/using-jekyll-plugins-on-github-pages.html) and it worked like a charm omg the best thing that happened to me today

So basically all I have to do was to put this on my `Rakefile` 
	
	require "tmpdir"
	require "bundler/setup"
	require "jekyll"

	GITHUB_REPONAME = "kbleabres/kbleabres.github.io"

	desc "Generate blog files"
	task :generate do
  		Jekyll::Site.new(Jekyll.configuration({
    		"source"      => ".",
    		"destination" => "_site"
  		})).process
	end

	desc "Generate and publish blog to gh-pages"
	task :publish => [:generate] do
  		Dir.mktmpdir do |tmp|
    		cp_r "_site/.", tmp

		    pwd = Dir.pwd
		    Dir.chdir tmp

		    system "git init"
		    system "git add ."
    		message = "Site updated at #{Time.now.utc}"
    		system "git commit -m #{message.inspect}"
    		system "git remote add origin git@github.com:#{GITHUB_REPONAME}.git"
    		system "git push origin master --force"

	    Dir.chdir pwd
  		end
	end

and all I had to do next was to run `rake publish` and when I viewed my site wow it was back to normal, with the feature I wanted!! 

Cooleo
