---
layout: post
title: "Rebuilding Gray Duck Labs with Octopress"
author: "Charles Worthington"
date: 2012-11-04 00:25
comments: true
categories: [Gray Duck Labs]
---

When I chose to leave my awesome [strategy consulting job](http://www.altvil.com) to pursue a freelancing career in product design and development, creating a brand to represent myself was one of the first projects I worked on. In a way, I was my own first "client."

Gray Duck Labs was the outcome of this effort. I created a portfolio of branded documents, including a letterhead, a slide and wireframing template, business cards, and of course, a web site.

{% img /images/posts/grayducklabs_old_screenshot.png Screenshot of the old Gray Duck Labs site %}

Since this effort coincided with a personal push to refresh my web development skills, I took the opportunity to design and code my own site, including writing my own blogging engine in Ruby on Rails. Initially I was happy with the result (I kept [a copy live on Heroku](http://grayducklabs.herokuapp.com) if you're interested). As time passed, however, I became frustrated with the limitations of the blog engine I had created. For example, my engine lacked:

- Social sharing buttons
- An article index
- Article tagging
- Named page routes
- Comments

None of these features are revolutionary. In fact, these are problems that have been solved [many](http://www.wordpress.org) [times](http://www.blogger.com) [over](http://www.squarespace.com). As I became busy with actual client work, I found less appeal in the idea of writing these features from scratch "for fun."

Enter [Octopress](http://www.octopress.org). Octopress is a Ruby framework that extends [Jekyll](https://github.com/mojombo/jekyll) to allow you to generate a static site that includes many features that would normally require a dynamic web application.

<!-- more -->

I chose to use Octopress because it includes many key features out of the box, including social integration and a responsive layout. Being built with Ruby, it was also a good match for my skill set. 

I wound up customizing the implementation much more than I was expecting going in. This was because Octopress's default theme (and most of the [third party themes](https://github.com/imathis/octopress/wiki/3rd-Party-Octopress-Themes)) are very blog-centered, and apparently not intended to host static marketing content. If you're interested in the gory details of how I created the site you're looking at, you can [check out the source code on GitHub](https://github.com/cew821/grayducklabs).

Overall, I'm happy with the transition. I like the design changes I made to the marketing content, and despite the heavy customization required it was helpful to use Octopress' themes as a starting point for my own responsive design.

But the main benefit to this change is that I can now focus on the content. Writing a new post is as easy as dropping a text file into the `/posts` directory and running `$ rake generate` and `$ git push heroku`.

#### Why this might be a bad idea

In the immediate term, the new site is definitely an improvement, but I do have a few reservations. Mainly, these boil down to two issues: 

- **I need my development machine to make a new post.** Before, I could just log in to my Rails app from any computer to make a change. Now, pushing an update from a location other than my development machine would require a non-trivial configuration process. I also won't be able to update the blog from a mobile device.
- **It will be nearly impossible for non-coders to contribute.** Though Gray Duck Labs is just me for now, it's not inconceivable that at some point there will be other folks I want to contribute to this blog. In a traditional web app (including in my basic Rails implementation), this would be no problem. With Octopress, other contributors would need the ability to make pull requests to my GitHub repo. Fine for developers, pretty much unusable for everyone else.

I suspect if it ever gets to that point I'll wind up migrating to a more traditional database-driven blog engine like Wordpress. But until then, I'm happy to be 'blogging like a developer.'

If you've been using Octopress or Jekyll for your company blog and encountered these issues, or if you'd like to learn more about my experience, [drop me a line](mailto:%63%6f%6e%74%61%63%74@%67%72%61%79%64%75%63%6b%6c%61%62%73.%63%6f%6d).