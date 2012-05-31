---
layout: post
title: "Getting More Out Of Our Documentation (By Putting More In It)"
date: 2012-05-30 00:03
comments: true
categories: testing, documentation
---

I came to Ruby by way of Cucumber. Cucumber placed an emphasis on writing tests
that described the products behavior. Ideally Product Owners at the helm,
authoring your requirements. No good tools were made available to provide
documentation for the stakeholders. Out of necessity, I created an extension for
YARD. I started to work on another extension for RSpec. I even started playing
with integrating Bundler. Through this experimentation I realized how little our
documentation tells us as stakeholders, testers, and developers.

Tests are documentation; but they are not in our documentaiton.

So I would like to help champion the continual war to encourage documentation by
building tools that make better documentation from what has already been
written. In that spirit I built an
[extension](https://github.com/burtlo/yard-docco) that generates better
documentation with the comments made within methods themselves: annotated source
code (docco style).