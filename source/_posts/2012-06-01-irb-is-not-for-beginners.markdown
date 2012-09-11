---
layout: post
title: "IRB Is Not For Beginners"
date: 2012-06-01 16:13
comments: true
categories: ruby, teaching, IRB
---

Several introductory tutorials use IRB as a tool to show off Ruby to new
developers. I can understand the allure of using IRB:

> * does not require the user to create a file
> 
> * immediate feedback for their actions

In my experience, using IRB is fraught with far too much peril to make it a
valuable learning tool.

> * unable to visualize all the code that they have written
> 
> * navigation in IRB is much harder than in an editor
>
> * state of the code is lost when exiting IRB

## does not require the user to create a file

It is a major boon that the user does not have to create a file to get started.
However, it becomes a major pain if the example starts to span multiple lines
or requires particular step-by-step code to be completed.

Given that a user will want to learn by re-typing all the code they see in an
example, they are likely to make a mistake during the data entry process and
because of the poor, console like, interaction provided to the user they will
often be unable to use any of the code that follows the mistake.

From a troubleshooting point of view, it is extremely difficult to have the user
move back through their command history (Up-Arrow) multiple times to find the 
issue.

Eventually your examples will have to exist in a file and while it seems like
a headache from the start to have the user create a file for them to copy the
example or write their code in, it is simply where they are going to end up.

> Examples longer than a single line should be written in a file (not IRB).

## immediate feedback for their actions

I do rather enjoy this feature of IRB over using a text file. When a user
finishes a line and presses the *enter* key, they are immediately treated with
feedback to the code that they wrote. It's like chatting with *Rainman*; able
to repeat what you tell it and solve complex math problems quickly.

It is great when it works. It is a great failure when it does not work.

The great failure I am talking about is the first time in an example you ask
a new developer to perform any operation that has an opening element and a
closing element.

```
001 > puts "Hello World'
002"> _
```

Instead of getting immediate feedback, the user is treated with what appears to
them as a broken console. They are not even able to exit! Everything they start
type in at this point stops working as it once did. All because they opened a
string with a `"` and closed it with a `'`.

This only gets worse when they start to use more of these required starting
and ending elements in Arrays, Hashes, Methods, and Blocks.

> Limit your IRB examples that use starting-ending elements
