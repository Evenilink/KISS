# KISS Launcher - Keep It Short and Simple

![FEUP's logo](Images/feup.png)

### Software Engineering - MIEIC

##### Group:
* Cláudia Margarida da Rocha Marinho - up201404493
* José Carlos Alves Vieira - up201404446
* Tiago Rafael Ferreira da Silva - up201402841

## Software Design

### Table of Contents
* [Software Testability and Reviews](#Software-Testability-and-Reviews)

### Software Testability and Reviews
* Testability: the change to expose a fault/bug through tests.
* Controllability: affecting the behavior of the software, with how much work can we provide a program with the right and needed inputs for testing that will affect his behavior.
* Observability: how easy it is for developers to observe the software behavior, more specificly, his outputs, and his response to the tests.
* Isolateability: how deep can the developers test a specific component isolated from all the other components, in other words, how much specific can we get to test that component.
* Separation of Concerns: how specific is the component which we want to test, the degree level of his responsibilities.
* Understandability: how good a component under test is documented.
* Heterogeneity: using diverse technologies, how deep are the test methods and required tools in order to be testable using those technologies.

### Test Statictics and Analystic:
We stated our project didn’t have tests implemented. As such, the projects doesn’t have information about the number of test cases, percentage of coverage or number of flaky tests.
The only automatic test being used is a very basic one, like testing if the project compiles or if it displays certain information, like if the results appear, if it can type in the search bar, etc...
Eclipse is to be blamed for the inexistence of robust tests. At the time the project started, Eclipse made tests complicated to program an to run. KISS relies on system data (e.g. contacts), so the app needed to know the contacts in order to get them. To develop a test in order to see if the contacts were being correctly processed, a contact needed to be created (every time a test was running). If the test failed the contact would remain in the system. Running the test many times would create a huge amount of useless data! It was impossible to use emulators for the job, since they were not yet available.


### Identify a new bug and/or correct a bug
