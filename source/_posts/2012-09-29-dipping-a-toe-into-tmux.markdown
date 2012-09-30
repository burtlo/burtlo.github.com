---
layout: post
title: "Dipping a toe into TMUX"
date: 2012-09-29 21:45
comments: true
categories: 
---

I started to scroll through [tmux: productive mouse-free development](http://pragprog.com/book/bhtmux/tmux). I thought I would give it a try to help me better manage remote systems by using it locally. And for the most part it has been a pretty awesome tool that yields some results pretty quickly for my investment. So far the only tedious part is the lack of ability to easily scroll back through the history. I can do it, but it is by far not as easy it was simply using Terminal.

Of course, after starting to read the book and starting working with the first few commands I started to hate using **Ctrl+B** as the prefix key. I immediately stopped reading and started to search for an easier way. What follows is how I mapped my **CAPS LOCK** key as **Ctrl+B**.

### Disabling CAPS LOCK

If you haven't already done so, gain an extra key and open up your *System Preferences > Keyboard > Modifier Keys ...* and rebind CAPSLOCK to **No Action**.

### Rebinding CAPSLOCK to CONTROL_R (Right Control Key)

* Install [PCKeyboardHack](http://pqrs.org/macosx/keyremap4macbook/pckeyboardhack.html.en)
* Open *System Preferences > PCKeyboardHack*
* Change "Caps Lock" to keycode `62`.

![Reassigning the Caps Lock](https://img.skitch.com/20120926-m3r8ttqnwu4j5bny7dx81tgp9d.jpg)

### Rebinding CONTROL_R (Right Control Key) to CONTROL + B

* Install [KeyRemap4MacBook](http://pqrs.org/macosx/keyremap4macbook/index.html.en)

* Open *System Preferences > KeyRemap4MackBook > Misc & Uninstall*
* Click "Open private.xml"

![Opening private.xml](https://img.skitch.com/20120926-f31sf7jfsfhunggg7qh9fbrpud.jpg)

* Copy and Paste the private.xml provided with in this gist.
* Open *System Preferences > KeyRemap4MackBook > Change Key*
* Click "ReloadXML"
* Toggle open "TMUX Key Remappings"
* Check "TMUX: Right Control to Control+B

![Enabling Keybinding](https://img.skitch.com/20120926-mp93sudigdsjseuqtfwp7q68w9.jpg)