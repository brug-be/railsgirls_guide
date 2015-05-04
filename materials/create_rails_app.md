---
layout: default
title: Create Rails App
permalink: /create_rails_app/
---

# Create the rails app

* download the  <a href="https://railsgirls-be.github.io/railsgirls_guide/materials/introductions.html" target="_blank">introductions.html</a> file to your computer

* open the terminal

{% highlight bash %}
rails new railsgirls-app
cd railsgirls-app
mv --your-downloads-folder-here--/introductions.html public/
(i.e. mv ~/Downloads/introductions.html public/)
{% endhighlight %}

## Start rails app

* `rails s`
* visit [http://localhost:3000/introductions.html](http://localhost:3000/introductions.html)

# Create pages controller

* open another terminal window and go to your app directory (`cd railsgirls-app`) to continue
* `rails g controller pages`
* Open the file `config/routes.rb` in your text editor. The first line will look something like that:  `Rails.application.routes.draw do`
* add the following as the second line of the file

{% highlight ruby %}
get "/pages/introductions" => 'pages#introductions'
{% endhighlight %}

* Now open the file `app/controllers/pages_controller.rb` in your editor. After the first line add the following

{% highlight ruby %}
def introductions
  render text: 'introductions'
end
{% endhighlight %}

* visit [http://localhost:3000/pages/introductions](http://localhost:3000/pages/introductions) 
* `mv public/introductions.html app/views/pages/introductions.html.erb`

* Go back to the file `app/controllers/pages_controller.rb` and remove the line that starts with `render`.

{% highlight ruby %}
def introductions
end
{% endhighlight %}

* visit [http://localhost:3000/pages/introductions](http://localhost:3000/pages/introductions)

# Now let's try something dynamic

* Open the file `app/views/pages/introductions.html.erb`. See how we explicitly state the year in the section `<footer>`? Let's have Rails provide the current year for us dynamically. To do that change the code as follows, and reload the page to see that the year is still there.

{% highlight erb %}
<footer>
  Created by 42 in <%= Time.now.year %>.
</footer>
{% endhighlight %}

* (Show how this also works in IRB and maybe the controller)

* Now go back to the file `app/controllers/pages_controller.rb`. We'll be introducing a variable to hold the value of the year now. 

{% highlight ruby %}
def introductions
  @year = Time.now.year
  render :introductions
end
{% endhighlight %}

* And in `app/views/pages/introductions.html.erb`:

{% highlight erb %}
<footer>
  Created by 42 in <%= @year %>.
</footer>
{% endhighlight %}

Other examples could be the

* Calculate age of the participant
* Try a dynamic image using some API or something...

# Iterate over lists

Indicate that re-writing lists is boring! We don't want to repeat ourselves.

Introduce the idea of arrays for the hobbies and languages lists in the HTML.

{% highlight ruby %}
def introductions
  @year = Time.now.year
  @hobbies = ['Painting', 'Eating', 'Cooking', 'Whatever']
  @languages = ['Dutch', 'English', 'Html', 'Ruby']
  render :introductions
end
{% endhighlight %}

First introduce the list rendering like so (without the classes, or maybe even without HTML.)

{% highlight erb %}
<ul>
  <% @hobbies.each do |hobby| %>
    <li>
      <%= hobby %>
    </li>
  <% end %>
</ul>
{% endhighlight %}

In a second pass, you can show how we only need to add the classes/html once, thus making our life easier.

{% highlight erb %}
<ul class="list-group">
  <% @hobbies.each do |hobby| %>
    <li class="list-group-item">
      <%= hobby %>
    </li>
  <% end %>
</ul>
{% endhighlight %}

Don't forget to do the same for languages.

If the participant wants to keep the "see more" functionality you can go with 3 options.

Option 1: Just bring it back, using a each_with_index

{% highlight ruby %}
<ul class="list-group">
  <% @hobbies.each_with_index do |hobby, i| %>
    <li class="list-group-item <% i < 3 ? '' : 'hide' %>">
      <%= hobby %>
    </li>
  <% end %>
</ul>
{% endhighlight %}

Option 2: Just bring it back, using `first` and `drop`

{% highlight erb %}
<ul class="list-group">
  <% @hobbies.first(3).each do |hobby, i| %>
    <li class="list-group-item">
      <%= hobby %>
    </li>
  <% end %>
  <% @hobbies.drop(3).each do |hobby, i| %>
    <li class="list-group-item">
      <%= hobby %>
    </li>
  <% end %>
</ul>
{% endhighlight %}

Option 3: Talk them out of it

As and added bonus you could try to make the "show more" link work as real link sending a param to the controller, changing the behaviour of the code.

# Introduce a second page and layout

Introduce the idea of having a second page

* cp app/views/pages/introductions.html.erb app/views/pages/second_page.html.erb
* Add the route

{% highlight ruby %}
get "/pages/second_page" => 'pages#second_page'
{% endhighlight %}

You can now explain the magic of rails, why this works without writing the action in the controller.

To trigger the "need" for layouts we will now add a menu to one of the pages.

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
            <li><a href="/pages/introductions">Introduction</a></li>
            <li><a href="/pages/second_page">Second page</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
{% endhighlight %}

Let the participant discover that it only in one page. Prevent them from copy/pasting this to the other page. Explain that we can do better.

Explain the concept of layouts.

* Create or replace layout in app/views/layouts/application.html.erb
* Move the common HTML into this file (don't forget the menu)
* Do not forget the `<% yield %>`
* Remove the common HTML from the controller action views

Let them discover the awesomeness of layouts.

At this point (if you were using the `@year` variable) the second page might break (not showing it). Explain the scope of variables per action. Maybe even explain the difference between variable with and without an @.
Fix this issue either by creating a before_action, a helper method or just inlining the code.

# Adding a DB

Explain that changing the code for each new hobby or language would be very boring. We can add forms and a database for this.

Start with generating a scaffold for hobbies or languages

{% highlight bash %}
rails g scaffold hobby name
rake db:create db:migrate
{% endhighlight %}

{% highlight bash %}
rails g scaffold language name
rake db:create db:migrate
{% endhighlight %}

Show what rails generated give a quick tour of the views and controller. Focus on the model.

Let them create/edit/delete some entries.

Now bring this to their own page.

{% highlight ruby %}
def introductions
  @year = Time.now.year
  @hobbies = Hobby.all
  @languages = Language.all
  render :introductions
end
{% endhighlight %}

This will suck a bit, as rails will put the object inspect into the HTML. explain that now we must either teach the model how to show itself as a piece of text, or have the view use the name (like in the scaffolds.)

{% highlight ruby %}
def to_s
  name
end
{% endhighlight %}

{% highlight erb %}
<ul class="list-group">
  <% @hobbies.each_with_index do |hobby, i| %>
    <li class="list-group-item <% i < 3 ? '' : 'hide' %>">
      <%= hobby.name %>
    </li>
  <% end %>
</ul>
{% endhighlight %}

Show how this works in the console as well.
