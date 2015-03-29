# Create the rails app

* rails g myapp
* cd myapp
* mv INTRODUCTION_FILE public/

## Start rails app

* rails s
* visit http://localhost:3000/introductions.html

# Create pages controller

* rails g controller pages
* Add the route

```ruby
get "/pages/introductions" => 'pages#introductions'
```

* Add the action

```ruby
def introductions
  render text: 'introductions'
end
```

* visit http://localhost:3000/pages/introductions
* mv public/introductions.html app/views/pages/introductions.html.erb

* Remove the render call

```ruby
def introductions
  render :introductions
end
```

* visit http://localhost:3000/pages/introductions

# Try something dynamic

* Put the year dynamically in the footer

```erb
<footer>
  Created by 42 in <%= Time.now.year %>.
</footer>
```

* Show how this also works in IRB and maybe the controller


```ruby
def introductions
  @year = Time.now.year
  render :introductions
end
```

```erb
<footer>
  Created by 42 in <%= @year %>.
</footer>
```

Other examples could be the

* Calculate age of the participant
* Try a dynamic image using some API or something...

# Iterate over lists

Indicate that re-writing lists is boring! We don't want to repeat ourselves.

Introduce the idea of arrays for the hobbies and languages lists in the HTML.

```ruby
def introductions
  @year = Time.now.year
  @hobbies = ['Painting', 'Eating', 'Cooking', 'Whatever']
  @languages = ['Dutch', 'English', 'Html', 'Ruby']
  render :introductions
end
```

First introduce the list rendering like so (without the classes, or maybe even without HTML.)

```erb
<ul>
  <% @hobbies.each do |hobby| %>
    <li>
      <%= hobby %>
    </li>
  <% end %>
</ul>
```

In a second pass, you can show how we only need to add the classes/html once, thus making our life easier.

```erb
<ul class="list-group">
  <% @hobbies.each do |hobby| %>
    <li class="list-group-item">
      <%= hobby %>
    </li>
  <% end %>
</ul>
```

Don't forget to do the same for languages.

If the participant wants to keep the "see more" functionality you can go with 3 options.

Option 1: Just bring it back, using a each_with_index

```erb
<ul class="list-group">
  <% @hobbies.each_with_index do |hobby, i| %>
    <li class="list-group-item <% i < 3 ? '' : 'hide' %>">
      <%= hobby %>
    </li>
  <% end %>
</ul>
```

Option 2: Just bring it back, using `first` and `drop`

```erb
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
```

Option 3: Talk them out of it

As and added bonus you could try to make the "show more" link work as real link sending a param to the controller, changing the behaviour of the code.

# Introduce a second page and layout

Introduce the idea of having a second page

* cp app/views/pages/introductions.html.erb app/views/pages/second_page.html.erb
* Add the route

```ruby
get "/pages/second_page" => 'pages#second_page'
```

You can now explain the magic of rails, why this works without writing the action in the controller.

To trigger the "need" for layouts we will now add a menu to one of the pages.

```html
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
```

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

```
rails g scaffold hobby name
rake db:create db:migrate
```

```
rails g scaffold language name
rake db:create db:migrate
```

Show what rails generated give a quick tour of the views and controller. Focus on the model.

Let them create/edit/delete some entries.

Now bring this to their own page.

```ruby
def introductions
  @year = Time.now.year
  @hobbies = Hobby.all
  @languages = Language.all
  render :introductions
end
```

This will suck a bit, as rails will put the object inspect into the HTML. explain that now we must either teach the model how to show itself as a piece of text, or have the view use the name (like in the scaffolds.)

``` ruby
def to_s
  name
end
```

```erb
<ul class="list-group">
  <% @hobbies.each_with_index do |hobby, i| %>
    <li class="list-group-item <% i < 3 ? '' : 'hide' %>">
      <%= hobby.name %>
    </li>
  <% end %>
</ul>
```

Show how this works in the console as well.
