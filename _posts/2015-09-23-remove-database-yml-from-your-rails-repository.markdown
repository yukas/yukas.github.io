---
layout: post
title:  "Remove database.yml from your Rails repository"
date:   2015-09-23 15:57:38
categories: rails tips
---

# Problem
Everyone working on the project should be able to have it's own database config.

## Remove database.yml
If you just created new rails project, it's simple: just add database.yml to .gitignore file. If you already fave database.yml inside repo first you need to remove it and commit the change. Them make another commit to .gitignore to ignore it inside repo and then add it back again, this time you'll not see any change in git status - you remove database.yml from your repo.

## Deploy
Use capybara 3 for deploy. There is an option to link files. Place database.yml with environment specific config in deploy_dir/shared/config/database.yml. Capybara will automatically create link to it from app/config directory.

{% highlight ruby %}
set :linked_files, fetch(:linked_files, []).push('config/database.yml')
{% endhighlight %}

Good day!