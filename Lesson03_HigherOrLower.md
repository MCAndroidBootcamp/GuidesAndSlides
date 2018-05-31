
# Lesson 03 - Higher or Lower

<!-- issue: Crashes on Correct Guess - not sure why -->
<!-- todo: user interface is annoying ... keyboard gets in the way and it doesn't clear the textbox on focus -->


### Update app/build.gradle

```java
apply plugin: 'com.android.application'

apply plugin: 'kotlin-android'

apply plugin: 'kotlin-android-extensions'

android {
    compileSdkVersion 27
    defaultConfig {
        applicationId "com.mcandroid.higherorlower"
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
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}
```

## Add a Main Activity and Layout
<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527506542/Hi_LowReadyToGo_qn4kok.png" width="400px">

### MainActivity.kt

```kotlin
package com.mcandroid.higherorlower

import android.content.Intent
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*
import org.jetbrains.anko.toast
import java.util.*

class MainActivity : AppCompatActivity() {

    var randomNum = 0

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        assignRandomNumber()
        createListenerForButton()
    }

    fun generateRandomNumber(): Int{
        var random = Random()
        var min = 1
        var max = 10
        randomNum = random.nextInt(max + 1 - min) + min

        return randomNum
    }

    fun assignRandomNumber(){
        randomNum = generateRandomNumber()
    }

    fun guessNumber(){
        var numberToGuess = randomNum
        var userGuess: Int = Integer.parseInt(numGuess.getText().toString())

        checkUserGuess(userGuess, numberToGuess)
    }

    fun checkUserGuess(userGuess: Int, numberToGuess: Int){
        if(userGuess > numberToGuess) {
            lblFeedback.setText("Lower")
        } else if (userGuess < numberToGuess) {
            lblFeedback.setText("Higher")
        } else {
            lblFeedback.setText("Correct!")
            openCorrectGuessScreen()
        }
    }

    fun createListenerForButton(){
        btnGuess.setOnClickListener{
            if(numGuess.getText().toString().equals("")){
                toast("Please enter a number")
            } else {
                guessNumber()
            }
        }
    }

    fun openCorrectGuessScreen(){
        val intent = Intent("com.mcandroid.higherorlower.CorrectGuessActivity")
        startActivity(intent)
    }
}

```
### res/layout/activty_main.xml


```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.mcandroid.higherorlower.MainActivity">

    <TextView
        android:id="@+id/lblFeedback"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Ready to go"
        android:textSize="30sp"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginStart="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginEnd="8dp"
        app:layout_constraintTop_toTopOf="parent"
        android:layout_marginTop="148dp"
        app:layout_constraintHorizontal_bias="0.502" />

    <Button
        android:id="@+id/btnGuess"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Guess"
        android:layout_marginBottom="24dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginStart="8dp"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginEnd="8dp" />

    <EditText
        android:id="@+id/numGuess"
        android:layout_width="121dp"
        android:layout_height="44dp"
        android:layout_marginBottom="16dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:ems="10"
        android:inputType="number"
        android:textAlignment="center"
        app:layout_constraintBottom_toTopOf="@+id/btnGuess"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.502"
        app:layout_constraintStart_toStartOf="parent" />
</android.support.constraint.ConstraintLayout>

```

## Add a Correct Guess Activity and Layout

### CorrectGuessActivity.kt
```java
package com.mcandroid.higherorlower

import android.content.Intent
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_correct_guess.*

class CorrectGuessActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_correct_guess)

        playAgain()
        exitGame()
    }

    fun playAgain(){
        btnPlayAgain.setOnClickListener{
            val intent = Intent("com.mcandroid.higherorlower.MainActivity")
            startActivity(intent)
        }
    }

    fun exitGame(){
        btnExit.setOnClickListener{
            this.finishAffinity()
        }
    }
}
```

### ActivityCorrectGuess.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.mcandroid.higherorlower.CorrectGuessActivity"
    android:background="#485ECE">

    <ImageView
        android:id="@+id/imgPartyPopper"
        android:layout_width="235dp"
        android:layout_height="299dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="80dp"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@drawable/party_popper_emoji" />

    <TextView
        android:id="@+id/lblCorrect"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Correct!"
        android:textColor="@android:color/background_light"
        android:textSize="30sp"
        android:layout_marginTop="20dp"
        app:layout_constraintTop_toBottomOf="@+id/imgPartyPopper"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_marginEnd="8dp"
        app:layout_constraintStart_toStartOf="parent"
        android:layout_marginStart="8dp" />

    <Button
        android:id="@+id/btnPlayAgain"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:text="Play Again"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/guideline"
        app:layout_constraintStart_toStartOf="parent" />

    <Button
        android:id="@+id/btnExit"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginBottom="8dp"
        android:layout_marginEnd="8dp"
        android:layout_marginStart="8dp"
        android:text="Exit"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="@+id/guideline" />

    <android.support.constraint.Guideline
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/guideline"
        android:orientation="vertical"
        app:layout_constraintGuide_percent="0.5" />
</android.support.constraint.ConstraintLayout>
```
<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527506543/Hi_LowCorrectGuess_mul3zy.png" width="400px">

### Add image to res/drawable

party_poper_emoji.png

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527508491/party_popper_emoji_szo1o3.png" width="300">

### Clearing the Number Entry Box on Focus
# How to  capture when a control has focus and clear the text
### https://stackoverflow.com/questions/10627137/how-can-i-know-when-an-edittext-loses-focus

val txtEdit = findViewById(R.id.edittxt) as EditText

        txtEdit.onFocusChangeListener = object : OnFocusChangeListener {
            fun onFocusChange(v: View, hasFocus: Boolean) {
                if (!hasFocus) {
                    // code to execute when EditText loses focus
                }
            }
        }



### AndroidManifest.xml
```java
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mcandroid.higherorlower">

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
        <activity
            android:name=".CorrectGuessActivity">
            <intent-filter>
                <action android:name="com.mcandroid.higherorlower.CorrectGuessActivity" />

                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

More to do:
Add a counter to keep track of number of guesses. Tell the user they got the answer in X guesses.
Track the number of games and how many guesses it takes for each. Give feedback to the user.

