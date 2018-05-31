

# Lesson 04 - BMI Calculator

For this project you will build a BMI (Body Mass Index) Calculator. BMI is a commonly used scale to determine how healthy a person is, depending upon both height and weight. You can calculate your BMI through the following formula:

```
(weight(kg)/height(m)/height(m)
```

Create a new Android application. Refer to Lesson 01 for detailed instructions. Here are some notes to get you started:
* Application Name: BMICalculator
* Continue using API 21: Lollipop
* Select an Empty Activity
* Leave the first activity called MainActivity

Add a line of code to the *dependencies* method of your application's ```build.gradle``` file for the Anko library.

``` java
Compile “org.jetbrains.anko:anko-commons: 0.10.4” 
```

Your build.gradle file should look something like mine shown below

### BMICalculatorKotlin/app/build.gradle

```java
apply plugin: 'com.android.application'

apply plugin: 'kotlin-android'

apply plugin: 'kotlin-android-extensions'

android {
    compileSdkVersion 22
    defaultConfig {
        applicationId "com.sterlingbusinessadvantage.bmicalculator"
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
    implementation 'org.jetbrains.anko:anko-commons:0.10.4'
    implementation fileTree(include: ['*.jar'], dir: 'libs')
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jre7:$kotlin_version"
    implementation 'com.android.support:appcompat-v7:27.1.1'
    implementation 'com.android.support.constraint:constraint-layout:1.1.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.1'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'
}

```


Give the project time to synch and download all the required libraries. Now you're ready to begin coding.

Open ```activity_main.xml``` in Design View which your find under the ```/res``` folder
 * Add two TextViews
 * Add two Number (Decimal) fields
 * Add a button
 
Name the text views lblWeight and lblHeight. Name the Number (Decimal) fields numWeight and numHeight. Name the button btnCalculateBMI. Remember, meaningful names help make your job as a programmer easier and allow the program to document itself.

Change the text elements so they read: "Enter your weight (kg):" and "Enter your height (m):" and the text on the button to read "Calculate BMI".



### Creating another activity
Right-click on the layout folder user res, click New | Activity | Empty Activity. Name this activity something like 'BMIResultsScreen'. Click finish.
* Add three TextView elements and two Buttons
* Name the top TextView 'lblYourBMI'
* Name the TextView below that 'lblBMIResult'
* Name the button in the top right corner 'btnExit'
* Name the button at the bottom 'btnCheckAgain'

Change the *textSize*, *textAlignment* and *textColor* of the middle TextView so it stands out from the rest. This will display your results.

Place constraints to make the elements on the form line up nicely.

### Make the new activity accessible from the main activity

We need to modify the 'AndroidMainfest.xml' file as shown below.  Copy the *intent filter* from the main activity, change the name to the second activity's package name, and the category to 'DEFAULT' rather than 'LAUNCHER'. Your mainifest file should look like this:

<!-- app/src/main/AndroidManifest.xml-->
```xml

<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.sterlingbusinessadvantage.bmicalculator">

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
            <action android:name="android.intent.action.BMIResultsScreen" />

            <category android:name="android.intent.category.DEFAULT" />
        </intent-filter>
        </activity>
    </application>

</manifest>

```

#### Edit your MainActivity.kt 
Make the file look as shown below

<!--BMICalculatorKotlin/app/src/main/java/com/sterlingbusinessadvantage/bmicalculator/MainActivity.kt-->
```kotlin

package com.sterlingbusinessadvantage.bmicalculator

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
        var weight: Double = (txtWeight.getText().toString().toDouble())
        var height: Double = (txtHeight.getText().toString().toDouble())

        return (weight / height / height)
    }

    private fun setOnClickListenerForButton(){
        btnCalculateBMI.setOnClickListener{
            val intent = Intent("com.sterlingbusinessadvantage.bmicalculator.BMIResultsScreen")
            intent.putExtra("BMIResult",calculateBMI())
            startActivity(intent)
        }
    }
}


```

### Edit your BMIResultsScreen.kt

