---
layout: post
title:  "Anatomy of good service object"
date:   2015-10-22 14:52:38
categories: ruby design
comments: true
---

## What is good service object?
* Single responsibility
* Checks it's input
* Cammand/query separation
* Same level of abstraction
* Transparent
* Tested

## Example of good service object

Let's start by giving a simple example:

{% highlight ruby %}
class UserCreator
  attr_reader :email, :first_name, :last_name
  attr_reader :created_user

  def initialize(email, first_name, last_name)
    @email      = email
    @first_name = first_name
    @last_name  = last_name

    raise ArgumentError, "Missing email" if email.nil?
  end

  def create_user
    check_user_existance
    save_user
  end

  def user_created?
    created_user.present?
  end

  private

  def check_user_existance
    raise "User already exists" if User.exists?(...)
  end

  def save_user
    @created_user = User.create(...)
  end
end
{% endhighlight %}

## Single responsibility

As we see above, `UserCreator` is single responsible for creating user. No email
sending or whatever. If you need, for example, to send welcome email it is
probably should be done from another service, say `WelcomeEmailSender`.
The smaller construction blocks in your program the less pain it would
be to change them.

## Checks it's input

It's simple, don't check email address for being nil outside of the service. This
will spread such checks all across the codebase. Object constructor is the one who's responsible for setting object invariant - state in which object can perform without braking. If your service needs email address to work correctly and it is not given - brake early, throw exception from constructor.


## Command/query separation

Usually objects have two types of methods: *commands* and *queries*. Commands
change something and return nothing, queries in opposite return current
state without changing anything. This separation brings clarity in how and when
you use particular method, so you don't endup in situation when asking object
for it's state also changes it or sends email.

## Same level of abstraction

Term abstraction is about hiding details. In `#create_user` we hide checking user existence and saving user *details* inside `#check_user_existance` and `#save_user` methods. We'd brake this rule if some details are still left inside `#create_user` method.

## Transparent

In other words you should be able to inspect input parameters, which usually got supplied
through constructor, hense you should have attribute readers for every input parameter. This especially handy upon debugging.

## Tested

According to Sandi Metz, you should test object query methods by asserting
returned result, command methods - by asserting direct public side effect and
outgoing command methods, by expecting to send them. Usually, I would test such
service as so:

{% highlight ruby %}
describe UserCreator do
  describe "#create_user" do
    it "creates user" do
      user_creator = UserCreator.new("ian@brown.com", "Ian", "Brown")
      user_creator.create_user

      expect(user_creator.created_user.email).to      eq("ian@brown.com")
      expect(user_creator.created_user.first_name).to eq("Ian")
      expect(user_creator.created_user.last_name).to  eq("Brown")
    end

    context "no email given" do
      it "raises exception" do
        expect {
          UserCreator.new(nil, "Ian", "Brown")
        }.to raise_error(/Missing email/)
      end
    end

    context "user already exists" do
      ...
    end
  end
end
{% endhighlight %}

## The anatomy

Trying to visualize service object I came out with this picture:

![Service anatomy picture]({{ site.url }}/images/service_anatomy.jpg)

## Further reading

[The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)

[Command Query Separation](http://martinfowler.com/bliki/CommandQuerySeparation.html)

[Writing confident code](https://practicingruby.com/articles/confident-ruby)

[The Magic Tricks of Testing by Sandi Metz](https://www.youtube.com/watch?v=URSWYvyc42M)