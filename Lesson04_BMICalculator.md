

# Lesson 04 - BMI Calculator

For this project you will build a BMI (Body Mass Index) Calculator. BMI is a commonly used scale to determine how healthy a person is, depending upon both height and weight. You can calculate your BMI through the following formula:

```
BMI = weight(kg)/height(m)/height(m)
```

Create a new Android application. Refer to Lesson 01 for detailed instructions. Here are some notes to get you started:
* Application Name: BMICalculator
* Continue using API 21: Lollipop
* Select an Empty Activity
* Leave the first activity called MainActivity

Add a line of code to the *dependencies* method of your application's ```build.gradle``` file for the Anko library.

``` java
implementation "org.jetbrains.anko:anko-commons:0.10.5" 
```

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527472006/Build.gradle_location_keotac.png" width="450px">


> Pro Tip: Sync your project and run it now. 
> It should run without any errors. If not, start over or fix the issue.

You can find the latest version of anko commons from their GitHub repository at https://github.com/kotlin/anko

Your build.gradle file should look something like mine shown below

### BMICalculatorKotlin/app/build.gradle

```java
apply plugin: 'com.android.application'

apply plugin: 'kotlin-android'

apply plugin: 'kotlin-android-extensions'

android {
    compileSdkVersion 27
    defaultConfig {
        applicationId "com.mcandroid.bmicalculator"
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


Give the project time to synch and download all the required libraries. Now you're ready to begin coding.

Open ```activity_main.xml``` in Design View which your find under the ```/res``` folder
 * Add two TextViews
 * Add two Number (Decimal) fields
 * Add a button
 
Name the text views lblWeight and lblHeight. Name the Number (Decimal) fields numWeight and numHeight. Name the button btnCalculateBMI. Remember, meaningful names help make your job as a programmer easier and allow the program to document itself.

Change the text elements so they read: "Enter your weight (kg):" and "Enter your height (m):" and the text on the button to read "Calculate BMI".

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527472908/BMICalcDataEntry_t9u1cf.png" width="400px">


### BMICalculatorKotlin/app/src/main/res/layout/activity_main.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="23dp"
        android:layout_marginStart="4dp"
        android:layout_marginTop="57dp"
        android:text="Enter your weight (kg):"
        app:layout_constraintEnd_toStartOf="@+id/numWeight"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginEnd="23dp"
        android:layout_marginStart="4dp"
        android:layout_marginTop="32dp"
        android:text="Enter your height (m):"
        app:layout_constraintEnd_toStartOf="@+id/numHeight"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

    <EditText
        android:id="@+id/numWeight"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="47dp"
        android:ems="10"
        android:inputType="numberDecimal"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/textView"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/numHeight"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="15dp"
        android:accessibilityLiveRegion="polite"
        android:ems="10"
        android:inputType="numberDecimal"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/textView2"
        app:layout_constraintTop_toBottomOf="@+id/numWeight" />

    <Button
        android:id="@+id/btnCalculateBMI"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="7dp"
        android:layout_marginTop="47dp"
        android:text="Calculate BMI"
        app:layout_constraintStart_toStartOf="@+id/numHeight"
        app:layout_constraintTop_toBottomOf="@+id/numHeight" />

</android.support.constraint.ConstraintLayout>

```
#### Edit your MainActivity.kt 

Make the file look as shown below

<!--BMICalculatorKotlin/app/src/main/java/com/sterlingbusinessadvantage/bmicalculator/MainActivity.kt-->
```kotlin

package com.mcandroid.bmicalculator

import android.content.Intent
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import kotlinx.android.synthetic.main.activity_main.*

class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        setOnClickListenerForButton()
    }

    private fun calculateBMI():Double{
        var weight: Double = (numWeight.getText().toString().toDouble())
        var height: Double = (numHeight.getText().toString().toDouble())

        return (weight / height / height)
    }

    private fun setOnClickListenerForButton(){
        btnCalculateBMI.setOnClickListener{
            val intent = Intent("com.mcandroid.bmicalculator.BMIResultsScreen")
            intent.putExtra("BMIResult",calculateBMI())
            startActivity(intent)
        }
    }
}

```

### Creating another activity
Right-click on the layout folder user res, click New | Activity | Empty Activity. Name this activity something like 'BMIResultsScreen'. Click finish.

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527473453/BMIRightClickAddActivity_fbi44v.png" width="400px">

* Add three TextView elements and two Buttons
* Name the top TextView 'lblYourBMI'
* Name the TextView below that 'lblBMIResult'
* Name the button in the top right corner 'btnExit'
* Name the button at the bottom 'btnCheckAgain'

Change the *textSize*, *textAlignment* and *textColor* of the middle TextView so it stands out from the rest. This will display your results.