Make the file look as shown below

<!--BMICalculatorKotlin/app/src/main/java/com/sterlingbusinessadvantage/bmicalculator/BMIResultsScreen.kt-->
```kotlin
package com.sterlingbusinessadvantage.bmicalculator

import android.content.Intent
import android.support.v7.app.AppCompatActivity
import android.os.Bundle
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

    private fun showBMIResult(){
        var decimalFormat = DecimalFormat("#.#")
        var formattedBMI = decimalFormat.format(getIntent().getExtras().getDouble("BMIResult"))
        lblBMIResult.setText(formattedBMI.toString())
    }

    private fun findBMICategory(){
        var categoryOfBMI = "Unknown"
        var resultBMI = getIntent().getExtras().getDouble("BMIResult")

        if (resultBMI < 15){
            categoryOfBMI = "Very Severely Underweight"
        } else if (resultBMI in 15 .. 16) {
            categoryOfBMI  = "Severely Underweight"
        } else if (resultBMI > 16 && resultBMI <= 18.5){
            categoryOfBMI = "Underweight"
        } else if (resultBMI > 18.5 && resultBMI <= 25){
            categoryOfBMI = "Normal (Healthy Weight)"
        } else if (resultBMI in 25 .. 30){
            categoryOfBMI = "Overweight"
        } else if (resultBMI in 30 .. 35){
            categoryOfBMI = "Moderately Obese"
        } else if (resultBMI in 35 .. 40) {
            categoryOfBMI = "Severely Obese"
        }else if (resultBMI >= 40){
            categoryOfBMI = "Very Severely Obese"
        }

        lblBMIResultCategory.setText(categoryOfBMI)
    }

    private fun setExitListener(){
        btnCloseResults.setOnClickListener {
            this.finishAffinity()
        }
    }

    private fun setCheckAgainListener(){
        btnCheckAgain.setOnClickListener(){
            val intent = Intent("com.example.sterlingbusinessadvantage.bmicalculator.MainActivity")
            startActivity(intent)
        }
    }


}

```


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
        app:layout_constraintEnd_toStartOf="@+id/txtWeight"
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
        app:layout_constraintEnd_toStartOf="@+id/txtHeight"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

    <EditText
        android:id="@+id/txtWeight"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="47dp"
        android:ems="10"
        android:inputType="number"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/textView"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/txtHeight"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="15dp"
        android:ems="10"
        android:inputType="number"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toEndOf="@+id/textView2"
        app:layout_constraintTop_toBottomOf="@+id/txtWeight" />

    <Button
        android:id="@+id/btnCalculateBMI"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="7dp"
        android:layout_marginTop="47dp"
        android:text="Calculate BMI"
        app:layout_constraintStart_toStartOf="@+id/txtHeight"
        app:layout_constraintTop_toBottomOf="@+id/txtHeight" />

</android.support.constraint.ConstraintLayout>

```
### BMICalculatorKotlin/BMICalculatorKotlin.iml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<module external.linked.project.id="BMICalculatorKotlin" external.linked.project.path="$MODULE_DIR$" external.root.project.path="$MODULE_DIR$" external.system.id="GRADLE" type="JAVA_MODULE" version="4">
  <component name="FacetManager">
    <facet type="java-gradle" name="Java-Gradle">
      <configuration>
        <option name="BUILD_FOLDER_PATH" value="$MODULE_DIR$/build" />
        <option name="BUILDABLE" value="false" />
      </configuration>
    </facet>
  </component>
  <component name="NewModuleRootManager" LANGUAGE_LEVEL="JDK_1_7" inherit-compiler-output="true">
    <exclude-output />
    <content url="file://$MODULE_DIR$">
      <excludeFolder url="file://$MODULE_DIR$/.gradle" />
    </content>
    <orderEntry type="inheritedJdk" />
    <orderEntry type="sourceFolder" forTests="false" />
  </component>
</module>

```

### BMICalculatorKotlin/app/app.iml

