---
layout: post
title: "Working with errors in Ruby: begin, rescue and ensure"
date: 2015-07-17 08:55:08 -0400
comments: true
categories: 
---

Seeing Ruby throw an exception when you don't expect it can be pretty frusturating. At this point in my programming career, most of them are user created. Ruby raises pretty specific exceptions and even a backtrace to figure out where you went wrong. Usually my protocol is to do find the error, figure out where my logic or typing went wrong and restart my program.   

But sometimes exceptions arise in your program and they're not your fault. Lately, I've been working on a database of beer that is populated with information from an online database, [breweryDB](http://www.brewerydb.com/). To gather the info, I'm getting JSON files from their API with a method like this:

```
def load_db_json(input)
  JSON.load(RestClient.get("http://api.brewerydb.com/v2/" + input)
end

```

Specifically, I'm downloading JSON files that provide me ids to find all the beers from a group of breweries. 

```
def all_beers_from_brewery(brewery)
  load_db_json("brewery/#{brewery.db_code}/beers?key=#{@key}")["data"]
end

@state.breweries.each do |brewery|
  if !all_beers_from_brewery(brewery).nil?
    all_beers_from_brewery(brewery).each do |hash|
      brewery.beers.build(name: hash["name"], db_code: hash["id"], abv: hash["abv"], ibu: hash["ibu"], description: hash["description"])
      brewery.save
    end
  end
end

```

The above code uses the brewery ids to find the beers associated with it, and build ActiveRecord associated objects. But it doesn't always work.

Sometimes the breweries will be missing from the database and so when I try to get a json from a missing page, I get a fun exception:  

RestClient::ResourceNotFound: 404 Resource Not Found  

And my program stops. Since I'm iterating through a long list of breweries, this is super annoying, because no subsequent data will be entered into my database. This is where Ruby's catch, throw and ensure methods come in.

```
begin
  a + a
rescue => e
  puts e
ensure
  puts "we'll be fine, regardless of what happens"
end
undefined local variable or method `a' for main:Object
we'll be fine, regardless of what happens

```

If this code was just written as below, it would break, because a isn't defined.

```
a + a
```

Using begin, rescue and ensure allows you to negotiate the error and keep moving. It's a little hacky to solve ordinary problems, but when working with large datasets that might fail because of errors in the members of the dataset, it can be super helpful.

The above isn't super different than an if/else statement, except that the error is handled and the code continues. Here's how I use it in real life:

```
def all_beers_from_brewery(brewery)
  begin
    load_db_json("brewery/#{brewery.db_code}/beers?key=#{@key}")["data"]
  rescue => error
    retry
    puts "#{error} for #{brewery.name}"
  end
end

```

Now if I get an error iterating through my database with the below code, I don't break the operation, I just get an error message and move on with the parsing:

```
@state.breweries.each do |brewery|
    all_beers_from_brewery(brewery).each do |hash|
      brewery.beers.build(name: hash["name"], db_code: hash["id"], abv: hash["abv"], ibu: hash["ibu"], description: hash["description"])
      brewery.save
    end
end

```

Javascript and Python have very similar operations. Javascript uses "try, throw and catch, finally" and Python uses "try, except and finally". 

Here are some resources to learn more:

[Ruby Bastards: exception handling](http://ruby.bastardsbook.com/chapters/exception-handling/)  
[Ruby Learning: exceptions](http://rubylearning.com/satishtalim/ruby_exceptions.html)  
[Weird Ruby, rescue and interrupt](https://blog.newrelic.com/2014/12/10/weird-ruby-2-rescue-interrupt-ensure/)  
[Ruby Monk: handling exceptions](https://rubymonk.com/learning/books/4-ruby-primer-ascent/chapters/41-exceptions/lessons/92-handling)  
[Exception handling in JS and Python](http://www.jiaaro.com/pythonjavascript-trick-hacky-error-handling/)  

