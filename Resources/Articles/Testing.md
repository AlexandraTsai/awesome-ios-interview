* [Testing](#testing)
  * [Test types](#testing)
  	  * [Unit Tests](#unit-tests)
  	  * [Integration Tests](#integration-tests)
  	  * [Functional Tests](#functional-tests)
     * [Acceptance Tests](#acceptance-tests)
  * [Writing tests for iOS apps](#what-is-the-benefit-writing-tests-in-ios-apps)
  * [“Arrange-Act-Assert”](#please-explain-arrange-act-assert)
  * [Test Driven Development](#what-is-the-test-driven-development-of-three-simple-rules)  
  * [Your new app is prone to crashing. What do you do?](#youve-just-been-alerted-that-your-new-app-is-prone-to-crashing-what-do-you-do)

# Testing

### Unit Tests
Tests the **smallest unit** of functionality, typically a **method/function** 
> e.g. given a class with a particular state, calling x method on the class should cause y to happen

Unit tests should be **focussed on one particular feature**
> e.g., calling the pop method when the stack is empty should throw an InvalidOperationException

Everything it touches should be done in memory; this means that the test code and the code under test **shouldn't**:

1.	Call out into (non-trivial) collaborators
2.	Access the network
3.	Hit a database
4.	Use the file system
5.	Spin up a thread

Any kind of dependency that is slow / hard to understand / initialise / manipulate should be stubbed / mocked / whatevered using the appropriate techniques so you can focus on what the unit of code is doing, not what its dependencies do.
In short, unit tests are as simple as possible, easy to debug, reliable (due to reduced external factors), fast to execute and help to prove that the smallest building blocks of your program function as intended before they're put together. The caveat is that, although you can prove they work perfectly in isolation, the units of code may blow up when combined which brings us to ...

### Integration Tests
Integration tests build on unit tests by **combining** the units of code and **testing that the resulting combination functions correctly**. This can be either the innards of one system, or combining multiple systems together to do something useful. 

* 和 unit tests 的差別是： **the environment**
Integration tests can and will use **threads**, access the **database** or do whatever is required to ensure that all of the code and the different environment changes will work correctly.
If you've built some serialization(連續的) code and unit tested its innards without touching the disk, how do you know that it'll work when you are loading and saving to disk? Maybe you forgot to flush and dispose filestreams. Maybe your file permissions are incorrect and you've tested the innards using in memory streams. The only way to find out for sure is to test it 'for real' using an environment that is closest to production.

* **The main advantage:**
 they will find bugs that unit tests can't such as wiring bugs 
  > (e.g. an instance of class A unexpectedly receives a null instance of B) 

  and environment bugs (it runs fine on my single-CPU machine, but my colleague's 4 core machine can't pass the tests). 
* **The main disadvantage:**
 integration tests touch more code, are less reliable, failures are harder to diagnose(診斷) and the tests are harder to maintain.
Also, integration tests **don't necessarily prove that a complete feature works**. The user may not care about the internal details of my programs, but I do!

### Functional Tests
Functional tests **check a particular feature** for correctness **by comparing the results** for **a given input against the specification(規格)**.

 Functional tests don't concern themselves with intermediate results or side-effects, just the result (they don't care that after doing x, object y has state z). They are written to test part of the specification such as, 
 >"calling function `Square(x)` with the argument of `2` returns `4`".

### Acceptance Tests
Acceptance testing seems to be split into **two types**:
Standard acceptance testing involves performing tests on the full system (e.g. using your web page via a web browser) to see whether the application's functionality satisfies the specification. E.g. "clicking a zoom icon should enlarge the document view by 25%." There is no real continuum of results, just a pass or fail outcome.
The advantage is that the tests are described in plain English and ensures the software, as a whole, is feature complete. The disadvantage is that you've moved another level up the testing pyramid. Acceptance tests touch mountains of code, so tracking down a failure can be tricky.
Also, in agile software development, user acceptance testing involves creating tests to mirror the user stories created by/for the software's customer during development. If the tests pass, it means the software should meet the customer's requirements and the stories can be considered complete. An acceptance test suite is basically an executable specification written in a domain specific language that describes the tests in the language used by the users of the system.

## What is the benefit writing tests in iOS apps?
Tests gives us a clear perspective on the API design, by getting into the mindset of being a client of the API before it exists.
Good tests serve as great documentation of expected behavior.
It gives us confidence to constantly refactor our code because we know that if we break anything our tests fail.
If tests are hard to write its usually a sign architecture could be improved. Following RGR ( Red — Green — Refactor ) helps you make improvements early on.

## Please explain “Arrange-Act-Assert”
AAA is a pattern for arranging and formatting code in Unit Tests. If we were to write XCTests each of our tests would group these functional sections, separated by blank lines:
Arrange all necessary preconditions and inputs.
Act on the object or method under test.
Assert that the expected results have occurred.

## What is the Test Driven Development of three simple rules?
You are not allowed to write any production code unless it is to make a failing unit test pass.
You are not allowed to write any more of a unit test than is sufficient to fail; and compilation failures are failures.
You are not allowed to write any more production code than is sufficient to pass the one failing unit test.

## You’ve just been alerted that your new app is prone to crashing. What do you do?

This classic interview question is designed to see how well your prospective programmer can solve problems. What you’re looking for is a general methodology for isolating a bug, and their ability to troubleshoot issues like sudden crashes or freezing. In general, when something goes wrong within an app, a standard approach might look something like this:  

#### Determine the iOS version and make or model of the device.
#### Gather enough information to reproduce the issue.
#### Acquire device logs, if possible.  

Once you have an idea as to the nature of the issue, acquire tooling or create a unit test and begin debugging.
A great answer would include all of the above, with specific examples of debugging tools like Buglife or ViewMonitor, and a firm grasp of software debugging theory—knowledge on what to do with compile time errors, run-time errors, and logical errors. The one answer you don’t want to hear is the haphazard approach—visually scanning through hundreds of lines of code until the error is found. When it comes to debugging software, a methodical approach is must.
