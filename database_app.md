---
layout: default
title: Database app
---

# Database app

Last time we loaded data in via a hash. This had problems:
  * Clunky
  * Inefficient
  * Not scalable
  
## Setting up a basic database

Put following in a file called `db.rb`:

    require 'rubygems'
    require 'active_record'

    ActiveRecord::Base.establish_connection(
      adapter: "sqlite3",
      database: "data/database.sqlite3",
      pool:5,
      timeout:5000 )

    ActiveRecord::Migration.create_table :colleges do |t|
      t.string :name
    end

    class College < ActiveRecord::Base
    end

Copy and paste into irb session. Try the following:

    c = College.new
    c.name = "Oriel"
    c.name

We've created a 'college' object, and set it's name.

    c.save

We've now saved our college object. We can view it by using firefox's sqlite3 dataviewer.


