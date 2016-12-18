# KISS Launcher - Keep It Short and Simple

![FEUP's logo](Images/feup.png)

### Software Engineering - MIEIC

##### Group:
* [Cláudia Margarida da Rocha Marinho](https://github.com/arwen7stars) - up201404493
* [José Carlos Alves Vieira](https://github.com/Evenilink) - up201404446
* [Tiago Rafael Ferreira da Silva](https://github.com/tirafesi) - up201402841

### Table of Contents
* [Software Maintainability](#software-maintainability)
* [Report Evolution Process](#report-evolution-process)
* [Link to Pull Request](#link-to-pull-request)

## Software Maintainability
Using [Better Code Hub](https://bettercodehub.com) to analyse our project's compliance, the result was quite good, rating a 7 out of 10.

[![BCH compliance](https://bettercodehub.com/edge/badge/Evenilink/KISS)](https://bettercodehub.com)

It failed on the following tests:
* **Writing short units of code** - some of the units have more than 15 lines of code, making it harder to understand, reuse and test. These units needed to be split in smaller units in order to get a better score.
* **Writing simple units of code** - some of the units have more than 5 branch points (if, for, while, etc), raising complexity during code analysis, change and test. Sub-branches that have more than 5 branch points needed to be seperated to new units of no more than 5 branch points.
* **Automate tests** - this one didn't come as a surprise, since we've been refering it through the whole project. Since our project only tests some basic functionalities, the extend of testing code is below the suggested. The project has 5417 lines of production code and only 71 of test code, and the sugested is at least half of the production code, in this case 2574 lines.

It succeeded on the following tests:
* **Write code once** - very few code is duplicated, avoiding copy-paste erros and error fix in multiple places.
* **Keep unit interfaces small** - the suggested number of parameters per unit is 2. Most of the units have 2 to 4, making the units easy to understand and reuse.
* **Separate concern in modules** - the suggested number of modules calls is no more than 10, to avoid the consequences of change. Only 4 modules have more than 10 calls.
* **Couple architecture components loosely** - it's possible to maintain components in isolation.
* **Keep architecture components balanced** - components are well defined based on their functionality. It's easy to locate the code we need, showing a balance in both components amount and size.
* **Keep the codebase small** - the code's volume is mainly right, improving maintainability and lowering the amount of work it would take to make structural changes.
* **Write clean code** - although there are some commented code, there's only a few code smells, providing a maintainable environment for the developers to work on.

## Report Evolution Process
We decided to add an option to let the user choose the Default Launcher. We noticed that in some phones after choosing to always have the Android Launcher as default, it was complicated to revert this. Therefore, after pressing the  *Home Button*, the phone would show the default *Home Screen* instead of going back to KISS. As such, we thought that having an option to change this behavior on the application itself would be very useful.

It was fairly easy to locate the parts in the source code that needed to be modified. Since the feature we wanted to implement consisted of adding an option to the settings screen, we knew from the start that most of our changes would be focused around the file *preferences.xml*.

Adding the extra option to the settings screen was the easy part. We just had to add an extra entry to *preferences.xml* and create a new *Preference* class.

*preferences.xml*
```java
<PreferenceScreen android:title="@string/title_advanced">
    <fr.neamar.kiss.preference.DefaultLauncherPreference
        android:dialogMessage="@string/default_launcher_warn"
        android:key="default-launcher"
        android:title="@string/default_launcher_title"/>
```

*DefaultLauncherPreference.java*

```java
public class DefaultLauncherPreference extends DialogPreference {

}
```

The difficult part came afterwards. We had to find a way to change the Default Launcher.

Well, turns out this is simply impossible to do programmatically. As you can imagine, allowing an application to set the default behavior of the phone would be a major security problem.

As such, the only solution was to ask the user to change the Default Launcher himself. But how could this be accomplished?

We could create an [App Chooser](https://developer.android.com/training/basics/intents/sending.html#AppChooser), but that raised a problem. It didn't allow the user to set an app as default. It only allowed to select the app for that *specific* action, at that *specific* moment. So this wouldn't be any good.

After struggling for a while to find an answer, we came across [this](http://stackoverflow.com/questions/13167583/clearing-and-setting-the-default-home-application/13239706#13239706) post, on Stack Overflow.

And so, we decided to follow this approach.

We simply had to *trick* the phone, in order for it to think that there was a new app installed, so that it would show the App Chooser (notice that this chooser now has the option to set default apps, since it was launched by the System itself, and not programmatically).

In order to do this, we created a dummy (or fake - however you want to call it) Activity. As the name suggest, this is just a *completely* empty Activity. But this alone isn't enough. We need to tell the System that there is, in fact, a new app capable of receiving *Home* intents (the *Intent* launched when the *Home Button* is pressed). This was done in the *AndroidManifest.xml*, by declaring an *intent-filter*.

*DummyActivity.java*
```java
public class DummyActivity extends Activity {

}
```

*AndroidManifest.xml*
```java
<activity android:name=".DummyActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.HOME" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
    </activity>
```

Now, all that was left was to fill the *DefaultLauncherPreference* and trick the phone into launching an App Chooser through the System.

This was done by creating a new *ACTION_MAIN Intent*, with a *HOME* category associated to it, and then launching it.


*DefaultLauncherPreference.java*
```java
// create a new (implicit) intent with MAIN action
Intent intent = new Intent(Intent.ACTION_MAIN);
// add HOME category to it
intent.addCategory(Intent.CATEGORY_HOME);
// launch intent
context.startActivity(intent);
```

However, after the first time this was used, it would stop working, because the phone would no longer assume the DummyActivity was a new app. The phone already knew it, so the App Chooser wouldn't launch, since the user had already selected what default app he wanted.

In order to avoid this, DummyActivity needs to start off disabled, in order to *hide* it from the system. It then gets enabled when it's time to launch the intent that will trick the phone. After that, it is disabled once again, in order for it to be reusable the next time we need to trick the phone.

*AndroidManifest.xml*
```java
<activity android:name=".DummyActivity"
      android:enabled="false">
      <intent-filter>
          <action android:name="android.intent.action.MAIN" />
          <category android:name="android.intent.category.HOME" />
          <category android:name="android.intent.category.DEFAULT" />
      </intent-filter>
  </activity>
```

*DefaultLauncherPreference.java*
```java
// get dummyActivity
ComponentName componentName = new ComponentName(context, DummyActivity.class);

// enable dummyActivity (it starts disabled in the manifest.xml)
packageManager.setComponentEnabledSetting(componentName, PackageManager.COMPONENT_ENABLED_STATE_ENABLED, PackageManager.DONT_KILL_APP);

// create a new (implicit) intent with MAIN action
Intent intent = new Intent(Intent.ACTION_MAIN);
// add HOME category to it
intent.addCategory(Intent.CATEGORY_HOME);
// launch intent
context.startActivity(intent);

// disable dummyActivity once again
packageManager.setComponentEnabledSetting(componentName, PackageManager.COMPONENT_ENABLED_STATE_DISABLED, PackageManager.DONT_KILL_APP);
```

And it is complete. Now, everytime the user selects the option to change the Default Launcher, an App Chooser will appear.

## Link to Pull Request

The pull request is located [here](https://github.com/Neamar/KISS/pull/576).
