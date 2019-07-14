---
published: false
layout: post
title: A new beginning
featured-img: book-731199_1920
summary: "Do what you can, with what you have, where you are."
categories: [english]
---

> "Do what you can, with what you have, where you are."
>
> &mdash; <cite>Theodore Roosevelt</cite>


We live in complicated days...

I expected it was simpler to start this new blog using [Jekyll](http://jekyllrb.com). Mainly because I was following along with an [excellent tutorial](https://jtemporal.com/do-tema-ao-ar/) made by Jessica Temporal (in pt_BR).

I decided not to pollute my machine with this new stuff. Wise decision I made, creating a virtual machine with Virtualbox and Vagrant. Read on.

After so many years using Python I tamed its environment. Ruby is still a beast to me and I couldn't install Rubygems without privileged access. For some reason the process was complaining I didn't have `make` installed. After some googling and trying to keep strict to Jessica's tutorial, I decided to go to the canonical reference: [Jekyll Installation](https://jekyllrb.com/docs/installation/). Tutorials are good and they don't cancel the official documentation.

This solved the `make` and Rubygems problems. Finally I could install Jekyll and bundler.

But, when I tried to `bundle install`, a new problem raised. This time a message said I didn't have `bundler ~> 1.2`. My bundler is newer, 2.0.2, but it didn't work. After some time googling again, I discovered how to solve it:

```
$ gem install bundler -v '~>1.12'
$ bundle _1.12_ install
```

I admit I still don't understand the last command, but my goal was to write a first post on the new blog. So, that wasn't a priority.

Now I could run `bundle install` and finally I saw my wannabe blog running in a virtual machine. In hindsight I can't tell you if I was happy or angry. But I was upset, for sure.

All I wanted was to start writing my texts in my own platform and all I was getting was error messages. Medium was not an option because I wanted my texts with me.

Now that I was seeing my blog running, I decided to choose another theme. I liked [Sleek](https://github.com/janczizikow/sleek) for it is "a modern Jekyll theme focused on speed performance & SEO best practices".

What I didn't imagine was that "modern" means "bloated" in these complicated days. It requires nodejs to run! Likewise Ruby, I had never touched nodejs before. It was my debutting. I installed node 10, the latest LTS version by the time of writing this post. I always go with LTS versions.

After that, when `npm install`'ing I got a message telling me Python was needed! So, let's install Python. And it wasn't Python 3. It has to be Python 2.

To my surprise Gulp (new to me, too) didn't support this nodejs version. I have to uninstall node 10 (an LTS version) and downgrade to node 9.

Due to reasons I don't reason about, some npm packages didn't install as expected and I was unable to run `gulp img` to generate different sizes for my images. So, I removed the directory `node_modules/` and `npm install`'ed all over again.

This time everything was fine and I could optimize images.

Now, you are reading this article because publishing in Github Pages is very simple. Ironically, it isn't an open source platform.

I'm an enthusiast of open souce software, but usability counts a lot and sometimes all we want is "things that simply works".

See this scenario: I wanted only a static site generator to publish my posts. I had to install Ruby, Nodejs and Python. Three different languages! Just to abstract the work to generate static pages from my markdown texts.

How confusing things can be.

I know by experience every beginning has its own traps. This couldn't be different.

The secret is keep moving on.
