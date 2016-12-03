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
* [Test Statictics and Analystic](#test-statictics-and-analystic)
* [Identify a new bug and correct it](#identify-a-new-bug-and-correct-it)
* [Contributions](#contributions)

### Software Testability and Reviews
* Testability: the chance to expose a fault/bug through tests. In our project we can only identify bugs through manual testing, so the testability sucess would depend on how much time and effort the developers put into it.

* Controllability: affecting the behavior of the software, with how much work can we provide a program with the right and needed inputs for testing that will affect his behavior. This one is quite hard in our project. Since the tests needed to have acess to the system's data, developing a robust test suite to handle the creation of such data would be hard.

* Observability: how easy it is for developers to observe the software behavior, more specificly, his outputs, and his response to the tests. Since there are no specific tests implemented, this is pretty much impossible to see in the project.

* Isolateability: how deep can the developers test a specific component isolated from all the other components, in other words, how much specific can we get to test that component. In our case not much, due to the nature of the tests (manual), we can only test components as a whole that are already part of the system, thus decreasing isolateability.

* Separation of Concerns: how specific is the component which we want to test, the degree level of his responsibilities. Since we can't isolate a component via manual testing, his responsabilities might not be only related to itself, but may contain other component's responsabilities too.

* Understandability: how good a component under test is documented. The only understandability that comes from manual testing can only the code itself. Only through understanding the code and realizing how something works is the only way to really compreend how a component is handled in order to test it.

* Heterogeneity: using diverse technologies, how deep are the test methods and required tools in order to be testable using those technologies. The only technology at hands is java, so were test method implemented, java wouldn't be an obstacle at running them.

### Test Statictics and Analystic
We stated our project didn’t have tests implemented. As such, the projects doesn’t have information about the number of test cases, percentage of coverage or number of flaky tests.

The only automatic test being used is a very basic one, like testing if the project compiles or if it displays certain information, like if the results appear, if it can type in the search bar, etc...

Eclipse is to be blamed for the inexistence of robust tests. At the time the project started, Eclipse made tests complicated to program and to run. KISS relies on system data (e.g. contacts), so the app needed to know the contacts in order to get them. To develop a test in order to see if the contacts were being correctly processed, a contact needed to be created (every time a test was running). If the test failed the contact would remain in the system. Running the test many times would create a huge amount of useless data! It was impossible to use emulators for the job, since they were not yet available.

So basically, all the testing is manual.

When it comes to statistics, since there are no real tests to run, there's no statistics to show, only some bugs info. When the app crashes, the user can send the stack trace to the developers team. This way it's possible for the developers to be aware of some bugs' existence.


### Identify a new bug and correct it
We started by fixing issue [#531](https://github.com/Neamar/KISS/issues/531).

In order to do this, we removed the finish() call from the SettingsActivity, like suggested on the issue report. This call was causing the SettingsActivity to close when it lost focus, so when the screen changed orientation, the activity would finish, as the issue states. We also did a slight cleaning on the if statement, since with the finish() removed there was no need for an if...else statement.

Once we fixed this issue, we noticed another problem. The SettingsActivity was not being affected by the 'Force portrait mode' option. At first we thought we caused it, but after testing with the original app, we found that it was indeed a KISS problem, and not ours. (Simply launch the SettingsActivity with the phone in landscape mode with the option 'Force portrait mode' checked, and you'll see the SettingsActivity will launch in landscape mode, instead of the expected portrait mode).

Thus, we also fixed this. In order to do it, we check whether the user checked the option to force portrait mode, and then change the screen orientation accordingly.

We already opened a [Pull Request](https://github.com/Neamar/KISS/pull/559) with the fixes, but at the time of writting of this report, it's still open (though it's been noticed, and feedback has been given).

Thus, we believe we have achieved both objectives for this point. We fixed an existing bug, and found a new bug, which we also fixed.

## Contributions
Every group member worked evenly in order to create this report.