```xml

<?xml version="1.0" encoding="UTF-8"?>
<module external.linked.project.id=":app" external.linked.project.path="$MODULE_DIR$" external.root.project.path="$MODULE_DIR$/.." external.system.id="GRADLE" type="JAVA_MODULE" version="4">
  <component name="FacetManager">
    <facet type="android-gradle" name="Android-Gradle">
      <configuration>
        <option name="GRADLE_PROJECT_PATH" value=":app" />
      </configuration>
    </facet>
    <facet type="android" name="Android">
      <configuration>
        <option name="SELECTED_BUILD_VARIANT" value="debug" />
        <option name="ASSEMBLE_TASK_NAME" value="assembleDebug" />
        <option name="COMPILE_JAVA_TASK_NAME" value="compileDebugSources" />
        <afterSyncTasks>
          <task>generateDebugSources</task>
        </afterSyncTasks>
        <option name="ALLOW_USER_CONFIGURATION" value="false" />
        <option name="MANIFEST_FILE_RELATIVE_PATH" value="/src/main/AndroidManifest.xml" />
        <option name="RES_FOLDER_RELATIVE_PATH" value="/src/main/res" />
        <option name="RES_FOLDERS_RELATIVE_PATH" value="file://$MODULE_DIR$/src/main/res" />
        <option name="ASSETS_FOLDER_RELATIVE_PATH" value="/src/main/assets" />
      </configuration>
    </facet>
    <facet type="kotlin-language" name="Kotlin">
      <configuration version="3" platform="JVM 1.8" useProjectSettings="false">
        <compilerSettings />
        <compilerArguments>
          <option name="destination" value="$MODULE_DIR$/build/tmp/kotlin-classes/debug" />
          <option name="noStdlib" value="true" />
          <option name="noReflect" value="true" />
          <option name="moduleName" value="app_debug" />
          <option name="jvmTarget" value="1.8" />
          <option name="addCompilerBuiltIns" value="true" />
          <option name="loadBuiltInsFromDependencies" value="true" />
          <option name="languageVersion" value="1.2" />
          <option name="apiVersion" value="1.2" />
          <option name="pluginOptions">
            <array>
              <option value="plugin:org.jetbrains.kotlin.android:experimental=false" />
              <option value="plugin:org.jetbrains.kotlin.android:enabled=true" />
              <option value="plugin:org.jetbrains.kotlin.android:defaultCacheImplementation=hashMap" />
            </array>
          </option>
          <option name="pluginClasspaths">
            <array>
              <option value="$USER_HOME$/.gradle/caches/modules-2/files-2.1/org.jetbrains.kotlin/kotlin-android-extensions/1.2.30/680ff8d567db8dec157cd9344f0a1d2b5e4d651b/kotlin-android-extensions-1.2.30.jar" />
            </array>
          </option>
        </compilerArguments>
      </configuration>
    </facet>
  </component>
  <component name="NewModuleRootManager" LANGUAGE_LEVEL="JDK_1_7">
    <output url="file://$MODULE_DIR$/build/intermediates/classes/debug" />
    <output-test url="file://$MODULE_DIR$/build/intermediates/classes/test/debug" />
    <exclude-output />
    <content url="file://$MODULE_DIR$">
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/apt/debug" isTestSource="false" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/r/debug" isTestSource="false" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/aidl/debug" isTestSource="false" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/buildConfig/debug" isTestSource="false" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/rs/debug" isTestSource="false" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/res/rs/debug" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/res/resValues/debug" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/apt/androidTest/debug" isTestSource="true" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/r/androidTest/debug" isTestSource="true" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/aidl/androidTest/debug" isTestSource="true" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/buildConfig/androidTest/debug" isTestSource="true" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/rs/androidTest/debug" isTestSource="true" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/res/rs/androidTest/debug" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/res/resValues/androidTest/debug" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/build/generated/source/apt/test/debug" isTestSource="true" generated="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/debug/res" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/debug/resources" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/debug/assets" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/debug/aidl" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/debug/java" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/debug/rs" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/debug/shaders" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTestDebug/res" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTestDebug/resources" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTestDebug/assets" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTestDebug/aidl" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTestDebug/java" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTestDebug/rs" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTestDebug/shaders" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/testDebug/res" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/testDebug/resources" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/testDebug/assets" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/testDebug/aidl" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/testDebug/java" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/testDebug/rs" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/testDebug/shaders" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/main/res" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/main/resources" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/main/assets" type="java-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/main/aidl" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/main/java" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/main/rs" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/main/shaders" isTestSource="false" />
      <sourceFolder url="file://$MODULE_DIR$/src/test/res" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/test/resources" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/test/assets" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/test/aidl" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/test/java" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/test/rs" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/test/shaders" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTest/res" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTest/resources" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTest/assets" type="java-test-resource" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTest/aidl" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTest/java" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTest/rs" isTestSource="true" />
      <sourceFolder url="file://$MODULE_DIR$/src/androidTest/shaders" isTestSource="true" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/blame" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/check-manifest" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/incremental" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/manifests" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/prebuild" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/res" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/rs" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/splits-support" />
      <excludeFolder url="file://$MODULE_DIR$/build/intermediates/symbols" />
      <excludeFolder url="file://$MODULE_DIR$/build/outputs" />
    </content>
    <orderEntry type="jdk" jdkName="Android API 22 Platform" jdkType="Android SDK" />
    <orderEntry type="sourceFolder" forTests="false" />
    <orderEntry type="library" scope="TEST" name="Gradle: com.android.support.test:runner-1.0.1" level="project" />
    <orderEntry type="library" name="Gradle: android.arch.lifecycle:common:1.1.0@jar" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:support-annotations:27.1.1@jar" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:animated-vector-drawable-27.1.1" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:support-compat-27.1.1" level="project" />
    <orderEntry type="library" name="Gradle: android.arch.lifecycle:viewmodel-1.1.0" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support.constraint:constraint-layout-solver:1.1.0@jar" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: com.squareup:javawriter:2.1.1@jar" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:support-vector-drawable-27.1.1" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:support-core-ui-27.1.1" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:support-core-utils-27.1.1" level="project" />
    <orderEntry type="library" name="Gradle: org.jetbrains:annotations:13.0@jar" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: com.google.code.findbugs:jsr305:2.0.1@jar" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: com.android.support.test.espresso:espresso-core-3.0.1" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: com.android.support.test:rules-1.0.1" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: javax.inject:javax.inject:1@jar" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:support-fragment-27.1.1" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support.constraint:constraint-layout-1.1.0" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: junit:junit:4.12@jar" level="project" />
    <orderEntry type="library" name="Gradle: android.arch.core:runtime-1.1.0" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: org.hamcrest:hamcrest-core:1.3@jar" level="project" />
    <orderEntry type="library" name="Gradle: com.android.support:appcompat-v7-27.1.1" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: com.android.support.test.espresso:espresso-idling-resource-3.0.1" level="project" />
    <orderEntry type="library" name="Gradle: org.jetbrains.anko:anko-commons:0.10.4@jar" level="project" />
    <orderEntry type="library" name="Gradle: org.jetbrains.kotlin:kotlin-stdlib:1.2.30@jar" level="project" />
    <orderEntry type="library" name="Gradle: android.arch.lifecycle:livedata-core-1.1.0" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: org.hamcrest:hamcrest-library:1.3@jar" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: org.hamcrest:hamcrest-integration:1.3@jar" level="project" />
    <orderEntry type="library" name="Gradle: android.arch.core:common:1.1.0@jar" level="project" />
    <orderEntry type="library" name="Gradle: org.jetbrains.kotlin:kotlin-stdlib-jre7:1.2.30@jar" level="project" />
    <orderEntry type="library" scope="TEST" name="Gradle: net.sf.kxml:kxml2:2.3.0@jar" level="project" />
    <orderEntry type="library" name="Gradle: android.arch.lifecycle:runtime-1.1.0" level="project" />
  </component>
</module>
```



```python

```
