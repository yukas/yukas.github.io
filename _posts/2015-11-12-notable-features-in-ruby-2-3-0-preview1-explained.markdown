---
layout: post
title:  "Notable features in Ruby 2.3.0-preview1 explained"
date:   2015-11-12 16:28:38
categories: ruby
comments: true
---

## Frozen string literal pragma

Since string literals will be frozen by default in Ruby 3, `frozen_string_literal` pragma at your disposal to prepare for the future:

{% highlight ruby %}
# frozen_string_literal: true

hello = "Hello"
hello << " world!" # => RuntimeError: can't modify frozen String
{% endhighlight %}

This is done for performance reasons, others may say what immutable objects are simple, thread-safe and less error prone.

Don't confuse string literals with string objects. String literals create immutable string objects. But string objects themselves is mutable by default and will always be:

{% highlight ruby %}
# frozen_string_literal: true

hello = String.new   # produces mutable string

hello << "Hello"     # legit call
hello << " world!"   #

hello # => "Hello world!"
{% endhighlight %}

In Rails if you doing something like this:

{% highlight ruby %}
sql =  "SELECT #{sec_id}, pt.path, st.doc_count"
sql << "FROM #{stats_tablename} AS st"
sql << "JOIN #{path_tablename} AS pt ON (st.path_id = pt.id)"
{% endhighlight %}

With the pragma it should look like so:

{% highlight ruby %}
# frozen_string_literal: true

sql = String.new("SELECT #{sec_id}, pt.path, st.doc_count")
sql << "FROM #{stats_tablename} AS st"
sql << "JOIN #{path_tablename} AS pt ON (st.path_id = pt.id)"
{% endhighlight %}

## Safe navigation operator

We used to do `user.try(:company).try(:name)` the Rails way or `user && user.company && user.company.name` the Ruby way, but now we can do it as `user&.company&.name` everywhere. It's defiantly shorter. Other whan this I'd suggest not to go too deep down the chain in favour of **Low of Demeter** and just have `user&.company_name` instead.

Funny thing: Matz first agreed on `user.?company.?name` syntax, but later [reconsidered his decision](https://bugs.ruby-lang.org/issues/11537#note-42):

> I think about this for a while, and thinking of introducing &. instead of .?, because:
> 
> - .? is similar to ?. in Swift and other languages, but is different anyway.
> - Since ? is a valid suffix of method names in Ruby, we already see a lot of question marks in our programs.
> - u&.profile reminds us as short form of u && u.profile.
>
> But behavior of &. should be kept, i.e. it should skip nil but recognize false.
> 
> Matz.

## Did you mean?

[did_you_mean](https://github.com/yuki24/did_you_mean) became part of Ruby as bundled gem (bundled gems are default gems which come preinstalled with ruby).

Watch this:

{% highlight ruby %}
hello = "hello"

puts hell

# undefined local variable or method ‘hell’ for main:Object
#
# Did you mean? hello

{% endhighlight %}

How nice is what? Can't wait when ruby introduces "friend mode", where on every error it will print: "You doing a great job! Keep going, man! You almost there."

This is it on major Ruby 2.3.0-preview1 features, continue by looking at [full changelog](https://github.com/ruby/ruby/blob/8d9e36000747bb650d169d44dd812bfc7e360bd5/ChangeLog).

Good day!