# GradleEMMAExample
An Illustration Example of How to use EMMA (Code Coverage Tool) in Gradle

### Emma

[EMMA](http://emma.sourceforge.net/) has been a popular Java code coverage tool, and has been widely used in research community.
However, it is non-trivial to configure EMMA in Gradle for Android Platform.
This repo serves as an illustration example of how to configure it in Gradle.

### Steps:

- Download emma.jar, and put it in SDK_PATH/tools/lib/.
- Copy 4 Java source files from EmmaInstrument package your project. (Don't forget to modify the parent class of InstrumentedActivity, and change the package name)
- Update AndroidManifest.xml. Specifically, (1) Declare the new Activity (InstrumentedActivity); (2) Declare receiver for intent **edu.gatech.m3.emma.COLLECT_COVERAGE**; (3) Declare <instrumentation/>; (4) Request for permission, i.e., WRITE_EXTERNAL_STORAGE, READ_EXTERNAL_STORAGE, INTERNET, ACCESS_NETWORK_STATE
- Add new buildTypes in build.gradle, and a new task `injectEmma`. Note that it is crucial to turn off debuggable. `injectEmma` instruments each .class files when JavaCompiler finishs compiles.
- Build APK

### Commands:

- Install apk `adb -s ${emulator_name} install -t ./debug/app-debug.apk`
- Launch App with instrumentation, `adb shell am instrument com.github.cetoolbox/com.github.cetoolbox.EmmaInstrument.EmmaInstrumentation`
- Manually or automatically test your app.
- Trigger event to collect coverage data, `adb shell am broadcast -a edu.gatech.m3.emma.COLLECT_COVERAGE`
- Pull Coverage data file, `adb pull /mnt/sdcard/coverage.ec ./`
- Generate coverage report, `java -cp ${emma.jar path}$ emma report -r [txt | html] -id coverage.ec,./out/coverage.em`
