---
layout: "post"
# override 
title: "Using jekyll _posts"
date: 2017-09-24 15:59:59 -0700
---

Here I will show you how to move around jekyll and add stuff to your website as _posts.  

To run and serve:  
$ jekyll serve

To run for the first time:  
$ jekyll new (project-name)   
$ bundle exec jekyll serve

<!-- Change to a title -->
Front matter:  

Categories - controls the url  
so be wary what you put in the categories: 
every word (separated by a space) is separated by a "/" in the url. 
categories: jekyll new-cat
url: (port)/jekyll/new-cat

to solve this, we turn to "permalink:" ! 
Permalinks override categories in the blog post url. 
You can define it as absolute:  
permalink: /my-new-url/test/test2  
  
Or you can also access certain variables from categories or the date via a permalink, as:  
permalink: /:categories/:day/:year/:month/:title.html

Defaults and config.yml
There are certain defaults to front matter when adding a new post. 
The most common ones are layout, or author, etc. 
In order to make our work much easier, we can actually change the default layout of the front matter for every new post! 
We do this by modifying the config.yml file in the main project directory. 
We just add this line of code:  

defaults: 
  -
    scope:
      path: ""
      type: "posts" # adding types sets the default for certain folders / types 
    values:
        layout: "post"
      #values
  
Do note that you should restart your server every time you modify the config file.  

Themes: 
To change themes, go to "rubygems.org"
and search for: jekyll-theme
Next, add that theme to your Gemfile, using the theme name from the github link:  
gem "jekyll-theme-hacker"  
Then, run $ bundle install on your terminal
  
Now, you can change the theme, in your config(.yml) file to your new theme. 
  
  Note that when you shift to a new theme, you must check the available layouts of that theme from the same github page earlier. 

  Then, you can run $ bundle exec jekyll serve and check 

  Layouts: 

  We can also make our own posts and layouts by adding a "_layouts" folder. Head on over to that folder and check out the files there. It's a great way to learn by applying! 

  Note: There's also a wrapper.html file. This illustrates that we can also use two layouts at the same time, by calling the wrapper layout in the post layout. As you can see, post calls the content, while calling wrapper as the (parent) layout. 

  Essentially, you can use this feature to call improvised layout that you want to wrap in a different style, like customized headers, while only definine the html (inside wrapper) once! 

  Variables: 
  You can use variables instead of plain strings, such that everything is linked! 
  Just enclose them in curly brackets {{ layout.title }}, {{ page.title }}, {{page.author}}. You can also use site information and display them anywhere you want (in the wrapper - set it as the tab title under head - title) as, {{ site.title }} - from the config file. 

  For more information on variables, check out the site: https://jekyllrb.com/docs/variables/
  to make your site more powerful and flexible! 

  Includes: 
  Includes are when you combine several html's so that they don't appear messy in your code. 

  Includes are located in the _includes folder - where you can call them in your layouts or posts, like this:  
  {% include header.html color="blue" %}
  
  Looping through posts: 
  Looping through posts could be used to list the contents of your site. This could be done for easy access - like a table of contents! Here is a sample code of looping through posts in your website: 

  <!-- change this to a code snippet  -->
    {% for post in site.posts %} 
    <!-- could also loop through site.pages -->
        <li><a href="{{ post.url }}">{{post.title}}</a></li>
    {% endfor %}

  if else conditionals: 
  You can also use if else and elseif (elsif) when you want to approach a command dynamically. 
  In addition, you can also use the conditionals 'and' and 'or'.  
  Here is a sample code: 

    {% if page.title == "My First Post" %} <!--you can also use conditonals:  and condition ; or condition -->
        This is the first post 
    {% elsif page.title == "My Second Post" %}
        This is the second post 
    {% else %}
        This is another post 
    {% endif %}

  Here is one way you can use conditionals. Say you want to highlight a post url as a different color (red) when the post is shown as on the page. Here is a sample code that does that:  

    {% for post in site.posts%}
        <li><a style="{% if page.url == post.url %}color:red;{% endif %}" href="{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}

  But take note that conditionals are used in many ways possible, so don't limit your usage to this! 

  Data files: 

  There are three types of data files: yml, json, or csv. 
  These files are read from the _data folder. 
  You can access this with the code: site.data.(filename)
  and continue to access variable tags with the syntax of the form: (tag).(value)

  Here is a sample usage of the code in a for loop - looping through the data file people; then printing out the tags name, and occupation. 

    {% for person in site.data.people %}
        {{ person.name }}, {{ person.occupation }} <br>
    {% endfor %}

  Static files: 

  Static files are files that do not have front matter, examples are jpg, png, pdf, etc. Here we store them in a folder called assets - although it's not a requirement to store the files in this folder - you can place it anywhere site.static_files is able to locate all the static files in our website! Here is a sample:

    {% for file in site.static_files %}
        {{ file.name }} <br>
        <!-- we can access other parameters of the file like file.name file.basename file.extname -->
    {% endfor %}

  With these static files, we can also assign front matter. This is done by adding the front matter to the defaults which we defined earlier in the config file. After assigning a default path to limit what we can display, which could be segragated into folders. Sample code is shown: 

    {% for file in site.static_files %}
        {% if file.image %}
            <img src="{{file.path}}" alt="{file.name}">
        {% endif %}
    {% endfor %}

  While the defaults in the config should look like this: 

    defaults: 
    -
        scope:
        path: "assets/img"
        values:
        image: true
        # scope and values should be at the same tab level