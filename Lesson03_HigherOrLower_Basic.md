
# Lesson 03 - Higher or Lower
This is a basic version of Higher or Lower. Get it working then we'll expand on it to make it better and more user friendly.


### MainActivity.kt

```kotlin
package com.mcandroid.higherorlower

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

        //toast(randomNum)
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

    fun checkUserGuess(userGuess:Int, numberToGuess:Int){
        if(userGuess > numberToGuess){
            lblFeedback.setText("Lower")
        } else if (userGuess < numberToGuess) {
            lblFeedback.setText("Higher")
        } else {
            lblFeedback.setText("Correct!")
        }
    }

    fun createListenerForButton() {
        btnGuess.setOnClickListener{
            guessNumber()
        }
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
    tools:context=".MainActivity">

    <Button
        android:id="@+id/btnGuess"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="162dp"
        android:text="Guess"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/numGuess" />

    <TextView
        android:id="@+id/lblFeedback"
        android:layout_width="wrap_content"
        android:layout_height="0dp"
        android:layout_marginBottom="37dp"
        android:layout_marginTop="183dp"
        android:text="Let's get started!"
        app:layout_constraintBottom_toTopOf="@+id/numGuess"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/numGuess"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="22dp"
        android:ems="10"
        android:inputType="number"
        app:layout_constraintBottom_toTopOf="@+id/btnGuess"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/lblFeedback" />
</android.support.constraint.ConstraintLayout>

```

### Adding Toast

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




```python

```
