---
layout: default
title: Part 2
permalink: /part_2/
---

# Part 2 - Creating the Rails app

* Open the terminal and create a new Rails app

{% highlight bash %}
rails new railsgirls-app
cd railsgirls-app
{% endhighlight %}

* Have a look how Rails generated a folder structure and a lot of files for you. There are two ways of doing that, in the terminal or with Finder/Explorer. Ask your coach about the details. Once you have seen them, move your ```about.html``` to the public folder.

{% highlight bash %}
mv --your-downloads-folder-here--/about.html public/
(i.e. mv ~/Downloads/about.html public/)
{% endhighlight %}

## Start rails app

* `rails server`
* visit [http://localhost:3000/about.html](http://localhost:3000/about.html)

# Create pages controller

* open another terminal window and go to your app directory (`cd railsgirls-app`) to continue
* `rails generate controller pages`
* Open the file `config/routes.rb` in your text editor. The first line will look something like that:  `Rails.application.routes.draw do`
* add the following as the second line of the file

{% highlight ruby %}
get "/pages/about" => 'pages#about'
{% endhighlight %}

* Now open the file `app/controllers/pages_controller.rb` in your editor. After the first line add the following

{% highlight ruby %}
layout false

def about
  render file: 'public/about'
end
{% endhighlight %}

* visit [http://localhost:3000/pages/about](http://localhost:3000/pages/about)

Does it work?

* `mv public/about.html app/views/pages/about.html.erb`

* Go back to the file `app/controllers/pages_controller.rb` and remove the line that starts with `render`.

{% highlight ruby %}
def about
end
{% endhighlight %}

* visit [http://localhost:3000/pages/about](http://localhost:3000/pages/about)

# Now let's try something dynamic

* Open the file `app/views/pages/about.html.erb`. See how we explicitly state the year in the section `<footer>`? Let's have Rails provide the current year for us dynamically. To do that change the code as follows, and reload the page to see that the year is still there.

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

# Iterate over lists

Imagine part of your pages looks like this:

{% highlight html %}
<ul>
    <li>Painting</li>
    <li>Eating</li>
    <li>Cooking</li>
    <li>Whatever</li>
</ul>
{% endhighlight %}

Adding another hobby here means you have to repeat writing the HTML tags each and every time.
Have a look what we can do using dynamic lists (arrays) in ruby. In the file `app/controllers/pages_controller.rb`
add the following:

{% highlight ruby %}
def about
  @name = 'Jane Doe'
  @year = Time.now.year
  @hobbies = ['Painting', 'Eating', 'Cooking', 'Whatever']
  @languages = ['Dutch', 'English', 'Html', 'Ruby']
end
{% endhighlight %}

* And in `app/views/pages/about.html.erb`:

{% highlight erb %}
<ul>
  <% @hobbies.each do |hobby| %>
    <li>
      <%= hobby %>
    </li>
  <% end %>
</ul>
{% endhighlight %}

Don't forget to do the same for languages.

(If you want to go wild ask your coach about the following code:

Option 1: Just bring it back, using a each_with_index

{% highlight erb %}
<ul class="list-group">
  <% @hobbies.each_with_index do |hobby, i| %>
    <li class="list-group-item <%= i < 3 ? '' : 'hide' %>">
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
{% endhighlight %})

# Introduce a second page and layout

Introduce the idea of having a second page

* cp app/views/pages/about.html.erb app/views/pages/second_page.html.erb
* Add the route

{% highlight ruby %}
get "/pages/second_page" => 'pages#second_page'
{% endhighlight %}

* Make some changes on the page and open [http://localhost:3000/pages/second_page](http://localhost:3000/pages/second_page)

You have now created a dynamic web application with multiple interconnected pages. Feels good, right? :)

* High five your neighbours.

Now, let's add a menu to connect the pages. This will trigger the need for something called "layouts"

Add this code and remember to include bootstrap, if you haven't yet (ask your coach).

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
            <li><a href="/pages/second_page">Second page</a></li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</nav>
{% endhighlight %}

See that this only appears in one page. Why?

Introducing: layouts.

* Open `app/views/layouts/application.html.erb`
* Move the navbar elements into this file
* Remove the common HTML from the erb pages
* Remove `layout false`

# Adding a DB

Now let's add the next level of magic. Let's use the database to store the languages and hobbies, because editing your code every time you get a now hobby is for amateurs.

{% highlight bash %}
rails generate scaffold hobby name:string
rake db:create
rake db:migrate
{% endhighlight %}

{% highlight bash %}
rails generate scaffold language name:string
rake db:migrate
{% endhighlight %}

Check out [http://localhost:3000/hobbies](http://localhost:3000/hobbies) and [http://localhost:3000/languages](http://localhost:3000/languages)

Try out the forms and create, edit and delete a couple of entries.

Now let's use the hobbies and languages stored in the database.

{% highlight ruby %}
def about
  @year = Time.now.year
  @hobbies = Hobby.all
  @languages = Language.all
end
{% endhighlight %}

Now go check what the page looks like.

Does it look weird?!

To fix this add the following code:

{% highlight erb %}
<ul class="list-group">
  <% @hobbies.each_with_index do |hobby, i| %>
    <li class="list-group-item <% i < 3 ? '' : 'hide' %>">
      <%= hobby.name %>
    </li>
  <% end %>
</ul>
{% endhighlight %}


# The Rails console

Open the rails console:

{% highlight bash %}
rails console
{% endhighlight %}

{% highlight ruby %}
Language.create(name: 'ruby')
{% endhighlight %}

And now have another look at: [http://localhost:3000/languages](http://localhost:3000/languages). What changed?

# Rubygems

Ask the coach "what is a gem?" and be prepared for an interesting lecture about programmer happiness.

Try adding the gem [youtube_addy](https://github.com/datwright/youtube_addy) to your `Gemfile` and creating a new scafford for videos. NOTE that the keyword `raw` needs to be added to make the gem work correctly (`<%= raw YouTubeAddy.youtube_embed_url("your-url-here",420,315) %>`).

Ask the coach for his/her favourite gems.


