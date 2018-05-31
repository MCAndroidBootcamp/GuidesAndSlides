
# Lesson 01 - Creating your first Android Project

#### Create a new project.  
Open Android Studio and pick the ‘Start a new Android Studio project’ from the welcome screen or just go to ‘File -> New -> New Project…’

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358817/Screen_Shot_2018-05-23_at_4.36.17_PM_wz4wwy.png" width="400px">

#### Enter project details

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358815/create_project_ov8aex.png" width="400px">

Fill in the details and be sure you check the option to ‘Include Kotlin support at the bottom.
This will create your project with Kotlin instead of Java.

Click next and leave the default values on the remaining screens.

#### Selecting your target SDK

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358818/target_andriod_devices_gvhufw.png" width="400px">

For the form factors and minimum SDK you’ll want to target API 21: Lollipop or something around that so your app will  function on most devices.

### Select the activity for the application. 

> An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with ```setContentView(View)```. 

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358816/choose_empty_activity_pc8y3o.png" width="400px">

A typical application is made up of many activities.  For now just select ‘Empty Activity’ and click Next.
You’ll be prompted to name the main activity. The default ‘MainActivity’ is fine. Select the option to ‘Generate Layout File’ along with the option for ‘Backwards Compatibility’. Then click ‘Finish’.

Android Studio will now create the project and you may be presented with a blank grey screen. You will want to open the project view. You can do this by clicking the ‘1: Project’ button on the left sidebar of the screen.

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358817/main_activity_default_code_glfyu7.png" width="400px">

You’ll see in the sidebar some files and the main activity code screen will also be shown. You probably want to view the actual project folder on the left. So click the drop-down list where it says ‘Android’ and select ‘Project’ to change the view to that of the project folder structure.

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358817/project_tree_q3nmma.png" width="400px">

Congratulations! So far you’ve created a complete app that does nothing useful.  

You can actually run it if you want but we’ll now modify it to make it more interesting. And useful!



### Adding Controls to your App

From the project view you should be able to navigate to the project's  ```app | src | main | java```
in the tree to see the ```MainActivity.kt``` code.

 You can view the layout by selecting the project's ``` app | src | res | layout ``` folders to open the ```main_activity.xml``` in design view. 


You can toggle between the two views by clicking on the tabs for Design and Text along the bottom menu.

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358817/Screen_Shot_2018-05-23_at_4.39.45_PM_zgdes4.png" width="150px">

Right now look at design view. You may see ‘Hello World!’ already written. This is the default label and not our app. You can actually delete that component. You won’t need it.

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358818/Screen_Shot_2018-05-23_at_4.40.06_PM_s75vcf.png" width="300px">


#### To add a component
Drag the component, such as a Button, on to the screen. In the ‘Palette’ menu to the left of the design, you’ll see a number of available components. Click Button and drag it to the right. 

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358817/main_activity_design_view_mw0b5u.png" width="400px">


The blue lines and arrows indicate the alignment. However, if you do nothing all the controls you add will float to the top left of your screen. Probably not the effect you want. 

Use your mouse to size everything and place it on the screen as you like. 

> Pro Tip: Click the magic wand to “Infer Layout”. The magic wand is your new best friend. 
> Saves hours of painful layout configuration.
> <img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527358815/infer_constraints_xgxsox.png" width="300px">

#### Setting the ID of your component

Whenever you add a component, you should give it an ID that is meaningful to you. 
Potentially you’ll be adding lots of components to apps in the future so having unique and descriptive IDs simplifies your job as a programmer. 

<!-- todo: need image of setting the button id -->

In the ID box (on the right of your screen) type ‘btnSayHello’. This tells you the component  is a button and it will Say Hello. A perfectly descriptive ID! 

> Pro Tip: When naming components, variables or methods the first letter should be lower case. This is a standard Java programming convention. Class names should begin with upper case. If you see something in upper case you know it’s a class in Java/Kotlin.

