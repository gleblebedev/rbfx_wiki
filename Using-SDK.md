If you wish to maintain engine code outside of your project you may build engine SDK. SDK is created by building install target:

1. `cmake -DCMAKE_INSTALL_PREFIX=/path/to/SDK /path/to/code/repository`
2. `cmake --build . --target install`

Now `/path/to/SDK` contains a newly built engine SDK. In order to use SDK in your project import engine target in your CMake project:

```cmake
include(/path/to/SDK/share/CMake/Urho3D.cmake)
add_executable(YourApplication main.cpp)
target_link_libraries(YourApplication Urho3D)
```

This will import all required include paths, compile definitions and compiler options.