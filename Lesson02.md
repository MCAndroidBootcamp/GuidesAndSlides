
<!-- need code for using date picker and code for getting hour of day to prompt for morning, evening, etc -->

# Lesson 02 A More Personalized Greeting

* Add textboxes to prompt for the user's name, age and favorite movie
* Display a message to the user in a label

Create Lesson 01 again. You must do this as a new app. 
> The practice of creating apps over and over helps you to learn.

Add your application to GitHub. Use the command line. It gives you greater control.







### ActivityMain.kt
```kotlin
package com.mcandroid.myapplication

import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*


class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        changeText()
    }

    fun changeText(){
        btnSayHello.setOnClickListener{

            var message = "Hello, " + etName.text.toString()
            message += "\n You are " + numAge.text.toString()
            message += "\n Your favorite movie is " + etMovie.text.toString()
            lblMessage.text = message
        }
    }
}

```


### activity_main.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity"
    tools:layout_editor_absoluteY="81dp">

    <Button
        android:id="@+id/btnSayHello"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="32dp"
        android:text="@string/say_hello"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/etMovie" />

    <TextView
        android:id="@+id/lblNamePrompt"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:layout_marginTop="16dp"
        android:text="@string/name"
        android:textSize="30sp"
        app:layout_constraintStart_toStartOf="@+id/guideline2"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/lblAgePrompt"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="10dp"
        android:layout_marginTop="18dp"
        android:text="@string/age"
        android:textSize="30sp"
        app:layout_constraintStart_toStartOf="@+id/guideline2"
        app:layout_constraintTop_toBottomOf="@+id/lblNamePrompt" />

    <TextView
        android:id="@+id/lblMoviePrompt"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        android:text="@string/movie"
        android:textSize="30sp"
        app:layout_constraintStart_toStartOf="@+id/lblAgePrompt"
        app:layout_constraintTop_toBottomOf="@+id/lblAgePrompt" />

    <EditText
        android:id="@+id/etName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:ems="10"
        android:inputType="textPersonName"
        android:textSize="18sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/numAge"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="13dp"
        android:ems="10"
        android:inputType="number"
        android:textSize="18sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/etName" />

    <TextView
        android:id="@+id/lblMessage"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="41dp"
        android:inputType="textMultiLine"
        android:textSize="36sp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/btnSayHello" />

    <EditText
        android:id="@+id/etMovie"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="5dp"
        android:ems="10"
        android:inputType="text"
        app:layout_constraintStart_toStartOf="@+id/numAge"
        app:layout_constraintTop_toBottomOf="@+id/numAge" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_begin="20dp" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        app:layout_constraintGuide_begin="20dp" />

    <android.support.constraint.Guideline
        android:id="@+id/guideline3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_begin="20dp" />


</android.support.constraint.ConstraintLayout>
```

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527527416/Lesson02Design_klfyfg.png" width="400px">

### AndroidMainfest.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mcandroid.myapplication">

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".MainActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

### app/build.gradle
```java
apply plugin: 'com.android.application'

apply plugin: 'kotlin-android'

apply plugin: 'kotlin-android-extensions'

android {
    compileSdkVersion 27
    defaultConfig {
        applicationId "com.mcandroid.myapplication"
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

dependencies {
    implementation "org.jetbrains.anko:anko-commons:0.10.5"
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation"org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}
```


### What's next?
* Change the greeting based on the time of the day
* Clear the textboxes on focus of the name textbox
* Prompt for email address and display gravatar, if any. Provide fake gravatars for testing.

### How to get an MD5 hash in Kotlin

```kotlin
import java.math.BigInteger
import java.security.MessageDigest

fun String.md5(): String {
    val md = MessageDigest.getInstance("MD5")
    return BigInteger(1, md.digest(toByteArray())).toString(16).padStart(32, '0')
}
```



```python

```