#### Changing the properties of your controls
While you’re at it… set the text of your button to display “Say Hello”. You can set these properties in from the layout view. Or when we move to XML view you’ll see you can also set them (or modify them) there.
Add another component:
You need a TextView. This allows the app to show text to the user which will change when the user clicks the button. So drag a TextView to the screen and edit its attributes to call it lblText (for label) and change the text to “This is a test”. That text will get replaced when the button is clicked and the code it run.


#### Adding constraints
You can set the constraints yourself by clicking the circles on both sides of the component when it is selected and the component will remain centrally positioned. Now do the same to tie it to the top and bottom.
Set the constraints for every component. You can line them up with the edges of the screen or each other.


#### Let’s do some programming
You’ll write your Kotlin code in the ```MainActivity.kt```  file and other files you create with a *.kt* extension. Some code has already been generated.  Code inside the ```onCreate``` method is executed when the app is started up.

#### Your next task 
Add a function to the ```onCreate``` method so you can actually click the button you have placed on the screen. This will display the different text.

#### Add Anko to your project
> In computer programming, a third-party software component is a reusable software component developed to be either freely distributed or sold by an entity other than the original vendor of the development platform.Anko is one such library. So is Gradle.

Before we add any code we need to bring a Kotlin library named Anko into our project. This makes interacting with our elements much easier. 

Copy the following line of code:

``` java
Compile “org.jetbrains.anko:anko-commons: 0.10.4” 
```

> You can find the latest version number at github.com/Kotlin/anko.

Paste this line into the *build.gradle* file under the dependencies method.  

Note that there are two build.gradle files – you’re interested in the one that is inside your app folder and part of your project. If you see a comment telling you not to modify the file, you're in the wrong one.

Once you have modified build.gradle you should see a message at the top of your screen indicating that gradle files have been changed and a project sync is needed. Click the ‘Sync Now’ link and *wait* for the project sync to complete.

There should be no errors when it finishes the gradle build.


Return to the MainActivity.kt file

Add a function named ```changeText```. This function will change the text of the TextView to whatever you want when you click the ‘Say Hello’ button.

This code sets the onClickListener for the button so it will work and set the text for the lblText element to ‘Hello world!”

#### One last step  
Your program needs to call changeText()  at the end of the onCreate function. Do this by writing the name of the function followed by ()’s  as the last line in onCreate as shown below:

#### Running your app
Click the green play button at the top right of your screen to run your app. You can select a virtual device or a connected device and your app will be built, copied, installed and launched accordingly.

Let’s see how your basic MainActivity looks in Kotlin

```kotlin
package com.example.kotlinhelloworld

import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }
    fun changeText(){
        btnSayHello.setOnClickListener{
            lblText.setText("hello")
        }
    }
}

```

This is pretty much the same as in Java with just some specific Kotlin syntax in the class and method declaration. This doesn’t give us much so let’s add a few things and actually see some Kotlin in action.


Let’s add a button to our layout that will change the welcome text view from ‘Hello World’ to something more relevant to our context. We also need to add an id to the existing text view. The activity_main.xml should now look like this:

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.jcmsalves.kotlinplayground.MainActivity">

    <TextView
        android:id="@+id/welcomeTextView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/updateTextButton"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Update welcome message"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="@id/welcomeTextView"
        app:layout_constraintBottom_toBottomOf="parent" />

</android.support.constraint.ConstraintLayout>
```

Now we need to access those views in our MainActivity so we can actually update the text when the button is clicked. This is where you would either use findViewById() or something like ButterKnife to bind the views. 

Well with Kotlin you don’t need to do any of that, look at how simple it is to update the welcome message on click of the button.


As soon as you start typing the name of a view from your layout file, Android Studio will offer auto complete and will automatically add the following static import for all the views in our layout file.

```java
import kotlinx.android.synthetic.main.activity_main.*
```

Then this is all you need to add a listener to the button and change the welcome message on click. 

#### Testing your application

### Putting your project on GitHub

> You *must* put your project on GitHub. Once the classroom computers reboot they reset to the way they were when you came to class. Everything is erased. So you need to save it on GitHub.




```python

```