Place constraints to make the elements on the form line up nicely.

<img src="http://res.cloudinary.com/dkoqxjnsr/image/upload/v1527473448/BMIResultScreen_fjlhnz.png" width="400px">


### BMICalculatorKotlin/app/src/main/res/layout/activity_bmiresults_screen.xml
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".BMIResultsScreen">

    <TextView
        android:id="@+id/lblYourBMI"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="41dp"
        android:layout_marginStart="1dp"
        android:layout_marginTop="136dp"
        android:text="Your BMI is ..."
        app:layout_constraintBottom_toTopOf="@+id/lblBMIResult"
        app:layout_constraintStart_toStartOf="@+id/lblBMIResult"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/lblBMIResult"
        android:layout_width="61dp"
        android:layout_height="0dp"
        android:layout_marginBottom="17dp"
        android:text="0"
        android:textAlignment="center"
        android:textAllCaps="false"
        android:textAppearance="@android:style/TextAppearance.Large"
        app:layout_constraintBottom_toTopOf="@+id/lblBMIResultCategory"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/lblYourBMI" />

    <TextView
        android:id="@+id/lblBMIResultCategory"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="135dp"
        android:layout_marginStart="1dp"
        android:text="Unknown"
        app:layout_constraintBottom_toTopOf="@+id/btnCheckAgain"
        app:layout_constraintStart_toStartOf="@+id/lblBMIResult"
        app:layout_constraintTop_toBottomOf="@+id/lblBMIResult" />

    <Button
        android:id="@+id/btnCheckAgain"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="51dp"
        android:layout_marginEnd="121dp"
        android:text="Check Again"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/lblBMIResultCategory" />

    <Button
        android:id="@+id/btnCloseResults"
        android:layout_width="42dp"
        android:layout_height="43dp"
        android:layout_marginEnd="7dp"
        android:text="X"
        android:textAlignment="center"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</android.support.constraint.ConstraintLayout>

```


### Edit your BMIResultsScreen.kt
Make the file look as shown below



<!--BMICalculatorKotlin/app/src/main/java/com/mcandroid/bmicalculator/BMIResultsScreen.kt-->
```kotlin
package com.mcandroid.bmicalculator

import android.content.Intent
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
import com.mcandroid.bmicalculator.R
import com.mcandroid.bmicalculator.R.id.btnCloseResults
import kotlinx.android.synthetic.main.activity_bmiresults_screen.*
import java.text.DecimalFormat



class BMIResultsScreen : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_bmiresults_screen)

        showBMIResult()
        findBMICategory()

        setExitListener()
        setCheckAgainListener()
    }

    fun showBMIResult(){
        var decFormat = DecimalFormat("#.#")
        var formattedBMI = decFormat.format(getIntent().getExtras().getDouble("BMIResult"))
        lblBMIResult.setText(formattedBMI.toString())
    }

    fun findBMICategory(){
        var categoryOfBMI = "Unknown"
        var resultBMI = getIntent().getExtras().getDouble("BMIResult")

        if(resultBMI < 15){
            categoryOfBMI = "Very Severely Underweight"
        } else if(resultBMI in 15..16){
            categoryOfBMI = "Severely Underweight"
        } else if(resultBMI > 16 && resultBMI <= 18.5){
            categoryOfBMI = "Underweight"
        } else if(resultBMI > 18.5 && resultBMI <= 25){
            categoryOfBMI = "Normal (Healthy Weight)"
        } else if(resultBMI in 25..30){
            categoryOfBMI = "Overweight"
        } else if(resultBMI in 30..35){
            categoryOfBMI = "Moderately Obese"
        } else if(resultBMI in 35..40){
            categoryOfBMI = "Severely Obese"
        } else if(resultBMI >= 40){
            categoryOfBMI = "Very Severely Obese"
        }

        lblBMIResultCategory.setText(categoryOfBMI)
    }

    fun setExitListener(){
        btnCloseResults.setOnClickListener {
            this.finishAffinity()
        }
    }

    fun setCheckAgainListener(){
        btnCheckAgain.setOnClickListener{
            val intent = Intent("com.mcandroid.bmicalculator.MainActivity")
            startActivity(intent)
        }
    }
}
```

### Make the new activity accessible from the main activity

We need to modify the 'AndroidMainfest.xml' file as shown below.  Copy the *intent filter* from the main activity, change the name to the second activity's package name, and the category to 'DEFAULT' rather than 'LAUNCHER'. Your mainifest file should look like this:

<!-- app/src/main/AndroidManifest.xml-->
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.mcandroid.bmicalculator">

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
        <activity android:name=".BMIResultsScreen">
            <intent-filter>
                <action android:name="com.mcandroid.bmicalculator.BMIResultsScreen" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```





```python

```
