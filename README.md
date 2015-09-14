# Index
android build system mirror https://android.googlesource.com/platform/tools/base  

and just code, if you want to import. please see: http://tools.android.com/build

## Build system
new build system code at:  
-- root  
---- build-system

## Sync
sync mirror code period: 1 week

## Release notes
__1.4.0-beta__ (2015/08/24)  
* Instead of processing java resources during the packaging of the APK,
moved this upfront before the obfuscation tasks. This will allow
the obfuscation tasks to have a chance to adapt the java resources
following packages obfuscation
* made java resources extraction from libraries incremental tasks.
* Fixed issue with using jni code in experimental library plugin.
* Allow platform version to be set separately from compileSdkVersion in experimental plugin.
* Prevent a consumer of a library removing a resource from that library, which would lead to a runtime NoSuchFieldError.
* Allow a comma-separated list of serials in ANDROID_SERIAL when installing or running tests
* Fix installation failure on L+ devices when the APK name contains a space.
* Fix various issues related to AAPT error output.
* Vector drawable support for generating PNGs at build time.
    * PNGs are generated for every vector drawable found in a resource directory that does not specify an API version (or specifies a version lower than 21).
    * This only happens if minSdk is below 21.
    * Densities to use can be set using the new "generatedDensities" property in defaultConfig or per-flavor.
* Multiple modules (e.g. app and lib) now share the same mockable android.jar (for unit testing) which is generated only once. Delete $rootDir/build to regenerate it.

__1.3.1__ (2015/08/11)  
* fixed issue when ZipAlign task would not consume previous' task output when it the file name is customized.
* fixed packaging of Renderscript with NDK
* Keep the createDebugCoverageReport task name.
* Fix customized archiveBaseName handling : see http://b.android.com/182016
* fixed for http://b.android.com/182433

__1.3.0__ (2015/07/30)
* added experimental-plugin with ndk support.
* see http://tools.android.com/tech-docs/new-build-system/gradle-experimental
* When building aar, do not provide automatic @{applicationId} placeholder in manifest merger.
* Introduce support for incremental compilation support with Jill and Jack.
* By default, "LICENSE" and "LICENSE.txt" are excluded when creating an APK.
* This can be changed from the DSL:
``` groovy
android {
    packagingOptions.excludes = []
}
```
* New sourceSets task for inspecting the set of all available source sets.
* Unit tests recognize multi-flavor and per-variant source folders (e.g.testDemoDebug). Android tests recognized multi-flavor source folders.
* Unit testing improvements
  * Run javac on main and test sources, even if useJack is true.
  * Correctly recognize per-build-type dependencies.
* Added support for controlling the thread pool size for android tasks on the command line or in the gradle.properties file. The following example sets this to 4:
``` groovy
./gradlew <tasks> -Pcom.android.build.threadPoolSize=4
```
* It's now possible to specify instrumentation test runner arguments in build.gradle (in defaultConfig or per flavor):
``` groovy
android {
    defaultConfig {
        testInstrumentationRunnerArguments size: "medium"
    }
}
```
This can also be done on the command line:
``` groovy
./gradlew cC -Pandroid.testInstrumentationRunnerArguments.size=medium
```

__1.2.2__ (2015/04/28)  
* More fixes to the dependency management to speed things up.
* Fix for https://code.google.com/p/android/issues/detail?id=152811

__1.2.1__ (2015/04/27)
* Added support for 280dpi resources.

__1.2.0__ (2015/04/26)
* More fixes to the dependency management to speed things up.
* Added Enable flag to splits configuration for language.
* Upgraded Proguard to 5.1
* DSL: new code block for configuring the test tasks, see Unit testing support.
* Unit testing:
   * Java-style resources are put on the classpath.
   * Final modifier is stripped from public instance fields in the platform jar.

* Test-only ProGuard files. When running instrumentation tests (i.e. connectedCheck) against a minified variant, the test APK needs to be processed by ProGuard to  
   rename references to code in the main APK. Flags for this ProGuard run (mostly for silencing warnings) can now be specified like this:
   ``` groovy
   android.buildType.minified.testProguardFile "test-proguard-rules.pro"
   ```

__1.2.0-beta1__ (2015/03/25)
* Mainly a bug fixing release with very few new features.
* Better support for unit tests (libraries can now have unit tests, etc...)
* Added timeOut option to adbOptions to set the timeout when installing APKs on devices.
* Performance improvements during project evaluation.

