---
layout: default
title: Guide
permalink: /part_1/
---

# Part 1 - Creating an HTML page

**Bit by bit you will get there.**

### 1. Start with an introduction on web technologies given by coach.

### 2. Inspect an HTML file.
* together with your coach, have a look at: [introductions.html](https://railsgirls-be.github.io/railsgirls_guide/materials/introductions.html) [(source)](https://github.com/railsgirls-be/railsgirls_guide/blob/gh-pages/materials/introductions.html)
* discuss about general structure, different tags and their purpose

### 3. Write your own HTML file, using the one above as guide

First of all, do not copy-paste the content. Write each line yourself as it will greatly help you in the long run.

But before creating our file, let us create a folder for this project, in order to keep track of all the files we create. Open your console and type the following commands:

{% highlight bash %}
$ mkdir railsgirls        # mkdir is equivalent to "make directory railsgirls"
$ cd railsgirls           # we 'enter' the new directory using command 'cd'
$ touch introductions.html # command 'touch' creates a new file called as indicated
{% endhighlight %}
Open your newly created file `introductions.html` in your Sublime editor. Start editing the file with information about yourself.

What is important to add in your HTML file, which will be for further use, are the following elements:

* your name in h1 tag
* 2 lists (your hobbies and your languages for example)
* add a picture of yourself
* write today's date somewhere on the page

Be sure to put the following tags into your page:

{% highlight html %}
<h1></h1>
<h2></h2>
<ul><li></li></ul>
<img>
<a>
<p>
{% endhighlight %}

Here are some more tags for extra inspiration, but you don't have to use them:

{% highlight html %}
<strong></strong>
<em></em>
<ol><li></li></ol>
<pre></pre>
{% endhighlight %}

Now that we have created our own page, let us see how the browser sees it. Go to your `railsgirls` folder, where your `introductions.html` file is, right click on it and click tab `Open with` and then select your preferred browser.

It's cool to see your page up there, but at the same time, it seems rather dull and not really colourful. Let's do something about this!

### 4. Style your own page

* use simple (inline) css to color some text
* add spacing to some elements
* play with font's size

Ask the coach to show you how!

Be sure to try out the following css tags: ```color, font-size, font-family, background, border```
For the more adventurous check out ```border-radius, text-decoration, text-align```

### 5. Inspect a stylished version of the introduction

* discuss with your coach the following
<a href="http://railsgirls-be.github.io/railsgirls_guide/materials/introductions_boostrap_and_js.html" target="_blank">page</a>
(<a href="https://github.com/railsgirls-be/railsgirls_guide/blob/gh-pages/materials/introductions_boostrap_and_js.html" target="_blank">source</a>)

* understand the structure, what is the HTML part, the CSS part and the JS part

* understand what Bootstrap is and how we can use it in our file

* go to: [http://getbootstrap.com/](http://getbootstrap.com/) and find some classes to use
