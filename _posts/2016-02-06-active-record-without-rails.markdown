---
layout: post
title:  "ActiveRecord without Rails"
date:   2016-02-03 16:57:38
categories: active_record
comments: true
---

### The problem
You need to use common active record tasks such as create/purge database, dump/load the schema without Rails.

### Create a database from a schema
{% highlight ruby %}
require "active_record"

db_config = {
  "adapter"  => "sqlite3",
  "database" => "test.db"
}

ActiveRecord::Base.establish_connection(db_config)

ActiveRecord::Schema.define do
  create_table :blogs do |table|
    table.column :title, :string
  end

  create_table :posts do |table|
    table.column :blog_id, :integer
    table.column :title, :string
    table.column :body, :text
  end
end
{% endhighlight %}

### Dump current schema
{% highlight ruby %}
require "active_record"

db_config = {
  "adapter"  => "sqlite3",
  "database" => "test.db"
}

ActiveRecord::Base.establish_connection(db_config)

ActiveRecord::SchemaDumper.dump(ActiveRecord::Base.connection, File.new("schema.rb", "w"))
{% endhighlight %}

`schema.rb` will be placed in the current working directory (check current directory by running `Dir.pwd`).

### Load the schema into the database
{% highlight ruby %}
require "active_record"

db_config = {
  "adapter"  => "sqlite3",
  "database" => "test.db"
}

ActiveRecord::Tasks::DatabaseTasks.load_schema_for(db_config, :ruby, "schema.rb")
{% endhighlight %}

Second parameter can be `:ruby` or `:sql` depending on the type of the schema file.

### Purge the database
{% highlight ruby %}
require "active_record"

db_config = {
  "adapter"  => "sqlite3",
  "database" => "test.db"
}

ActiveRecord::Tasks::DatabaseTasks.root = Dir.pwd
ActiveRecord::Tasks::DatabaseTasks.purge(db_config)
{% endhighlight %}

`DatabaseTasks.root` is the directory where your `database` file will be searched.
In my case it's the current working directory.

### Need more tasks to run?
`bundle open activerecord` and read through the `lib/active_record/tasks/database_tasks.rb` file.