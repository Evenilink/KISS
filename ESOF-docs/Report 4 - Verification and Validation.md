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

## Software Testability and Reviews
* **Testability**: the chance to expose a fault/bug through tests. In our project we can only identify bugs through manual testing, so the testability success would depend on how much time and effort the developers put into it.

* **Controllability**: ability to affect the behavior of the software, that is, how much work can we provide a program with the needed inputs for testing that will affect its behavior. This one is quite hard in our project. Since the tests needed to have access to the system's data, developing a robust test suite to handle the creation of such data would be hard.

* **Observability**: how easy it is for developers to observe the software behavior, more specifically, its outputs and responses to tests. Since there are no specific tests implemented, this is pretty much impossible to see in the project. Using manual tests we can only be aware of the difference of what happened and what should have happened.

* **Isolateability**: how deep can the developers test a specific component isolated from all the other components. In other words, how specific can we get to test that component. In our case, the answer is not much. Due to the nature of the tests (manual), we can only test components as a whole that are already part of the system, thus decreasing isolateability.

* **Separation of Concerns**: how specific is the component we want to test, the degree of its responsibilities. Since we can't isolate a component via manual testing, its responsabilities might not be only related to itself, but may contain other component's responsabilities as well.

* **Understandability**: how good a component under test is documented. The only understandability that comes from manual testing can only be the code itself. Only through understanding of the code and realizing how something works can we really compreend how a component is handled in order to test it.

* **Heterogeneity**: using diverse technologies, how deep are the test methods and required tools in order to be testable using those technologies. The only technology at hands is java, so were test method implemented, java wouldn't be an obstacle at running them.

## Test Statictics and Analystic
We stated previously that our project doesn’t have tests implemented. As such, the project doesn’t have information about the number of test cases, percentage of coverage or number of flaky tests.

The only automatic test being used is very basic and tests few functionalities, like whether the project compiles, if it is possible to type in the search bar, if the search results are being displayed, etc.

Eclipse is to be blamed for the inexistence of robust tests. At the time the project started, Eclipse made tests complicated to program and to run. KISS relies on system data (e.g. contacts), so the app needed to acess the contacts in order to manipulate them. To develop a test in order to see if the contacts were being correctly processed, a contact needed to be created (every time a test was running). If the test failed the contact would remain in the system (a physical phone). Running the test many times would create a huge amount of useless data! It was impossible to use emulators for the job, so the developers could only use their own phones for this.

So, basically, all the testing is manual.

When it comes to statistics, since there are no real tests to run, there's no statistics to show, only some bugs info. When the app crashes, the user can send the stack trace to the developers team. This way it's possible for the developers to be aware of some bugs' existence and fix them.

## Identify a new bug and correct it
We started by fixing issue [#531](https://github.com/Neamar/KISS/issues/531).

In order to do this, we removed the finish() call from the SettingsActivity, like suggested on the issue report. This call was causing the SettingsActivity to close when it lost focus, so when the screen changed orientation, the activity would finish, as the issue states. We also did a slight cleaning on the if statement, since with the finish() removed there was no need for an if...else statement.

Once we fixed this issue, we noticed another problem. The SettingsActivity was not being affected by the 'Force portrait mode' option. At first we thought we caused it, but after testing with the original app, we found that it was indeed a KISS problem, and not ours. (Simply launch the SettingsActivity with the phone in landscape mode with the option 'Force portrait mode' checked, and you'll see the SettingsActivity will launch in landscape mode, instead of the expected portrait mode).

Thus, we also fixed this. In order to do it, we check whether the user checked the option to force portrait mode, and then change the screen orientation accordingly.

We opened a [Pull Request](https://github.com/Neamar/KISS/pull/559) with the fixes, which has been successfully merged onto the main branch.

Thus, we believe we have achieved both objectives for this point. We fixed an existing bug, and found a new bug, which we also fixed.

## Contributions
Every group member worked evenly in order to create this report.
