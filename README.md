# GradleEMMAExample
An Illustration Example of How to use EMMA (Code Coverage Tool) in Gradle

### Emma

[EMMA](http://emma.sourceforge.net/) has been a popular Java code coverage tool, and has been widely used in research community.
However, it is non-trivial to configure EMMA in Gradle for Android Platform.
This repo serves as an illustration example of how to configure it in Gradle.

### Steps:

1. Download emma.jar, and put it in SDK_PATH/tools/lib/.
2. Copy 4 Java source files from EmmaInstrument package your project. (Don't forget to modify the parent class of InstrumentedActivity, and change the package name)
3. Update AndroidManifest.xml. (Refers to AndroidManifest.xml in the example)
   1. Declare the new Activity (InstrumentedActivity);
   2. Declare receiver for intent **edu.gatech.m3.emma.COLLECT_COVERAGE**;
   3. Declare instrumentation;
   4. Request for permission, i.e., WRITE_EXTERNAL_STORAGE, READ_EXTERNAL_STORAGE, INTERNET, ACCESS_NETWORK_STATE
4. Add new buildTypes in build.gradle, and a new task `injectEmma`. Note that it is crucial to turn off debuggable. `injectEmma` instruments each .class files when JavaCompiler finishs compiles.
5. Build APK

### Commands:

1. Install apk `adb -s ${emulator_name} install -t ./debug/app-debug.apk`
2. Launch App with instrumentation, `adb shell am instrument com.github.cetoolbox/com.github.cetoolbox.EmmaInstrument.EmmaInstrumentation`
3. Manually or automatically test your app.
4. Trigger event to collect coverage data, `adb shell am broadcast -a edu.gatech.m3.emma.COLLECT_COVERAGE`
5. Pull Coverage data file, `adb pull /mnt/sdcard/coverage.ec ./`
6. Generate coverage report, `java -cp ${emma.jar path}$ emma report -r [txt | html] -id coverage.ec,./out/coverage.em`
