---
layout: post
title: "Ruby on Rails Conventions over Configuration"
modified:
categories: ruby
description:
tags: [ruby on rails]
image:
  feature:
  credit:
  creditlink:
comments:
share:
date: 2015-02-12T09:00:24+11:00
---
###Rails follows conventions over configurations

There is some programming philosophy already, but rails brings another one to them.

* DRY - Don't Repeat Yourself
* YAGNI - You ain't gonna need it
* Convention over Configuration (from rails)

To make a program as clear as possible, so that it is not necessary to explain it. Otherwise, give a short well-explained comment over every obscure piece of code as "How to" comments.


{% highlight ruby %}
# Verify fiven message with accepted signature
# Using RSA 256 and initialised key pair
# Probably will work for RSA 1024 and RSA 2048
def verify_message(msg, signarture)
...
end
{% endhighlight %}

###Camels for Class, Snakes Everywhere Else
* "Snake case": lowercase_word_separated_by_underscore
* "Camel case": ClassName
* Constants: ALL_UPPERCASE = true

###Parentheses(Optional)
{% highlight ruby %}
def find_customer(id, name)
  #Body
end

find_customer(3124585, 'Cohen')

#or

def find_customer id, name
  #Body
end

find_customer 3124585, 'Cohen'
{% endhighlight %}

I personally prefer the second one, because the first one brings some Java style mixed in the ruby style code. :)

{% highlight ruby %}
#If methods don't have arguments, leave it blank after the method name.

def split_message
  @msg.split if @msg.size > 140
end
{% endhighlight %}

###Folding up Lines & Blocks
{% highlight ruby %}
# Lines

# Don't do this
puts @user.name; puts @account
# Unless
Class Report < BaseModel; end
def method_to_be_overriden; end

# Blocks
10.times {|n| puts n}

10.times do |n|
  puts "The number is #{n}"
  puts "The power of 2 is #{n*n}"
end
{% endhighlight %}

### Conditional Statements
{% highlight ruby %}
# if vs unless
report.user = current_user unless !current_user.nil?

# Slightly more verbose than it needs
if not @read_only
  message.title = new_title
end

# More Idiomatic way
unless @read_only
  messge.title = new_title
end

# while vs until
# Avoid negated conditions
while !@printer.ready?
  #Body
end

#More idiomatic way
until @print.ready?
  #Body
end
{% endhighlight %}

### Use Modifier Forms
{% highlight ruby %}
message.title = new_title unless @read_only

# change
if request.xhr?
  render :index, layout: false
else
  render :index
end

# to
render :index, layout: !request.xhr?
{% endhighlight %}

### Loops: Use each instead for
{% highlight ruby %}
colors = ['green', 'red', 'blue']

# don't do this
for color in colors
  puts color
end

# use each
colors.each do |color|
  puts color
end
{% endhighlight %}

### More code in ruby style
{% highlight ruby %}
@user.name = '' unless @user.name.nil?

# The ruby way
@user.name ||= ''

# Never do this
some_condition.nil? ? handle_true_condition : handle_false_condition
{% endhighlight %}

### Use Symbols to Stand for Something (it also save memory both hardware and our brain)
{% highlight ruby %}
[23] pry(main) > Foo.methods.grep(/^c/)
=>[
:constants,
:const_get,
:const_set,
:const_defiled?
:const_missing,
:class_variables,
:class_variable_get,
:class_varibale_set,
:class_varibale_defined?,
:class_exec,
:class_eval,
:class,
:clone
]
{% endhighlight %}

### Readable Code
{% highlight ruby linenos %}
# Composing Methods for Humans

class TextComressor
  attr_reader :unique, :index

  def initialize(text)
    @unique = []
    @index = []

    words = text.split
    words.each do |word|
      i = @unique.index(word)
      if i
        @index << i
      else
        @unique << word
        @index << @unique.size - 1
      end
    end
  end
end

# usage
text = "This specification is the spec for a specification"
compressor = TextCompressor.new(text)
unique_word_array = compressor.unique
word_index = compressor.index

class TextCompressor
  attr_reader :unique, :index

  def initialize(text)
    @unique = []
    @index = []

    words = text.split
    words.each do |word|
      i = @unique.index(word)
      if i
        @index << i
      else
        @index << add_unique_word(word)
      end
    end
  end

  def unique_index_of(word)
    @unique.index(word)
  end

  def add_unique_word(word)
    @unique << word
    @unique.size - 1
  end
end

# Make a code a little more articulate
class TextCompressor
  attr_reader :unique, :index

  def initialize(text)
    @unique = []
    @index = []

    words = text.split
    words.each do |word|
      i = @unique.index(word)
      if i
        @index << 1
      else
        @index << add_unique_word(word)
      end
    end
  end

  def unique_index_of(word)
    @unique.index(word)
  end

  def add_unique_word(word)
    @unique << word
    @unique.size - 1
  end
end

# Readable Code
class TextCompressor
  attr_reader :unique, :index

  def initialize(text)
    @unique = []
    @index = []

    add_text(text)
  end

  def add_text(text)
    words = text.split
    words.each { |word| add_word(word)}
  end

  def add_word(word)
    i = unique_index_of(word) || add_unique_word(word)
    @index << i
  end

  def unique_index_of(word)
    @unique.index(word)
  end

  def add_unique_word(word)
    @unique << word
    @unique.size - 1
  end
end

# Readable code makes your classes easier to test
describe TextCompressor do

  it "should be able to add some text" do
    c = TextCompressor.new('')
    c.add_text('first second')
    c.unique.should == ['first', 'second']
    c.index.should == [0, 1]
  end

  it "should be able to add a word" do
    c = TextCompressor.new('')
    c.add_text('first')
    c.unique.should == ['first']
    c.index.should == [0, 1]
  end

  it "should be able to find the index of a word" do
    c = TextCompressor.new('hello world')
    c.unique_index_of('hello').should == 0
    c.unique_index_of('word').should == 1
  end

  #Other testing code
end
{% endhighlight %}

### Git
the **diff** says **what you did**;
your commit message should tell me **why you did this**

### Conclusion
Good code is like a good joke: It needs no explanation.

---

###Reference:
1. [David Paluy: Ruby On Rails coding conventions, standards and best practices](http://www.slideshare.net/davidpaluy/ruby-on-rails-coding-conventions-standards-and-best-practices)
2. [RailsGuides](http://guides.rubyonrails.org/active_record_basics.html)
3. [Taryn East: acts_as_good_style](http://rubyglasses.blogspot.com.au/2007/08/actsasgoodstyle.html)
