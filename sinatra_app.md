---
layout: default
title: Sinatra app
---

# Sinatra app

In this session we will begin to look at building dynamic websites - sites where the HTML is created on the fly when the user makes the request. This will ultimately allow us to build fully fledged web applications, but now we'll be looking at the simplest example possible, using a lightweight ruby framework called sinatra.

## Getting going with sinatra

We first create a new directory to hold our simple application:

    mkdir greeting_app
    cd greeting_app

Create a new file in this folder called `hi.rb`, with the following contents:

    require 'sinatra'
    
    get '/' do
      "Hello"
    end

That's it! This is pretty much the simplest sinatra app you can write. The first line tells ruby to load the sinatra gem, which contains the code that will tell the ruby server how to interpret the rest of the file.

The next bit tells the server that if it receives a `get` request on the root domain, to respond with the string "Hello". The root of a domain is just the plain address of the website, e.g. `www.example.com` (and not `www.example.com/about`).

To get this code running on your laptop you need to do two things: install the sinatra gem, and then run our example file.

Installing the gem involves pulling the sinatra code down from the internet. Thankfully ruby comes with an excellent code management system in the form of 'Rubygems'. To get the sinatra code you just need to do

    gem install sinatra

You can then run the app with 

    ruby hi.rb

This will start a server on your local machine. You should see some output like:

    == Sinatra/1.3.3 has taken the stage on 4567 for development with backup from Thin
    >> Thin web server (v1.3.1 codename Triple Espresso)
    >> Maximum connections set to 1024
    >> Listening on 0.0.0.0:4567, CTRL+C to stop
    
You can visit your site by typing localhost:4567 into your browser. You should see the word "Hello".

## Getting personal

We have a basics of a dynamic site up and running, but so far it's all pretty underwhelming. Don't get too despondent though - this next part's almost guaranteed to completely blow your mind. We're going to get the app to personalise its greeting. Add the following piece of code to `hi.rb`:

    get '/:name' do
      "Hello #{params[:name]}"
    end

You should now try visiting localhost:4567/tom (or similar). It probably won't work at first - you'll get an error page saying something like "Sinatra doesn't know this ditty". The server doesn't know about the changes to the ruby file. To make this work you need to restart the server - go to the console, press Ctrl-c, and then do `ruby hi.rb` again. If all is well, when you reload the page in your browser you should see "Hello tom". 

When you've finished cleaning bits of mind off your keyboard, you might start wondering if we've done anything special at all here - after all, you could get the same effect with a static site. I guess the point is that with a static site you'd need to create an individual page for each possible name, which would soon become unweildy and unmanageable. Here we've taken some input from the url specified by the user and crafted a response around it.

### Excercise

See if you jazz this up a bit by making the greeting a bit longer. Maybe you could add some randomness, so that it says "Hi" sometimes. You might want to investigate ruby's Array#shuffle method for this.

## Grown-up responses

[ Not sure I need anything here - sinatra seems to manage response codes ok ]

## An app with a view

So far we've just returned a text string to be displayed in the browser. What if we want to return html? 

The best way to return custom html is to use a template - a basic html `shell` into which your specifics can be injected.

There are many different options when it comes to templates and templating languages (the form of the instructions you use to tell the template interpreter where and how to insert your values). In ruby two of the front-runners are `erb` (Embedded RuBy) and `haml` (Html Abstraction Markup Language). Haml is really nice - it uses a stripped down syntax, which produces beautifully clean templates, but erb looks a lot closer to html so is easier to understand. We'll use erb for the moment.

We'll start by reproducing our current behaviour but using an erb template. You need to create a new folder in the greeting_app directory called `views`:

    mkdir views

Then create a file called `hello.erb` with the following contents:

    Hello <%= @name %>

You'll notice the `<%= ... %>` tags. These are erb's way of saying 'treat the following as ruby code'.

We also need to make changes in `app.rb`:

    get '/:name' do
      @name = params[:name]
      erb :hello
    end

The erb code is included in ruby, so this should all work fine. If we had decided to use haml, we would have needed to install the haml gem, just like we did with sinatra.

## A bigger example

We're still only returning a text string to the browser. To get a better idea of what can be done with templates it's good to look at a bigger example. Luckily I prepared one earlier. To get this, go back to the shell and move into the folder containing greeting_app (`cd ..`). Then:

    git clone git://github.com/TomClose/norrington_app.git

We'll start by taking a quick look around.

    cd norrington_app
    ls

You'll recognise a few things here: for starters we have an `app.rb`, and a `views` folder. No surprises there. You'll also notice a `data` folder, and a `public` folder. The `data` folder is nothing special - it just contains the raw data used in this app. The `public` folder is a special folder that sinatra knows about; it's used for storing assets that will be served directly to the browser, e.g. css files, javascript files, and images.

## Managing gems

There's also a file called `Gemfile`. If you take a look inside this file (`cat Gemfile`) you'll see a few lines, including:

    gem sinatra

Gemfiles are ruby's way of specifying which gems a project depends on. When we're working alone it's ok to install gems one-by-one using `gem install ...`, but this can get tedious when working on a big project with others. You can also run into problems with different gem versions, which can cause subtle and annoying bugs.

To make use of the Gemfile you need the bundler gem installed:

    gem install bundler

You can then load all the gems specified in the Gemfile, by typing

    bundle install

You'll also notice a file called Gemfile.lock. This specifies the precise versions of the gems that were installed when I did `bundle install` on my local machine. You should now have precisely the same gems installed on yours.

It's possible to get a lot more advanced with gemfiles. For example you can use the gemfile to specify that you want a specific version of a gem, or a sufficiently recent one, or even provide the precise location (e.g. someone's github repo) where you want to install the gem from. We'll come across more complicated Gemfiles later.

## Running the example

You can run the `norrington_app` example as before, by doing

    ruby app.rb

[TODO: Write about explaining the code in app.rb]

### Excercise

Extend the Norrington app so that when I visit `localhost:4567/colleges/Oriel` I get a list of Oriel's Norrington scores over the years.


### Excercise

Add Mixpanel analytics to your code, using the ruby library


