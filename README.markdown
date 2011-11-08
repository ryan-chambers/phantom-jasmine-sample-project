NOTE: This project is forked from some work done by Ivan DeMarino
[here](https://github.com/detro/phantomjs-jasminexml-example). He also wrote an 
[article](http://blog.ivandemarino.me/2011/07/09/Maven-PhantomJS-and-Jasmine-to-write-your-JS-Unit-Testing) 
describing his approach. The only change I made was to add the same validation.js and test file
that I'm using in the other sample projects.

-RC


# PhantomJS - Jasmine XML - Example Project Layout

## Description
This is an example project layout, that realises JavaScript Unit Testing
using [Jasmine](https://github.com/pivotal/jasmine) on the top of
[PhantomJS](https://github.com/ariya/phantomjs).

It is designed to be used _standalone_ or _within Maven_.

## Use Standalone

    $ phantomjs test/phantomjs_jasminexml_runner.js test/test_runner.html xml_output_dir/
    
This will produce a set of XML outputs, one for every Jasmine Test Suite.

## Use within Maven
The project _already_ provides a `pom.xml` that does the trick. So a simple:

    $ mvn test

or

    $ mvn clean install

will launch the tests and, if they all pass, make the mvn build pass.
If any test fails, the mvn build fails.

## Internals
Well, explaining the internals of PhantomJS here is pointless, but to understand how
this works, you need to understand a simple idea: PhantomJS provides a main "Javascript Context"
from within which, the user is able to _spawn_ a WebPage, effectively creating another "Javascript Context".

The new context has the following characteristics:

* **isolated** - the page doesn't know anything about phantom and phantom API
* **controllable** - the main phantom context provides API to control/influence the page context

### So, how does it work?
The _magic_ is done by 2 scripts:

    test/phantomjs_jasminexml_runner.js
    test/lib/jasmine-reporters/jasmine.phantomjs-reporter.js

`jasmine.phantomjs-reporter.js` is a _plugin_ for Jasmine that saves the result of the
tests at the bottom of the webpage where the test runs.

`phantomjs_jasminexml_runner.js` instead runs the show, launches the tests,
extracts the result from the page, then saves it to the directory passed (see source).

**Happy Testing ;-)**

