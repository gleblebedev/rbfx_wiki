Profiling
=========

Engine integrates [easy_profiler](https://github.com/yse/easy_profiler) for CPU profiling. GPU profiling may be supported in the future. In order to profile your application do the following:

1. Build engine with `-DURHO3D_PROFILING=ON`
2. Mark code sections you wish to profile with `URHO3D_PROFILE*` macros.
3. Start listening socket in your application by calling `GetProfiler()->StartListen();`.
4. Connect profiler application by pressing [[images/connect.png]]
5. Receive profiling data by pressing [[images/capture.png]]

[[images/profiler.png]]

# Macros

## URHO3D_PROFILE

Profile a block of code until end of scope. Pass unquoted string as a name of block.

```cpp

void ProfiledFunction()
{
    // Code timing of which is not included in profiled block.
    URHO3D_PROFILE(ProfiledFunctionBlock);
    // Other code here which is included in block timing.
}
```

## URHO3D_PROFILE_SCOPED

Profile a block of code until end of scope. Pass quoted string as a name of block. Use this for blocks whose name has to be dynamically constructed. If possible avoid this macro and use `URHO3D_PROFILE`.


```cpp

void ProfiledFunction()
{
    // Code timing of which is not included in profiled block.
    URHO3D_PROFILE_SCOPED("ProfiledFunctionBlock");
    // Other code here which is included in block timing.
}
```

## URHO3D_PROFILE_NONSCOPED and URHO3D_PROFILE_END

`URHO3D_PROFILE_NONSCOPED` defines a profiled block of code which does not end at the termination of current scope. Block must be terminated with `URHO3D_PROFILE_END`.

```cpp

void BeginProfiling()
{
    URHO3D_PROFILE_NONSCOPED(NonScopedBlock);
}

void StopProfiling()
{
    URHO3D_PROFILE_END();
}

void DoWork()
{
    // Code not included in `NonScopedBlock`.
    BeginProfiling();
    // Code included in `NonScopedBlock`.
    StopProfiling();
    // Code not included in `NonScopedBlock`.
}
```

## URHO3D_PROFILE_THREAD

If you use worker threads this macro can be used to give meaningful names to the threads. Place this macro at the start of worker function.

```cpp
void WorkerFunction()
{
    URHO3D_PROFILE_THREAD(MyWorker);
    // Other code.
}
```
