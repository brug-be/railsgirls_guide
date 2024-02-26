---
layout: default
title: Part 2
permalink: /part_2/
---

# Part 2 - Introducing Rails

## Creating your (first) Rails app

Open the terminal to create a new Rails app

{% highlight bash %}
rails new railsgirls-blog
cd railsgirls-blog
{% endhighlight %}

Find the project directory in the Finder/Explorer. Have a look how Rails generated a folder structure and a lot of files for you. Ask your coach about the details.


## Starting the Rails server

A Rails app can deliver dynamically generated content with the help of the Ruby programming language. This is why we can't just open a file anymore in the browser. We need a program that will combine the templates with the dynamic information to create the final HTML. This program is called the Rails server.

To start the Rails server, open you terminal (in the `railsgirls-blog` folder) and type the following:

{% highlight bash %}
rails server
{% endhighlight %}

Once it's started up, you can go visit your app under

[http://localhost:3000/](http://localhost:3000/).

The page indicates that everything worked fine and the server is running.

You can compare the server with any other computer application you start. This means you also need to stop it when you no longer need it. Do this with the keyboard shortcut `CTRL-C` inside the command line tool

## The power of Rails: creating a scaffold

One (not so secret) weapon of Rails is its ability to quickly get you started using scaffolds. Since we're trying to create blog here, let's put everything into place to create, update and show our blog articles.

To do that type the following commands into your terminal:

{% highlight bash %}
rails generate scaffold article title:string body:text author:string
rails db:create
rails db:migrate
{% endhighlight %}

Now you can checkout your app again in the browser. If you check

[http://localhost:3000/](http://localhost:3000/)

You're not going to notice any difference, since we haven't told our app about what to do when someone hits the home page just yet. For now you still have to know where to look for the articles, which is under

[http://localhost:3000/articles](http://localhost:3000/articles).

Click around and explore what your app can do already. Which is quite a lot. It doesn't look very pretty yet but try to discover that creating, editing and deleting of articles completely works out of the box. Make sure to create at least three articles so we have some data in our app to play with going forward.

## Tweaking the default to our needs

The power of Rails is that it can generate a lot of underlying functionality for you automatically. But what's equally important is that it allows us to work with what was created and tweak it to our needs. Let's try that with our articles index page to make it look a bit more like a blog.

In your editor open the file `app/views/articles/index.html.erb`. You'll notice that this looks nearly like the HTML you're already used to. Apart from some small differences. We'll get into those shortly. For now just go ahead and replace everything after the `h1` tag with the following code:

{% highlight ruby %}
<% @articles.each do |article| %>
   <h2><%= article.title %></h2>
   <p><%= article.body %></p>

   <%= link_to 'Show', article %>
   <%= link_to 'Edit', edit_article_path(article) %>
   <%= link_to 'Destroy', article, method: :delete, data: { confirm: 'Are you sure?' } %>
 <% end %>

 <br>
 <%= link_to 'New Article', new_article_path %>
{% endhighlight %}

Save it and go have a look at the result in your browser.

# What about our about page now?

Let's bring the two parts of this guide together. We want to integrate the about page we created in the first part into our Rails app. For that we will use our Rails' generator again and let it create all the setup we need for that page. To do that open the terminal again and type the following line:

{% highlight bash %}
rails generate controller pages about
{% endhighlight %}

If you open [http://localhost:3000/pages/about](http://localhost:3000/pages/about) you will see the page that Rails has created for us. It understands the URL and shows us some placeholder content. Let's now put our about page from earlier in that place. Open the file mentioned on the web page in your editor. Now paste the complete content from our about.html page into this file.

visit [http://localhost:3000/pages/about](http://localhost:3000/pages/about)

## Something dynamic

Now that our page is part of a Rails app there are many possibilities of making the content more dynamic. You might have noticed the `.erb` extension on our view file earlier. This allows us to run actual ruby inside our html. Whenever you see `<%= %>` or `<% %>` this means you're in Ruby land within HTML. Here are a couple of examples to see that at work:

Open the file `app/views/pages/about.html.erb`. See how we explicitly state the year in the section `<footer>`? Let's have Rails provide the current year for us dynamically. To do that change the code as follows, and reload the page to see that the year is still there.

{% highlight erb %}
<footer>
  Created by 42 in <%= Time.now.year %>.
</footer>
{% endhighlight %}

* (Show how this also works in IRB and maybe the controller)

* Now go back to the file `app/controllers/pages_controller.rb`. We'll be introducing a variable to hold the value of the year now.

{% highlight ruby %}
def about
  @name = 'Jane Doe'
end
{% endhighlight %}

* And in `app/views/pages/about.html.erb`:

{% highlight erb %}
<footer>
  Created by <%= @name %> in <%= Time.now.year %>.
</footer>
{% endhighlight %}

{% highlight erb %}
<h1> My name is <%= @name %>.</h1>
{% endhighlight %}

Other examples could be the

* Calculate age of the participant
* Try a dynamic image using some API or something...

### Iterate over lists

Imagine part of your pages looks like this:

{% highlight html %}
<ul>
    <li>French</li>
    <li>English</li>
    <li>Clingon</li>
    <li>Whatever</li>
</ul>
{% endhighlight %}

Adding another language here means you have to repeat writing the HTML tags each and every time.
Have a look what we can do using dynamic lists (arrays) in ruby. In the file `app/controllers/pages_controller.rb`
add the following:

{% highlight ruby %}
def about
  @year = Time.now.year
  @languages = ['Dutch', 'English', 'Html', 'Ruby']
end
{% endhighlight %}

* And in `app/views/pages/about.html.erb`:

{% highlight erb %}
<ul>
  <% @languages.each do |language| %>
    <li>
      <%= language %>
    </li>
  <% end %>
</ul>
{% endhighlight %}


### And now from the database

We're ready for the next level of magic: Let's use the database to store the languages, because editing your code every time you learn a new language is going to get boring fast. ;)

{% highlight bash %}
rails generate scaffold language name:string
rails db:migrate
{% endhighlight %}

Check out [http://localhost:3000/languages](http://localhost:3000/languages)

Try out the forms and create, edit and delete a couple of entries.

Now see how we can use the languages stored in the database. For that we tell the controller to load the information from the database. Open the file `app/controllers/pages_controller.rb` in your editor and add the line about languages.

{% highlight ruby %}
def about
  @year = Time.now.year
  @languages = Language.all
end
{% endhighlight %}

In `app/views/pages/about.html.erb` replace the line `<%= language %>` with:

{% highlight erb %}
<%= language.name %>
{% endhighlight %}

Now go check what the page looks like.

# Let's add a contact page and feel the need for a layout

We want to add another page to our blog that contains our contact information. Instead of using the generator we'll do this manually so you get to see the steps that are invovled. No worries there is no need to learn those by heart. Just follow along and let's see what happens.

Open a new file in your editor and save it as `app/views/pages/contact.html.erb`. Copy paste the following content in it and save again:

{% highlight html %}
<h1>Contact</h1>

<p>
  You can contact the author of this block under a@bc.de
</p>

<p>
  To learn more about the author <a href="/pages/about">click here</a>.
</p>
{% endhighlight %}

We need to add something called a route so the server knows what to do if someone visits the URL `/pages/contact`. For that open the file `config/routes.rb` in your editor and add the following line directly above `get 'pages/about'`.

{% highlight ruby %}
get '/pages/contact' => 'pages#contact'
{% endhighlight %}

Let's see what we've got [http://localhost:3000/pages/contact](http://localhost:3000/pages/contact).

You have now created a dynamic web application with multiple interconnected pages. Feels good, right? :)

* High five your neighbours.

Now, let's add a menu to connect the pages.

Add this code on top of your contact page and remember to include bootstrap, if you haven't yet (ask your coach).

{% highlight html %}
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">My cool page</a>
    </div>

    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu" role="menu">
            <li><a href="/pages/about">About</a></li>
            <li><a href="/pages/contact">Contact</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
{% endhighlight %}

Find your pages in the dropdown menu. See that this only appears in one page. Why?

Introducing: layouts.

* Open `app/views/layouts/application.html.erb`
* Move the navbar elements into this file
* Remove the common HTML from the erb pages


# The Rails console

Open the rails console:

{% highlight bash %}
rails console
{% endhighlight %}

{% highlight ruby %}
Language.create(name: 'ruby')
{% endhighlight %}

And now have another look at: [http://localhost:3000/languages](http://localhost:3000/languages). What changed?

You can compare the console a bit with the rails server. It is also just an application. If you want to quit the rails console use the keyboard shortcut `CTRL-D` or by typing `exit` and pressing `ENTER`.

# Rubygems

Ask the coach "what is a gem?" and be prepared for an interesting lecture about programmer happiness.

Try adding the gem [youtube_addy](https://github.com/datwright/youtube_addy) to your `Gemfile` and creating a new scafford for videos. NOTE that the keyword `raw` needs to be added to make the gem work correctly (`<%= raw YouTubeAddy.youtube_embed_url("your-url-here",420,315) %>`).

Ask the coach for his/her favourite gems.
