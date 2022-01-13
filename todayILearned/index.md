---
layout: default
title: Today I Learned
---

This page is meant to capture a few small things I learn either at work or out of it.
It's not a pretty page right now, but hopefully I'll learn what it takes to make it better!

Note: There are probably a few obvious things in here. But to that I say - https://xkcd.com/1053/


# Things I Learned


## January

### 13th Jan, 2021:

Golang is honestly a weird language. It's smart in terms of "ease of use" (looking at you `go fmt`)
But the issue with ease of use is complication when you face a bug but there's only limited things you can do.

Today I spent a few hours trying to get `go test` running.
The issue wasn't being able to run the test itself, but rather inspect the logs in the test.

The logs in go, like many other languages are buffered. Buffered logs are more efficient (usually) so you don't need to
Spend resources (or keep locks) trying to write to system out / syserr. This is a helpful thing in most normal programs.

However, during testing, you'd like to have a semi-live view of the work you're trying to do. This wasn't possible in go until
About 2019, this is because Go's testing framework would buffer all log outputs until the end of the test runs to allow
"Sensible formatting" on the output. This was essentially :

```
------ 
TestName: PASS
-------
SOME
log
Here
------
TestName took X seconds
```
That's nice logging, but I want to see what my test does live!

For this, there's `go test -v`, which will do a streaming print to the sysout allowing you to see the logs directly.
However, `-v` is only linked to the testing library, so you must specifically use the print methods on the testing object passed
Down as a parameter by the testing framework into your test.

That can cause issues if your code isn't currently setup to use the right logging library

But fine, you can now stream your logs! That's great

But can we just talk about ` go test ` for a sec.

`go test` assumes some defaults, e.g

- Your current directory is the test directory (that's fair)
- Your test files end in `_test.go` (This is also fine, I guess. Its weird but sure)
- Your tests themselves, have method names that start with `Test`. This is annoying but doable

By default it'll run all the tests in the given (assumed) package
This package name is extracted from the `go.mod` file in your local directory. I'm not sure what happens if it can't see it.

It also requires your "main" test (yes your test has to have a main) to call `m.run()` where `m` is the 
`Testing.Main` object that's passed in by the framework (wow. I know)

Anyway, once you get past all this, you'll see that running `go run` will actually run your test suite!

Awesome, but what if you wanna run just one test?

That requires 
```
go test run=^TestRunThisTest$ ../myCurrentFolder
```

That just makes me go ["Wat"](https://www.destroyallsoftware.com/talks/wat)

The `run` argument there is actually Regex that is writing the name of the test *case*. 
The `../myCurrentFolder` is the, well name of the current folder. But you need to provide a path
To `go run` and for some reason, `.` doesn't work...

I'm not sure if that's something odd with my test setup, or just golang itself.

But anyway, if you fill all the qualifications golang asks for, sacrifice a lamb or two

Then you might finally get golang's "ease of use"

### 12th Jan, 2021:


You can do PUT requests to Artifactory using cURL doing something like:
```
curl -v -u some_username:"some password" -T someFile.ext "http://Artifactory.path.to.server/repository/someFile.ext"
```

Also, second thing I learned:
Ensure you run coverage tests in your code often, because as code changes, if tests are not updated to match, you will definitely
Miss out on testing either complete features or core debugging/error handling scenarios.


### 11th Jan, 2021:

How exceptions work in Stream operations
<p>
If you've got a Java "Stream" object that's being iterated upon via the `forEach` calls,
an exception in the loop will end the execution for all following iterations rather than just one iteration. 
The preferred way handling an error case in this scenario is using `return` after logging.

This is because `continue` would be invalid as each forEach in the background is a map of a function (method) over items in the Stream. 
Continue in a method makes no sense! 
</p>

