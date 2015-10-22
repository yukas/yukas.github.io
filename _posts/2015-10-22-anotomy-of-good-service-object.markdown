---
layout: post
title:  "Anatomy of good service object"
date:   2015-10-22 14:52:38
categories: ruby design
---

## What is good service object?
* Single responsibility
* Cammand/query separation
* Transparent about input output
* Checks it's own input
* Tested

## Example of good service object

{% highlight ruby %}
class CreateUser
  attr_reader :email, :first_name, :last_name
  attr_reader :created_user

  def initialize(email, first_name, last_name)
    @email      = String(email)
    @first_name = String(first_name)
    @last_name  = String(last_name)

    raise "No email given" if email.blank?
  end

  def create_user
    check_user_existance!
    create_user_object
  end

  def user_created?
    @created_user.present?
  end

  private

  def check_user_existance
    User.exists?(...)
  end

  def create_user_object
    @created_user = User.create(...)
  end
end
{% endhighlight %}

## Single responsibility

As we see below, UserCreator is single responsible for creating user. No email
sending or whatever. If you need, for example, to send welcome email it's
probably should be done from another service, say `WelcomeEmailSender`. Why?
The smaller construction blocks in your program the less pain it would
be to change them.

## Command/query separation

Usually objects have two types of methods: commands and queries. Commands
change something and return nothing, queries in opposite return current
state without changing anything. This separation brings clarity in how and when
you use particular method, so you don't endup in situation when asking object
for it's state also changes it or sends email.

## Transparent about input/output
In other words you should always be able to inspect input parameters of the
service, which are usually got supplied through constructor, hense you should
have attribute readers for every input parameter.

## Check it's own input
It's simple, do not check email address for being nil outside of service. This
way those checks won't spread through your codebase every time you use the
service. I usually throw exception from constructor.

## Tested
According to Sandi Metz, you should test object query methods by asserting
returned result, command methods - by asserting direct public side effect and
outgoing command methods, by expecting to send them. Usually, I would test such
service as so:

{% highlight ruby %}
describe UserCreator do
  describe "#create_user" do
    it "creates user" do
      user_creator = UserCreator.new("qwe@qwe.qwe", "Ian", "Brown")
      user_creator.create_user

      expect(user_creator.user_created?).to eq(true)
    end

    context "no email given" do
      it "raises exception" do
        expect {
          UserCreator.new(nil, "Ian", "Brown")
        }.to raise_error(/No email given/)
      end
    end
  end

end
{% endhighlight %}

