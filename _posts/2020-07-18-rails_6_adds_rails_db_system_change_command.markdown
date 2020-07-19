---
layout: post
title:      "Rails 6 adds rails db:system:change command"
date:       2020-07-18 20:16:53 -0400
permalink:  rails_6_adds_rails_db_system_change_command
---


 One of the main challenge i encountered while building my single page application Rails/Javascript Api was having my database on recovery mode . 
 It all started when after i created my backend-api with the following command 
 
 
 ``` 
 rails new plant-api-backend --api --database=postgresql

 ```
 
 
 This part  ``` --database=postgresql ```  will make our database configuration specific to PostgreSQL . In my case i ran into an issue while running my migrations . 
 
 Come to find out the reason why my rake was aborted the whole time has to do with my postgreslq setup from Homebrews since that s where the error was pointing . After several search on Stackoverflow and others forums and even changing it to the current version .The bug was still persisting .

 
 I decided to uninstall  PostgreSQL and reinstall it .While reinstalling my PostgreSQL through the terminal .My database crashed and start an infinite loop . I force quit + exit my terminal then restart my computer . 
 In another attempt of running back my migrations ; it output that my database was on recovery mode . 
 
 
 Again i went back on some open forums to hunt for a solution . Why my database is on  recovery mode and when can i access it back .Unfortunately there was not an explicit or leading solution .
 
 After brainstorming with one my cohort classmate about my issue . He send me a couples of ressources and blogs . One of them was the project saver . It s pretty new and that command was release on rails version 6.0 to reverse the configuration system . 
 The blog example was laying out how to reverse the configuration to "postgreSQL" .
 
 
 In my case i have  subsituated it  with Sqlite 3 (placeholder) + the associated  gem ``` gem sqlite3``` (gemfile) 
 Change the adapters into Sqlite3 + bundle install . 
 
 And like magic , i reversed my database  from PostgreSql to sqlite3 which let me run my migrations smoothly . 
 
 Below is the main steps from the blog  to reverse a setup database configuration with this rails 6 command .
 
Let’s create a new Rails app.


```
rails new sample
```

Our database.yml contains configuration specific to sqlite3 because rails creates the application with sqlite3 as the default database.

Now if we want to change our database from sqlite3 to PostgreSQL or any other database. Then, we have to manually modify our database.yml to add PostgreSQL related configuration.


```
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>

development:
  <<: *default
  database: sample_development

test:
  <<: *default
  database: sample_test

production:
  <<: *default
  database: sample_production
  username: sample
  password: <%= ENV['SAMPLE_DATABASE_PASSWORD'] %>      

```


And along with this, we also need to remove sqlite3 adapter and add pg gem to our Gemfile.



```
gem 'pg'
```


Now we need to run bundle install to install the pg adapter.

To automate this process, Rails 6 has added rails db:system:change command.

It takes one argument to which specifies the database type. For example, if we want to change our database to PostgreSQL then to will be postgresql.

This command will automatically update our database.yml and Gemfile for the adapter based on the to option provided.

Let’s say, if we want to change the database to PostgreSQL then we need to run following command:


```
rails db:system:change --to=postgresql
    conflict  config/database.yml
Overwrite /Users/romil/Desktop/work/sample/config/database.yml? (enter "h" for help) [Ynaqdhm] Y
       force  config/database.yml
        gsub  Gemfile
        gsub  Gemfile    
```

Now the only thing we need to do is run bundle install and we are all set.

Happy Coding y'all



