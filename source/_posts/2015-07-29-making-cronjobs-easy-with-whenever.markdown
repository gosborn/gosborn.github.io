---
layout: post
title: "Making cron jobs easy with whenever"
date: 2015-07-29 20:37:20 -0400
comments: true
categories: flatiron school
---

##Cron

Scheduling tasks to happen at regular intervals is built into Unix systems with the [cron](https://en.wikipedia.org/wiki/Cron) daemon. Cron runs as a background process to execute scheduled commands or shell scripts at fixed intervals or dates.  

Utilizing cron jobs to automate tasks in Rails is easy with the [whenever](https://github.com/javan/whenever/tree/master) gem.

##Using whenever

First, download the gem:

```
$ gem install whenever
```

Add it to your Gemfile:

```ruby
gem 'whenever', require: false
```

And bundle:

```
$ bundle
```

To start using whenever, run 'wheneverize' in the project directory. This will add a schedule.rb file to your Rails project.

```
$ wheneverize .
```

The schedule.rb file allows you to define tasks in Ruby syntax that the whenever gem will add to your crontab to be automated by your local system.  

To play around with tasks with whenever, we'll add:

```ruby
set :environment, "development"
```

to the top of schedule.rb, so that our ruby tasks will run in the development environment (the default is set to run them in production only).  

The [whenever](https://github.com/javan/whenever/tree/master) documentation on Github contains great examples of what can be written in schedule.rb. Ruby code, scripts and even shell commands can be used by the whenever gem to populate crontab with cronjobs.

We'll start with running a simple shell command to understand how to interface with the whatever gem and cronjobs. 

In schedule.rb, add:

```ruby
every 1.minute do
  command "echo 'hello from rails!!!'"
end
```

When translated into a cron job, this will send you the message "Hello from rails!!!" once a minute (it won't be displayed in your console, it'll be sent to you as mail, so don't worry, it won't be too annoying).

Back at the command line, in the directory of your rails project, type:

```
$ whenever
```

This should display output like this, translating your ruby into cron syntax. The [cron](https://en.wikipedia.org/wiki/Cron) page on wikipedia has a great explanation of this.

```
$ whenever
* * * * * /bin/bash -l -c 'echo '\''hello from rails!!!'

## [message] Above is your schedule file converted to cron syntax; your crontab file was not updated.
## [message] Run `whenever --help' for more options.
```

So now whenever knows about the tasks you want to become cron jobs. To place these into crontab, where they will be executed on a schedule, type:

```
$ whenever -w
```

This modifies crontab and writes your tasks. Our above task should start running! Wait about a minute...  

Maybe now is a good time to run:

```
$ crontab -l
# Begin Whenever generated tasks for: /Users/geo/dev/pubfeed/config/schedule.rb
* * * * * /bin/bash -l -c 'echo '\''hello from rails!!!'\'''

# End Whenever generated tasks for: /Users/geo/dev/pubfeed/config/schedule.rb
```

That's cool, crontab did receive our jobs. Also how about:

```
$ whenever --help
```
It's a nice little help message. What version is it?

```
$ whenever -v
Whenever v0.9.4
You have new mail in /var/mail/geo
```

New mail? From where? Run [mailx](http://heirloom.sourceforge.net/mailx/mailx.1.html#1) (the Unix mail processing system) to see what we received:

```
$ mailx
Mail version 8.1 6/6/93.  Type ? for help.
"/var/mail/geo": 36 messages 13 new 36 unread
>N 24 geo@MacBook-Pro.loca  Wed Jul 29 21:26  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 25 geo@MacBook-Pro.loca  Wed Jul 29 21:27  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 26 geo@MacBook-Pro.loca  Wed Jul 29 21:28  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 27 geo@MacBook-Pro.loca  Wed Jul 29 21:29  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 28 geo@MacBook-Pro.loca  Wed Jul 29 21:30  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 29 geo@MacBook-Pro.loca  Wed Jul 29 21:31  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 30 geo@MacBook-Pro.loca  Wed Jul 29 21:32  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 31 geo@MacBook-Pro.loca  Wed Jul 29 21:33  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 32 geo@MacBook-Pro.loca  Wed Jul 29 21:34  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 33 geo@MacBook-Pro.loca  Wed Jul 29 21:35  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 34 geo@MacBook-Pro.loca  Wed Jul 29 21:36  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 35 geo@MacBook-Pro.loca  Wed Jul 29 21:37  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
 N 36 geo@MacBook-Pro.loca  Wed Jul 29 21:38  19/676   "Cron <geo@MacBook-Pro> /bin/bash -l -c "
```

Woah. Where'd all those come from? Cron! Let's open one:

```
? 36
Message 36:
From geo@MacBook-Pro.local  Wed Jul 29 21:38:00 2015
X-Original-To: geo
Delivered-To: geo@MacBook-Pro.local
From: geo@MacBook-Pro.local (Cron Daemon)
To: geo@MacBook-Pro.local
Subject: Cron <geo@MacBook-Pro> /bin/bash -l -c 'echo '\''hello from rails!!!'\'''
X-Cron-Env: <SHELL=/bin/sh>
X-Cron-Env: <PATH=/usr/bin:/bin>
X-Cron-Env: <LOGNAME=geo>
X-Cron-Env: <USER=geo>
X-Cron-Env: <HOME=/Users/geo>
Date: Wed, 29 Jul 2015 21:38:00 -0400 (EDT)

hello from rails!!!

```

It worked! Now let's shut it down. It's simple with whenever.

```
$ whenever -c
```

This will clear whenever-based cron jobs from crontab.

```
$ crontab -l 

```

Crontab is now empty: we won't get any more messages.

That's the whenever workflow. Adding ruby is super easy. Here's an example of code in schedule.rb that manages a database, fetches data from an API and sends email:

```ruby
every 1.day :at => '2:30 am' do
  runner 'EmailAutomator.clean_and_send'
end

```

```ruby
class EmailAutomator

  def self.clean_and_send
    Article.destroy_all
    User.all.each do |user|
      user.keywords.each do |keyword|
        keyword.get_all_recent_abstracts(user)
      end
      UserMailer.daily_result_email(user).deliver_now
    end
  end

end
```

As a note, cron jobs are handled by UNIX, not your Rails application. You don't need a server running for these jobs to complete. 

That's pretty much it. Whenever provides an easy to understand syntax and simple methods for scheduling simple to complex jobs in Ruby applications.




























