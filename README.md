# Midterm Assignment Project 2
*Emmely Lundberg cph-el69*

_______

### Overview 

This project is about Unit Testing, Testable Code, Mocking and Code Coverage.
An existing project is used to refactor and write new JUnit test.

#### Original project startCodeForTesting1


This project contains start code for an exercise given at cphbusiness.dk for the educations:
Original code project: https://github.com/Lars-m/startCodeForTesting1.git 

_______

### Assignment Tasks

 - Explain the necessary steps you did to make the code testable, and some of the patterns involved in this step 
 - Execute your test cases 
 - Explain basically about JUnit, Hamcrest, Mockito and Jacoco, and what problems they solve for testers 
 - Demonstrate how you used Mockito to mock away external Dependencies 
 - Demonstrate how/where you did state-based testing and how/where you did behavior based testing 
 - Explain about Coverage Criterias, using the results presented by running Jacoco (or a similar tool) against you final test code. 
 - Explain/demonstrate what was required to make this project use, JUnit (Hamcrest), Mockito and Jacoco 

 _______
 
### Assignment Solution

 
#### 01) Explain the necessary steps you did to make the code testable, and some of the patterns involved in this step 
 
##### Refactoring
 
 
To make the code teatable it needed to be refactored. 
To write testable code we should follow the SOLID principles. 
I have only focused on refactoring the code by the principles highlighted below.

 - __Single responsibility principle (A class should only have a single responsibility)__
 - Open/closed Principle (Meaning, that the code should be open for extensions, but closed for modifications)
 - Liskov substitution principle (Objects should be replaceable with instances of their subtypes and still maintain correctness, functionalwise. ie. Rest of the software doesn't need to be rewritten, because of the replacement)
 - __Interface segregation principle (meaning, many interfaces are better than one general purpose one).__
 - __Dependency inversion principle (Meaning, shifting the responsbility of dependencies away from the object, to reduce it's responsability, ie. Do Inversion of controll and dependency injection).__
 
General steps I took to refactor the code 
 
 - The code was untestable because hidden dependencies. The refactoring separated out dependencies.
 - The original code violated many rules for writing testable code. Refactor the code using polymorphism and the Factory Method Pattern helped to make the code more testable.
 
##### Refactor DateFormatter

*Before*
 
In the original code a new Date object was instantiated inside the method which violated single responsibility pattern and is a hidden dependency.
It didn't make use of Polymorphism and it had a static method.


*The basic issue with static methods is they are procedural code. It not easy unit-test procedural code. 
Unit-testing assumes that we can instantiate a piece of the application in isolation and dependencies can be mocked or replaced with stubs.
This is hard or impossible when methods are static. For writing testable code static methods should be avoided.*
 
*After*

A interface IDateFormatter has a non static method that inject a Date instance (doesn't have the responsibility to create the date).
The DateFromatter implements the interface IDateFormatter.

#####  Refactor JokeFetcher

By using polymorphism for fetching jokes ( we do this in four distinct ways) one interface is used IJokeFetcher.  
EduJoke, Moma, Chuck, and Tambal all implements the interface IJokeFetcher.
Each IJokeFetcher implementation contains the different logic when fetching the joke from the API.

By using polymorphism like this the JokeFetcher is now longer responsible for implementing the different ways to fetching the Jokes.
The responsibility has been put in to IJokeFetcher (Single Responsibility Principle). Each Class knows how to fetch the joke. Nice!


FetcherFactory implements IFetcherFactory interface.

The difference IJokeFetcher are instantiated with FetcherFactory that uses the Factory Method Pattern.

The Factory Method Pattern is used because logic is required for instancing the IJokeFetcher types. This pattern implements this logic and returns the right types,

Classes and Interfaces after refactoring

[![https://gyazo.com/e74f25c1bb39744461439916cc049ff6](https://i.gyazo.com/e74f25c1bb39744461439916cc049ff6.png)](https://gyazo.com/e74f25c1bb39744461439916cc049ff6)

#### 02) Execute your test cases 

 [![https://gyazo.com/f95872b3c7c7d5d1b19a523bd5183a0a](https://i.gyazo.com/f95872b3c7c7d5d1b19a523bd5183a0a.png)](https://gyazo.com/f95872b3c7c7d5d1b19a523bd5183a0a)
 
 
#### 03) Explain basically about JUnit, Hamcrest, Mockito and Jacoco, and what problems they solve for testers 
 
##### JUnit
*Is:*
 
JUnit is a unit testing framework for Java programming language. 
JUnit is a simple, open source framework to write and run repeatable tests. It is an instance of the xUnit architecture for unit testing frameworks. JUnit features include:

Assertions for testing expected results
Test fixtures for sharing common test data
Test runners for running tests
JUnit was originally written by Erich Gamma and Kent Beck. [source](https://junit.org/junit4/faq.html#overview_1)
 

 
*Solves problem for testers:*
 
It is a tool for writing automated test against code.
It is also a tool used in TDD. It aids the programmer to write the test against his own code. So coding and testing can be done by the same developer.
There is huge cost benefits in discovering bugs early in the development of code.
 
##### Hamcrest
*Is:*
  
Hamcrest. Matchers that can be combined to create flexible expressions of intent. Assertions can be stated declaratively.
  
*Solves problem for testers:*
  
Extends the JUnit library with more flexibility and more readable tests. Readable test can act as part of the documentation for developers.
  
  
##### Mockito
*Is:*
  
Mockito is the most popular mocking framework for Java, but exists for other languages aswell. 
It lets testers write (unit) tests with a clean & simple API. 
 
*Solves problem for testers:*

The framework aids the Junit tests in mocking away dependencies so objects can be tested in isolation.  
It has feature of Behavior test with *verify* by asking questions about interactions after execution. 
  
##### Jacoco
*Is:*
 
JaCoCo – a code coverage reports generator for Java projects.
 
*Solves problem for testers:*
  
It helps the tester to get fast automated generated code coverage analyzes/metrics. 
I saves time for testers rather than calculated it manually.
It is a tool to measure code coverage and thereby the confidence in the (JUnit) tests.

  
#### 04) Demonstrate how you used Mockito to mock away external Dependencies 

I use Mockito to mock away the external dependency Date in DateFormatterTest. 
By having a mock returning a fixed date the DateFormatter class can be tested in isolation.


In IFetcherFactoryTest I mock away the FetcherFactory so it returns stubbed Jokes.
In this way we can test the JokeFetchers behavior in isolation. 

Below the setup of mocks for testing the JokeFetchers and FetcherFactory.

[![https://gyazo.com/a3ec5006e2bceb17103ad8b1fd79eec7](https://i.gyazo.com/a3ec5006e2bceb17103ad8b1fd79eec7.png)](https://gyazo.com/a3ec5006e2bceb17103ad8b1fd79eec7)


#### 05) Demonstrate how/where you did state-based testing and how/where you did behavior based testing

State based testing is when you exercise one or many methods of an object and then assert the expected state of the object.
In DateFormatterTest I do statebased testing by checking how the time(state) change depending on different input.

[![https://gyazo.com/70975fa71be4bcc1df74ae4b1dcea053](https://i.gyazo.com/70975fa71be4bcc1df74ae4b1dcea053.png)](https://gyazo.com/70975fa71be4bcc1df74ae4b1dcea053)

Behavior based testing is when you expect specific interactions to occur between objects when certain methods are executed. 
Utilization of Mockitos method *verify* the test verify that expected interaction between objects of a system happened. 
Behavior based testing is about specifying how the system should behave rather than specifying the expected result of running the system.

Below is an example of my test where we verify a certain behavior, in this case that the method *getFormattedDate* is called exactly one time *times(1)*.

``` 
verify(dateFormatterMock,times(1)).getFormattedDate(anyString(),anyObject());// Verify with wanted number of invocations 
```

[![https://gyazo.com/ab69ffc4790e8e5f113b692a686aca30](https://i.gyazo.com/ab69ffc4790e8e5f113b692a686aca30.png)](https://gyazo.com/ab69ffc4790e8e5f113b692a686aca30)


#### 06) Explain about Coverage Criterias, using the results presented by running JaCoCo (or a similar tool) against you final test code. 

Code coverage is a way of ensuring that your tests are actually testing your code. 
When you run your tests you are presumably checking that you are getting the expected results. 
Code coverage will tell you how much of your code you exercised by running the test. 
The higher code coverage the more confidence we can have in the test. As a best practice 80% code coverage is aimed for.

The picture below shows the result from running the automated code coverage tool JaCoCo. I haven't implemented the aimed for 80% code coverage because I have focused on the learning aspects in this project.

[![https://gyazo.com/ae3a1bd4822897f49c7661391566ecb3](https://i.gyazo.com/ae3a1bd4822897f49c7661391566ecb3.png)](https://gyazo.com/ae3a1bd4822897f49c7661391566ecb3)


#### 07) Explain/demonstrate what was required to make this project use, JUnit (Hamcrest), Mockito and JaCoCo  

I used the IDE IntelliJ from Jetbrains and also upgraded to JUnit 5. I couldn't use the original pom-file as is so I added some dependencies there to make it work (see pom-file).

The first step was to make the code testable following the SOLID principles in particular Single Responsibility, Polymorphism and no hidden dependencies.
The code needed to be refactored as described above to be able to test it with JUnit.
So getting rid of hidden dependencies was the first step in doing that.

When the code was refactored the code were ready to test.

I created three test files using Mockito and Hamcrests:

 - IFetcherFactoryTest 
 - DateFormatterTest
 - JokeFetcherTest

Test and verify behavior using Mockito were made easier by refactoring the code to Factory Method Pattern in IFetcherFactory interface implemented in FetcherFactory.

Becuase I used JUnit 5 and the IDE Intellij both Hamcrest anf JaCoCo was build into the project and could be used on the fly.

Hamcrest is used to make the test more readable and JaCoCo to see ho much code coverage the test has.




