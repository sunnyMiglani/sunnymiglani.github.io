---
layout: default
title: Today I Learned
---

This page is meant to capture a few small things I learn either at work or out of it.
It's not a pretty page right now, but hopefully I'll learn what it takes to make it better!

Note: There are probably a few obvious things in here. But to that I say - https://xkcd.com/1053/


# Things I Learned


## January

11th Jan, 2021:

How exceptions work in Stream operations
<p>
If you've got a Java (1.8 ofc) "Stream" object that's being iterated upon via the `forEach` calls,
an exception in the loop will end the execution for all following iterations rather than just one iteration. 
The preferred way handling an error case in this scenario is using `return` after logging.

This is because `continue` would be invalid as each forEach in the background is a map of a function (method) over items in the Stream. 
Continue in a method makes no sense! 
</p>

