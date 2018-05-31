
# Links
https://github.com/MCAndroidBootcamp
    
https://cloudinary.com/console/media_library#  
https://developer.android.com/reference/android/app/Activity

https://android.jlelse.eu/build-a-bbc-world-news-aggregator-app-in-35-minutes-building-android-app-series-466cc7855abb


### Projects
https://github.com/AdamMc331/ToDo-Kotlin
https://androidessence.com/android/how-to-build-a-todo-list-in-kotlin-part-1-new-project/
https://www.raywenderlich.com/165824/introduction-android-activities-kotlin
https://github.com/SimpleMobileTools

### Errors
https://stackoverflow.com/questions/44994074/android-kotlin-error-return-type-is-unit-which-is-not-a-subtype-of-overridd


### Installing Android Studio
https://www.raywenderlich.com/120177/beginning-android-development-tutorial-installing-android-studio


```python
### Where to get help
stackoverflow
official documentation: 
    https://developer.android.com/training/basics/firstapp/creating-project
    https://developer.android.com/training/basics/firstapp/building-ui
```

# Kotlin Basics
http://blog.teamtreehouse.com/absolute-beginners-guide-kotlin
learnxinyminutes
try.kotlinlang.org

get current date/time: https://www.programiz.com/kotlin-programming/examples/current-date-time

# How to put your project in GitHub



```python
# How to get your project out of GitHub

```


```python
# How to use Cloudinary
```



### Please select SDK issue

## https://stackoverflow.com/questions/34353220/android-studio-please-select-android-sdk
Solution: CHange the 27 and 21 below to something from a project that works. This is from the apps build.gradle
```xml 
android {
    compileSdkVersion 27
    defaultConfig {
        applicationId "com.mcandroid.fourfunctioncalc"
        minSdkVersion 21
        targetSdkVersion 27
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
```

### Infer Constraints causes everything to go to 0,0 (Top, Left corner)
This is because you don't have any horizontal or vertical constraints

> If a view has no constraints when you run your layout on a device, it is drawn at position [0,0] (the top-left corner). You must add at least one horizontal and one vertical constraint for the view.

You could add this in xml to the button etc
```xml
app:layout_constraintBottom_toBottomOf="parent"
app:layout_constraintLeft_toLeftOf="parent"
app:layout_constraintRight_toRightOf="parent"
app:layout_constraintTop_toTopOf="parent"
app:layout_constraintVertical_bias="0.57"
```
then play with it in the design.


```python
how to do x in Android or Kotlin and also include HTML for trying Kotlin in a browser
```

### Why don't Android Apps have an Exit Button?

https://stackoverflow.com/questions/2439978/why-dont-android-applications-provide-an-exit-option
https://stackoverflow.com/questions/2033914/is-quitting-an-application-frowned-upon/2034238#2034238
    
    This will eventually get to your question, but I first want to address a number of issues you raise in your various comments to the various answers already given at the time of this writing. I have no intention of changing your mind -- rather, these are here for others who come to read this post in the future.

The point is that I cannot allow for Android to determine when my app is going to be terminated. that must be the choice of the user.

Millions of people are perfectly happy with the model where the environment closes up the application as needed. Those users simply don't think about "terminating" the Android app, any more than they think about "terminating" a Web page or "terminating" a thermostat.

iPhone users are much the same way, in that pressing the iPhone button does not necessarily "feel" like the app was terminated, since many iPhone apps pick up where the user left off, even if the app really was shut down (since iPhone only allows one third-party app at a time, at present).

As I said above, there is a lot of things going on in my app (data being PUSHed to the device, lists with tasks that always should be there, etc.).

I don't know what "lists with tasks that always should be there" means, but the "data being PUSHed to the device" is a pleasant fiction and should not be done by an activity in any case. Use a scheduled task (via AlarmManager) to update your data for maximum reliability.

Our users log in and can't be doing that every time they get a phone call and Android decides to kill the app.

There are many iPhone and Android applications that deal with this. Usually, it is because they hold onto logon credentials, rather than forcing users to log in every time manually.

For example, we want to check updates when exiting the application

That is a mistake on any operating system. For all you know, the reason your application is being "exited" is because the OS is shutting down, and then your update process will fail mid-stream. Generally, that's not a good thing. Either check updates on start or check updates totally asynchronously (e.g., via a scheduled task), never on exit.

Some comments suggest that hitting the back button does not kill the app at all (see link in my question above).

Pressing the BACK button does not "kill the app". It finishes the activity that was on-screen when the user pressed the BACK button.

It should only terminate when the users wants to terminate it - never ever any other way. If you can't write apps that behave like that in Android, then I think that Android can't be used for writing real apps =(

Then neither can Web applications. Or WebOS, if I understand their model correctly (haven't had a chance to play with one yet). In all of those, users don't "terminate" anything -- they just leave. iPhone is a bit different, in that it only presently allows one thing to run at a time (with a few exceptions), and so the act of leaving implies a fairly immediate termination of the app.

Is there a way for me to really quit the application?

As everybody else told you, users (via BACK) or your code (via finish()) can close up your currently-running activity. Users generally don't need anything else, for properly-written applications, any more than they need a "quit" option for using Web applications.

No two application environments are the same, by definition. This means that you can see trends in environments as new ones arise and others get buried.

For example, there is a growing movement to try to eliminate the notion of the "file". Most Web applications don't force users to think of files. iPhone apps typically don't force users to think of files. Android apps generally don't force users to think of files. And so on.

Similarly, there is a growing movement to try to eliminate the notion of "terminating" an app. Most Web applications don't force the user to log out, but rather implicitly log the user out after a period of inactivity. Same thing with Android, and to a lesser extent, iPhone (and possibly WebOS).

This requires more emphasis on application design, focusing on business goals and not sticking with an implementation model tied to a previous application environment. Developers who lack the time or inclination to do this will get frustrated with newer environments that break their existing mental model. This is not the fault of either environment, any more than it is the fault of a mountain for storms flowing around it rather than through it.

For example, some development environments, like Hypercard and Smalltalk, had the application and the development tools co-mingled in one setup. This concept did not catch on much, outside of language extensions to apps (e.g., VBA in Excel, Lisp in AutoCAD). Developers who came up with mental models that presumed the existence of development tools in the app itself, therefore, either had to change their model or limit themselves to environments where their model would hold true.

So, when you write:

Along with other messy things I discovered, I think that developing our app for Android is not going to happen.

That would appear to be for the best, for you, for right now. Similarly, I would counsel you against attempting to port your application to the Web, since some of the same problems you have reported with Android you will find in Web applications as well (e.g., no "termination"). Or, conversely, someday if you do port your app to the Web, you may find that the Web application's flow may be a better match for Android, and you can revisit an Android port at that time.


