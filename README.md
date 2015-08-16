# Index
android build system mirror https://android.googlesource.com/platform/tools/base  

and just code, if you want to import. please see: http://tools.android.com/build

## Build system
build-system code at:  
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
