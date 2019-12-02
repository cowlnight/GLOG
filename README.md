GLOG Note
===


## Table of Contents

[TOC]

## Needs

When I am working on a QT project , a program for designing the pattern users need, it will have some bugs that I can't figure out which line or which function cause. So I want to add some logging in my program.
What I need is a **simple, lite and fast** logging tool. Then I choose GLOG from google. Of course, others may recommand more useful and more powerful tools like BOOST, but that is not suit me.

## Environment

OS: Windows 10
Visual Studio 2017

How to compile GLOG for 64 bit OS Visual Studio project？
---
I failed a lot of times, again. Although the offical document says it is header only module, it needs to be compiled when it's working on 64 bit system. So we will get its static library after compilation. 
Finally I find the way to compile it sucessfully. 

**Offical website:** [GLOG Github](https://github.com/google/glog) 
**Tools:** CMake GUI -- [Windows win64-x64 ZIP](https://cmake.org/download//)

### Compilation on Windows:
1. If you don't have cmake, please install the latest version of cmake.
2. Open cmake-gui.exe, and use it to generate VS solution.
    *  At the top of it, fill in the *source code folder path*(like D:/glog-0.3.5) and *where to build* (like D:/glog-0.3.5/build). The build path is where the whole VS solution files will be generated to later.
    *  Click "Configure" button, and it will let you select your platform. Be sure to use the right one.
    Like this:
    ![](https://i.imgur.com/7XQkPej.png)
    * After configration, a list of options will be listed and probably marked as red. Don't worry about the red. Just go through them, and modify those you want to.
    * (For glog only) You may need to specify gflags path in Ungrouped Entries/gflags_DIR or disable gflags by unchecking WITH/WITH_GFLAGS. Also, modify CMAKE/CMAKE_INSTALL_PREFIX to a non-system folder, otherwise you will need administrative privileges to run INSTALL project.
    * If you modified any items, press "Configure" again to make it work.
    * When everything is configured right, press "Generate" to generate VS solution. Then you can open it by clicking "Open Project".
3. In the VS project, select the configuration you want to build ( x64/win32, Release/Debug, etc), then build ALL_BUILD. Then you can build INSTALL, this will copy the headers and libraries to CMAKE/CMAKE_INSTALL_PREFIX folder for a new project to use.


How to use GLOG in 64 bit OS Visual Studio project？
---
1. Put the library to your project.(Debug：glogd.lib / Release：glog.lib)
2. Put the `build/glog` to the `include path` in your project. (e.g. log_severity.h, logging.h, raw_logging.h, stl_logging.h, vlog_is_on.h)
3. When you include logging.h and build the project, you may meet some errors.Like this:
```
glog\log_severity.h(55): fatal error C1189: #error :  ERROR macro is defined. Define GLOG_NO_ABBREVIATED_SEVERITIES before including logging.h. See the document for detail.
```
    
**Solution:** You can fix the problem by adding `GLOG_NO_ABBREVIATED_SEVERITIES` in project properties.( **C++ ->Preprocessor ->Preprocessor Definitions** )
    
4. Other error like this you may meet:
```
error LNK2019: unresolved external symbol "__declspec(dllimport) void __cdecl google::InitGoogleLogging(char const *)" (__imp_?InitGoogleLogging@google@@YAXPEBD@Z)，referenced in function main
```

**Solution:**  Add `GOOGLE_GLOG_DLL_DECL=` in project properties.( **C++ ->Preprocessor ->Preprocessor Definitions** )
  
![](https://i.imgur.com/9PICnaH.png)

5. Finally you will get a logging file like this:

```
Log file created at: 2019/12/02 11:47:33
Running on machine: DESKTOP-C0459JH
Log line format: [IWEF]mmdd hh:mm:ss.uuuuuu threadid file:line] msg
```

Use Example
---
* Initialization
```
google::InitGoogleLogging((const char *)argv[0]);  //Inital (filename)
google::SetLogDestination(google::GLOG_INFO,"./myInfo");
// save path
```
* Type
Serious level: FATAL>ERROR>WARNING>INFO
```
LOG(INFO) << "I am INFO!";          //0
LOG(WARNING) << "I am WARNING!";    //1
LOG(ERROR) << "I am ERROR!";        //2
LOG(FATAL) << "I am FATAL!";        //3
```
---
Reference:
> https://www.cnblogs.com/21207-iHome/p/9297329.html
> https://stackoverflow.com/questions/41766969/installing-glog-on-windows
> https://blog.csdn.net/longlong530/article/details/9340803?fbclid=IwAR3-2Ft6HUzMX1PyZdf6YdMWIZcdErgZrWbEIeHnXlAjrF6zevYm11Dctio


###### tags: `glog` `logging` `visual studio`
