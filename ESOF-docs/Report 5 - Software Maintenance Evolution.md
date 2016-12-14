# KISS Launcher - Keep It Short and Simple

![FEUP's logo](Images/feup.png)

### Software Engineering - MIEIC

##### Group:
* Cláudia Margarida da Rocha Marinho - up201404493
* José Carlos Alves Vieira - up201404446
* Tiago Rafael Ferreira da Silva - up201402841

### Table of Contents
* [Software Maintainability](#Software-Maintainability)
* [Report Evolution Process](#Report-Evolution-Process)
* [Link to Pull Request](#Link-to-Pull-Request)

### Software Maintainability
Using [Better Code Hub](https://bettercodehub.com) to analyse our project's compliance, the result was quite good, rating a 7 out of 10.
[![BCH compliance](https://bettercodehub.com/edge/badge/Evenilink/KISS)](https://bettercodehub.com)

It failed on the following tests:
* Writing short units of code - some of the units have more than 15 lines of code, making it harder to understand, reuse and test. These units needed to be split in smaller units in order to get a better score.
* Writing simple units of code - some of the units have more than 5 branch points (if, for, while, etc), raising complexity during code analysis, change and test. Sub-branches that have more than 5 branch points needed to be seperated to new units of no more than 5 branch points.
* Automate tests - this one didn't come as a surprise, since we've been refering it through the whole project. Since our project only tests some basic functionalities, the extend of testing code is below the suggested. The project has 5417 lines of production code and only 71 of test code, and the sugested is at least half of the production code, in this case 2574 lines.

### Report Evolution Process

### Link to Pull Request