__1.1.3__ (2015/03/06)
* Fixed issue with duplicated dependencies on test app that triggered a Proguard failure with duplicate inclusion exception.
* Fix Comparator implementation which did not comply with JDK Comparator contract, was flagged by JDK7 as an error.

__1.1.2__ (2015/02/26)
* Path is normalized when creating mockable jar for unit testing.
* Fixed an issue where "archivesBaseName" setting in build.gradle was ignored.
* Unresolved placeholder failure in manifest merger when building library test application.

__1.1.1__ (2015/02/24)
* Only variants that package a Wear app will now trigger building them.
* Dependency related issues now fail at build time rather than at debug time. This is to allow running diagnostic tasks (such as 'dependencies') to help resolve the conflict.
* Calling android.getBootClasspath() is now possible again.

__1.1.0__ (2015/02/17)
* Unit testing support. Unit testing code is run on the local JVM, against a special version of android.jar that is compatible with popular mocking frameworks (e.g. Mockito).
  * New tasks: test, testDebug/testRelease, testMyFlavorDebug (when using flavors).
  * New source folders recognized as unit tests:
    src/test/java, src/testDebug/java, src/testMyFlavor/java etc.
  * New configurations for adding test-only dependencies, e.g.
    testCompile 'junit:junit:4.11'
    testMyFlavorCompile 'some:library:1.0'
  * Not compatible with Jack at the moment.
  * New option, android.testOptions.unitTests.returnDefaultValues to control the behaviour of the "mockable" android.jar.
* Task names that used to contain 'Test', e.g. 'assembleDebugTest' now use 'AndroidTest', e.g. 'assembleDebugAndroidTest'. This is to distinguish them from the unit test tasks, e.g. 'assembleDebugUnitTest'.
* ProGuard configuration files are no longer applied to the test APK. If minification is enabled, the test APK will be processed by ProGuard only to apply the mapping file generated when minifying the main APK.
* Fixes and changes to the dependency management:
    * Properly handle 'provided' and 'package' scopes to do what they should be doing.
    * 'provided' and 'package' cannot be used with Android Libraries, and will generate an error
    * sync tested and test dependency trees:
        * if the same version of an artifact is present in both, it'll get skipped in the test app.
        * if the version is different it'll generate a build error. Gradle provides mechanism to resolve this.
* Added support for anyDpi resource qualifier in the resource merger.
* Projects with large number of Android Modules will see evaluation and IDE sync speed improvements (YMMV)

__1.0.1__ (2014/1/9)
* Fix [81638](https://code.google.com/p/android/issues/detail?id=81638) : Fix PermGen issue when running extractAnnotations.
* Fix small manifest merger issues when importing libraries with targetSdkVersion < 16
* Fix density ordering when running with JDK8.
* Fix [82662](https://code.google.com/p/android/issues/detail?id=82662) : Disable passing --no-optimize to dx.

__1.0.0-rc4__ (2014/12/04)
* Handle local jars in tests of multidex-enabled library projects
* Fix path normalization (for unzipped aars) when path is only 1 char long
* Fix Jack/Jill memory setting through dexOptions.javaMaxHeapSize

__1.0.0-rc3__ (2014/12/03)
* Better handling of destination paths to unarchive AARs based on groupId/artifactId/version that contain characters not valid in folder names.

__1.0.0-rc2__ (2014/12/03)
* Enhanced manifest merger logging by specifying library coordinates.
* Allow manifest placeholder to be of any type as long as toString() is implemented.
* Fixed issue where a library with a low targetSdk would add permissions due to a declared permission in a different manifest.
* Better fix for issue where embedding a micro app could add new permissions to the main app manifest.
* Added check for conflict between density splits and resConfig property.
* test applications are now not using multi-dexing, unless they test a library project.
* Fixed lint issues [80872](https://code.google.com/p/android/issues/detail?id=80872), [80834](https://code.google.com/p/android/issues/detail?id=80834), [60416](https://code.google.com/p/android/issues/detail?id=60416), [80837](https://code.google.com/p/android/issues/detail?id=80837)

__1.0.0-rc1__ (2014/11/24)  
(OK this is the real RC1)
* Fixed issue in resources shrinking
* Fixed issue in publishNonDefault
* Install task on 21+ devices now does a reinstall again.
* Density split using aapt 21+ now use --preferred-density allowing for missing density version of some bitmaps.
hasProperty() will now work again on read-only wrapper returned by the variant API.
* Setting applicationId(Suffix) in a Library project will now properly fail.
* Fixed issue where embedding a micro app could add new permissions to the main app manifest.
