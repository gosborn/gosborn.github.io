---
layout: post
title: "The .zip method"
date: 2015-06-18 19:48:54 -0400
comments: true
categories: Flatiron School
---


->![zips](http://www.cayzu.com/wp-content/uploads/zipper-1030x360.jpg "zipper")<-

Uggh, the time I've wasted trying to google this method.

*"ruby how to add two arrays into one array and have order switch?"*   

*"How to combine two arrays in an interwoven fashion? ruby"*  

*"how to shuffle two arrays in order and get one back ruby"*  

*etc. etc.*  

Once you remember its name, you have a pretty good idea of what .zip does. But personally, it's tough for me to keep the name in working memory, perhaps because .zip lives all the way at the bottom of the [Ruby Ennumerable documentation](http://ruby-doc.org/core-2.2.2/Enumerable.html).  

Maybe .zip is tough to google because it's so much easier to show its functionality than describe it with words.

```ruby
array_1 = ["a", "b", "c"]
array_2 = [1, 2, 3]
array_1.zip(array_2) #=> [["a", 1], ["b", 2], [c, "3"]]
```

Calling .zip on an array, while passing in another array as an argument, results in a new array composed of:

 **[[array_1[0], array_2[0]],  [array_1[1], array_2[1]],  [array_1[2], array_2_[2]]**

The two arrays are now interwoven or shuffled or mixed in order, in a nested array...I guess zippered is the best terminology here. Even the Ruby documentation is a little vague:

>Takes one element from enum and merges corresponding elements from each args. This generates a sequence of n-element arrays, where n is one more than the count of arguments.  

I find myself using .zip from time to time, so its important to remember a couple of things.  

* **It returns a nested array.** If 2D isn't your thing, .flatten.  
* **It returns a nested array with elements equal to the length of the first array.**  
    * If your second array is longer than the first, those extra values will not be included in the output.   
    * But if it's the other way around, your second array is shorter, you'll get nil values to make up the difference. Example:
```ruby
a = [1,2,3]  
b = [101,102,103,104]  
a.zip(b) #=> [[1, 101], [2, 102], [3, 103]] 
b.zip(a) #=> [[101, 1], [102, 2], [103, 3], [104, nil]] 
```
* **The .zip function can work on more than two arrays:**

```ruby
a = [1,2,3,4]  
b = [10,11,12,13]
c = ["a", "b", "c", "d"] 
a.zip(b,c) #=> [[1, 10, "a"], [2, 11, "b"], [3, 12, "c"], [4, 13, "d"]] 
```

* **.zip can be used to easily create a hash** if your keys and values are in matching arrays:

```ruby
key_array = ["key1", "key2", "key3"]
value_array = ["value1", "value2", "value3"]
Hash[key_array.zip(value_array)] #=> {"key1"=>"value1", "key2"=>"value2", "key3"=>"value3"} 

```

* **Also, .zip does take a block, but return an unconditional nil.** There was [talk](https://bugs.ruby-lang.org/issues/5044) of changing this, or adding a .zip_with feature, to make use of this function a few years back, but was dropped and forgotten about. Unless you are looking to puts to the console, I can't think of a valid way to use this feature.

Hopefully, this post will increase .zip awareness: a very useful function, obviously to honor the mighty Akron Zips.

->![zips](http://assets.sbnation.com/assets/163084/AkronZipsLogo.jpg "Akron Zips Logo")<-





