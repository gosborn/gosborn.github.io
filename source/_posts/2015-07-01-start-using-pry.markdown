---
layout: post
title: "Start using Pry!"
date: 2015-07-01 19:40:56 -0400
comments: true
categories: flatiron, school
---

##Tips for using Pry

![pry_image](https://camo.githubusercontent.com/c26ae0f28a595b15dc4d135fdcc113366f811853/68747470733a2f2f646c2e64726f70626f782e636f6d2f752f32363532313837352f70727925323073747566662f6c6f676f2f7072795f6c6f676f5f3335302e706e67)

In the past, I've used REPLs like IRB or [repl.it](http://repl.it) to play around with Ruby. Pry adds on tons of functionality and has been my go-to REPL ever since diving into the documentation (on a pretty shallow level, I'll admit). It takes a little bit of getting used to, but just liking learning a manual transmission, it's tough to go back once you get the hang of it.

Below, I'll describe some of my favorite features and some resources for further reading.


##Pry Features

Pry adds in many features to the REPL environment.

- You can set **breakpoints** in code with runtime invocation.
  - I won't discuss this as most people are introduced to Pry via this utility to debug code.
- You can get **help** on the fly
  - **Pry-docs** and source code browsing will eliminate some trips to Google and the Ruby documentation: docs are provided in Pry
  - Tools are provided to **trace errors**
- You can easily travel around your code
  - Pry provides tools (not unlike your BASH console) that allow you to **travel in and out of environments** to view local variables and methods
  - Pry also provides tools that **interact with BASH**
  - Pry also allows you to **travel through time** by replaying your history
- Many **plugins** interact with Pry, allowing additional functionality




###Moving in and out of environments, in Ruby and BASH

In pry, cd and ls act in a way similar to the cd and ls commands in BASH. cd allows you to change environments. Below, I used cd to move into the environment of my array. The console tells us this by changing the prompt to include #<Array>.

```
// ♥ pry
[1] pry(main)> arr = Array.new
=> []
[2] pry(main)> cd arr
[3] pry(#<Array>):1> 

```

Now that I'm in an array, I can use ls to see what is available to me.

```
4] pry(#<Array>):1> ls
Enumerable#methods: 
  all?            each_slice        flat_map  max_by     none?         slice_when
  chunk           each_with_index   grep      member?    one?          sort_by   
  collect_concat  each_with_object  group_by  min        partition   
  detect          entries           inject    min_by     reduce      
  each_cons       find              lazy      minmax     slice_after 
  each_entry      find_all          max       minmax_by  slice_before
Columnize#methods: columnize  columnize_opts  columnize_opts=
Array#methods: 
  &         combination  fill        map!                  reverse!      sort!     
  *         compact      find_index  pack                  reverse_each  sort_by!  
  +         compact!     first       permutation           rindex        take      
  -         concat       flatten     place                 rotate        take_while
  <<        count        flatten!    pop                   rotate!       to_a      
  <=>       cycle        frozen?     pretty_print          sample        to_ary    
  ==        delete       hash        pretty_print_cycle    select        to_h      
  []        delete_at    include?    product               select!       to_s      
  []=       delete_if    index       push                  shelljoin     transpose 
  any?      drop         insert      rassoc                shift         uniq      
  assoc     drop_while   inspect     reject                shuffle       uniq!     
  at        each         join        reject!               shuffle!      unshift   
  bsearch   each_index   keep_if     repeated_combination  size          values_at 
  clear     empty?       last        repeated_permutation  slice         zip       
  collect   eql?         length      replace               slice!        |         
  collect!  fetch        map         reverse               sort        
self.methods: __pry__
locals: _  __  _dir_  _ex_  _file_  _in_  _out_  _pry_
```

ls will show you which methods, constants and variables are accessible to that environment. Use: 

- ls -m; for local methods
- ls -l; for local variables
- ls -c; for constants
- ls -G to search for paramters by name (or partial name)

With Pry-docs installed (gem install pry-docs), you can get info on how any method is used. While still in the array environment: 

```
[13] pry(#<Array>):1> ? self.map

From: array.c (C Method):
Owner: Array
Visibility: public
Signature: map()
Number of lines: 12

Invokes the given block once for each element of self.

Creates a new array containing the values returned by the block.

See also Enumerable#collect.

If no block is given, an Enumerator is returned instead.

   a = [ "a", "b", "c", "d" ]
   a.collect { |x| x + "!" }        #=> ["a!", "b!", "c!", "d!"]
   a.map.with_index{ |x, i| x * i } #=> ["", "b", "cc", "ddd"]
   a                                #=> ["a", "b", "c", "d"]
```

Using ? (show-doc is an alias) and .map (using self, an array in this case, as the receiver) returns the Ruby Documentation in the console! No more searching through Google.

Just like in BASH, cd .. goes up an environment, towards main.

###Examples with a method

Below, I've created a Dog class, which will return "wooooof" when bark is called on an instance of the class. 

