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
