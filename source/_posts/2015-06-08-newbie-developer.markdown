---
layout: post
title: "Newbie Developer, Ex-Scientist?"
date: 2015-06-08 19:34:19 -0400
comments: true
categories: 
---


##From biology to coding

I've traded in my microscopes for a keyboard. After a number of years pursuing a career in cellular biology, I've put down the pipettes and decided to really, honestly learn how to write code.

This blog will be about the struggle to learn new computer languages, find out how to use them and life lessons I learn along the way. I won't frequently comment on leaving [academia][1], but my perspective, learning habits and style have all been shaped by that environment.

Jumping out of academia is quite frightening. That universal, cellular process you were an expert on? That's suddenly just as esoteric as astrobiology to most people. Chatting about cell polarity to non-biologists has induced a panic-induced desire to run the nearest research university. Really, what am I left with?  

It turns out that studying signal tranduction, gene regulation and mutant phenotypes isn't terribly different from learning how to write, manipulate and refactor code. Evolution has provided our cells with (typically) elegant mechanisms for processing information, communicating data between simple machines and providing feedback on actions. Elegant code (typically) isn't that different. While learning a new coding language is a struggle, it is fun for a biologist to tinker around with: you can think of it like a simple organism.

###Code: not unlike a biological system

####Looping

A major principle in gene regulation is the logic behind feedback loops. Understanding how systems respond to, and attenuate, both transient and continuous information is a huge field of research. Below is a cartoon (from Paul Hardin's lab) of a feedback loop in circadian rhythm biology, containing both positive and negative information.

![feedback](http://www.bio.tamu.edu/USERS/phardin/images/Fig3-InterlockedFBLs.jpg)

Pretty complicated, but if you've ever studied this sort of regulation, or sat through a seminar, you know it's not impossible to sort out.  

Code, for some reason, is viewed as magic, or a black box, but it's simpler to figure out than the PERIOD and CLOCK mess above. 

``` 
def countdown(seconds)  
  while seconds > 0  
    puts "#{seconds} SECOND(S)!"  
    seconds -= 1  
  end  
  "0 left. HAPPY NEW YEAR!"  
end

```

The above Ruby code takes in an argument "seconds" as a number and counts down in a stepwise fashion. Once it hits zero, it's a happy new year. Code can be simple, understandable, and nowhere near as complex as circadian rhythms.



####The single responsbility principle

In code, one method should do one thing only. This is called the "Single Repsonbility Principle". Programs frequently have a method, called a runner, that runs all the methods. While genes are frequently pleiotropic, they usually act in concert with others to manage cellular activity. What typically unites genes of similar ontologies? Master regulators, which aren't all that unlike runner methods.  

Evolution usually favors simplicity. Code does too.

####Breaking it: the phenotypes

I love to break code. I get the feeling that many of my classmates at the Flatiron School do not feel the same way. Granted, most of the population does not enjoy seeing error messages like the one below.

```
countdown.rb:18: syntax error, unexpected end-of-input, expecting keyword_end

```  
Biologists sometimes spend years trying to introduce mutations into the genomes of model organisms, thereby breaking their systems. The field of biology is principally one that studies what happens when small wrenches are thrown into the inner workings of life. To get to play with systems so quickly, to see what is necessary for structure, what is superfluous or vestigal, and what surprisingly works is really fun. You can get errors back in milliseconds vs years.

###Computer vs. the lab bench

If you think about systems analytically, biological systems aren't too dissimilar from those written by programmers. The analytically skills biologists acquire can be helpful in understanding all of this. As I continue to learn how to develop, I hope to explore, uncover and share some more similarities. 

[1]: http://www.howtoleaveacademia.com/ "This is a great resource" 