show-source allows the user to look back on the code contained within Dog, very helpful while debugging a larger file, especially while using binding.pry. 

Also, observe how one can cd into and instance of the Dog classto see and call its methods.

```
[17] pry(main)> class Dog
[17] pry(main)*   def bark
[17] pry(main)*     "woooooof"
[17] pry(main)*   end  
[17] pry(main)* end  
=> :bark
[18] pry(main)> doggie = Dog.new
=> #<Dog:0x007fddff9fadc8>
[19] pry(main)> show-source Dog

From: (pry) @ line 6:
Class name: Dog
Number of lines: 5

class Dog
  def bark
    "woooooof"
  end
end
[20] pry(main)> cd doggie
[21] pry(#<Dog>):1> bark
=> "woooooof"
[22] pry(#<Dog>):1> ls
Dog#methods: bark
self.methods: __pry__
locals: _  __  _dir_  _ex_  _file_  _in_  _out_  _pry_
```
###BASH manipulation

Pry can also interact with BASH. Adding . before any BASH command while running pry will allow you to send those commands to BASH. You can switch directories, list content, even cowsay, all easily through Pry.

```
[36] pry(#<Dog>):1> .cd dev
[37] pry(#<Dog>):1> .pwd
/Users/geo/dev
[38] pry(#<Dog>):1> .cowsay take me home
 ______________ 
< take me home >
 -------------- 
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
[39] pry(#<Dog>):1> .cd ~
[40] pry(#<Dog>):1> .pwd
/Users/geo
[41] pry(#<Dog>):1> 
```


###Tracing errors in code

Sometimes errors can be a little hard to trace. Here's a convoluted example where the error trickles down through multiple methods.

```
[10] pry(main)> def gonna_make_error(name)
[10] pry(main)*   name.mistake
[10] pry(main)* end  
=> :gonna_make_error
[11] pry(main)> def second_layer_error(name)
[11] pry(main)*   gonna_make_error(name)
[11] pry(main)* end  
=> :second_layer_error
[12] pry(main)> def third_layer_errof(name)
[12] pry(main)*   second_layer_error(name)
[12] pry(main)* end  
=> :third_layer_errof
[13] pry(main)> third_layer_errof("greg")
NoMethodError: undefined method `mistake' for "greg":String
from (pry):10:in `gonna_make_error'
```
Pry provides two cool tools: wtf? and cat --ex, to help identify the cause of errors in your code.

wtf? allows you to display a few lines of backtrace for your most recent error.  Not too much different from receiving an error in ruby, but it proves developers have a sense of humor.

```
[14] pry(main)> wtf?
Exception: NoMethodError: undefined method `mistake' for "greg":String
--
0: (pry):10:in `gonna_make_error'
1: (pry):13:in `second_layer_error'
2: (pry):16:in `third_layer_errof'
3: (pry):18:in `__pry__'
4: /Users/geo/.rvm/gems/ruby-2.2.1/gems/pry-0.10.1/lib/pry/pry_instance.rb:355:in `eval'
5: /Users/geo/.rvm/gems/ruby-2.2.1/gems/pry-0.10.1/lib/pry/pry_instance.rb:355:in `evaluate_ruby'
6: /Users/geo/.rvm/gems/ruby-2.2.1/gems/pry-0.10.1/lib/pry/pry_instance.rb:323:in `handle_line'
7: /Users/geo/.rvm/gems/ruby-2.2.1/gems/pry-0.10.1/lib/pry/pry_instance.rb:243:in `block (2 levels) in eval'
8: /Users/geo/.rvm/gems/ruby-2.2.1/gems/pry-0.10.1/lib/pry/pry_instance.rb:242:in `catch'
9: /Users/geo/.rvm/gems/ruby-2.2.1/gems/pry-0.10.1/lib/pry/pry_instance.rb:242:in `block in eval'
```

cat --ex allows you to see the actual code that caused the error, which can be really helpful for debugging and learning how to code, especially when navigating errors in sinatra.

```
[15] pry(main)> cat --ex

Exception: NoMethodError: undefined method `mistake' for "greg":String
--
From: (pry) @ line 10 @ level: 0 of backtrace (of 32).

     5: def more_error(name)
     6:   error(name)
     7: end
     8: more_error("greg")
     9: def gonna_make_error(name)
 => 10:   name.mistake
    11: end
    12: def second_layer_error(name)
    13:   gonna_make_error(name)
    14: end
    15: def third_layer_errof(name)
[16] pry(main)> 
```
Additionally, hist allows you to see a history of all the commands you've typed into Pry, to get more information as to how your code has broken.


###Further Resources

Pry provides a ton more functionality, so I really suggest spending a little time reading the below resources.

[Pry Home Page](http://pryrepl.org/)  
[Pry on Git](https://github.com/pry/pry)  
[Pry Plugins](https://github.com/pry/pry/wiki/Available-plugins)  
[PryBaby](http://www.danvisintainer.com/?p=5)
