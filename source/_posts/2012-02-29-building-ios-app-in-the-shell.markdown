---
layout: post
title: "Building iOS Apps in the Shell"
date: 2012-02-29 11:53
comments: true
categories: iOS, ruby, bash
author: Franklin Webber
---

The current state of iOS command-line tools for iOS is abysmal especially if you
are familiar and comfortable with a particular feedback loop for building,
testing, and deploying. It is code savagery! Plain and simple!

## Problem

I wanted the ability to easily build Xcode targets from bash.

## Narrative

As an iOS developer, if you use any testing library requiring an additional,
independent target you find yourself within Xcode re-selecting targets and
platforms during development to validate functionality. Xcode's programming is
predominately Mouse-Driven Development (MDD) and great for initial discovery but
that begins to break down as you start to identify multi-target operations you
want to perform quickly. This operation of switching targets/platforms is a
small cost if you write a lot of test or application code up front. This fails
when you want to test drive your development as this small operational cost adds
up as you begin to refine your feedback loop.

A project Substantial inherited came with a rudimentary `Rakefile` with a number
of tasks and helper methods defined to assist with building and testing an iOS
application from the command-line. It was awesome! As long as I did not block
the IOS simulator I could write code and tests and get more immediate feedback.

Sadly, it was largely wrong: composed of a number of variables and hard-coded
values that were based on assumptions of how the project was built. We hit our
first major problem upgrading all the developers from Xcode 3.x to Xcode 4.x.
All of our command-line tools and files broke and that quick feedback loop we
became accustomed to broke down. Because of it our team and others delivered
functionality that broke several tests that we later, out of context, struggled
to resolve.

With the start of our next project I immediately ported over the
[Rakefile](https://gist.github.com/1953158) and took the opportunity to clean it
up. In a small amount of time I created some code that I was reasonably happy
with but again worried about having to drag a gist of code from project to
project. I thought about future me and I wanted to ensure I did not have to
remember any of this every again unless I absolutely needed.

## Solution

It was then I went on the search for software to save me. It was then that I
stumbled upon [Rayh](http://rayh.com.au/)'s
[xcoder](https://github.com/rayh/xcoder). I was able to throw away 200+ lines of
Rakefile to more elegantly write the following:

``` ruby Rakefile
require 'xcoder'

task :default => :specs

task :specs do
  Xcode.project('TestProject').target('Specs').config('Debug').builder.build
end
```

Gorgeous! And more importantly exceedingly more portable when I want to get
started on the right foot. To make this ever more portable I recently added
default rake tasks to the [xcoder](https://github.com/rayh/xcoder) gem that adds
all your Xcode projects in your default directory available through rake.

``` ruby Rakefile
require 'xcoder/rake_task'
Xcode::RakeTask.new
```

Which will generate several common tasks for each of your projects > targets >
configurations

``` bash rake output
rake xcode:test_project:specs:debug:build                   # Build TestProject Specs Debug
rake xcode:test_project:specs:debug:clean                   # Clean TestProject Specs Debug
rake xcode:test_project:specs:debug:package                 # Package TestProject Specs Debug
rake xcode:test_project:specs:debug:test                    # Test TestProject Specs Debug
rake xcode:test_project:specs:release:build                 # Build TestProject Specs Release
rake xcode:test_project:specs:release:clean                 # Clean TestProject Specs Release
rake xcode:test_project:specs:release:package               # Package TestProject Specs Release
rake xcode:test_project:specs:release:test                  # Test TestProject Specs Release
```

Now the `Rakefile` looks something like:

``` ruby Rakefile
require 'xcoder/rake_task'
Xcode::RakeTask.new

task :default => "xcode:text_project:specs:debug:build"
```

The benefit is that [Xcoder]((https://github.com/rayh/xcoder) is leveraging the
project file to generate the correct `xcodebuild` operation allowing your build
to remain in lock-step with the Xcode environment as you build and test your
product in a headless environment.
