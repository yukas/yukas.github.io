---
layout: post
title:  "Remove database.yml from your Rails repository"
date:   2015-09-23 15:57:38
categories: rails tips
comments: true
---

## Problem
Everyone working on a project should be able to have it's own database config.

## Remove database.yml
If you just created a new Rails project, it's simple — just add database.yml to .gitignore file.

If you already have a database.yml inside the repository first you need to remove it and commit changes. Then you need to change .gitignore file to ignore database.yml. Now you can add database.yml file back and as soon as it is already ignored inside the repository, you'll see no changes. Now you have your own database.yml file.

## Deploy
Use Capistrano 3 for deploy. There is an option in it to link files. Place database.yml with production config in `deploy_dir/shared/config/database.yml`. Capybara will automatically create link to it from app/config directory.

{% highlight ruby %}
set :linked_files, fetch(:linked_files, []).push('config/database.yml')
{% endhighlight %}

Good day!
