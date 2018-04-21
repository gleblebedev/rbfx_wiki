⚠️ C# support is experimental ⚠️

## Current C# wrapper features

* Extensive API coverage estimated at 90%+
* Support for inheriting native classes and overriding their virtual methods
* Support for `ref` parameters
* Support for several native containers like `VariantMap` and `StringVector`
* Mapping of `PODVector<T>` and `Vector<SharedPtr<T>>` containers to C# arrays
* Serialization and deserialization of managed components
* Conversion of getters and setters to C# properties
* Member variable and method renaming to match C# conventions

## Planned features

* Managed plugins for applications and editor
* Support for generating bindings for user code

## How the code looks

```cs
using System;
using System.Diagnostics;
using System.IO;
using Urho3D;

namespace DemoApplication
{
    [ObjectFactory]
    class RotateObject : LogicComponent
    {
        public RotateObject(Context context) : base(context)
        {
            UpdateEventMask = UseUpdate;
        }

        public override void Update(float timeStep)
        {
            var d = new Quaternion(10 * timeStep, 20 * timeStep, 30 * timeStep);
            Node.Rotate(d);
        }
    }

    class DemoApplication : Application
    {
        private Scene _scene;
        private Viewport _viewport;
        private Node _camera;
        private Node _cube;
        private Node _light;

        public DemoApplication(Context context) : base(context)
        {
        }

        public override void Setup()
        {
            var currentDir = Directory.GetCurrentDirectory();
            EngineParameters[EngineDefs.EpFullScreen] = false;
            EngineParameters[EngineDefs.EpWindowWidth] = 1920;
            EngineParameters[EngineDefs.EpWindowHeight] = 1080;
            EngineParameters[EngineDefs.EpWindowTitle] = "Hello C#";
            EngineParameters[EngineDefs.EpResourcePrefixPaths] = $"{currentDir};{currentDir}/..";
        }

        public override void Start()
        {
            Input.SetMouseVisible(true);

            // Viewport
            _scene = new Scene(Context);
            _scene.CreateComponent<Octree>();

            _camera = _scene.CreateChild("Camera");
            _viewport = new Viewport(Context, _scene, _camera.CreateComponent<Camera>());
            Renderer.SetViewport(0, _viewport);

            // Background
            Renderer.DefaultZone.FogColor = new Color(0.5f, 0.5f, 0.7f);

            // Scene
            _camera.Position = new Vector3(0, 2, -2);
            _camera.LookAt(Vector3.Zero);

            // Cube
            _cube = _scene.CreateChild("Cube");
            var model = _cube.CreateComponent<StaticModel>();
            model.Model = Cache.GetResource<Model>("Models/Box.mdl");
            model.SetMaterial(0, Cache.GetResource<Material>("Materials/Stone.xml"));
            _cube.CreateComponent<RotateObject>();

            // Light
            _light = _scene.CreateChild("Light");
            _light.CreateComponent<Light>();
            _light.Position = new Vector3(0, 2, -1);
            _light.LookAt(Vector3.Zero);

            SubscribeToEvent(CoreEvents.E_UPDATE, args =>
            {
                var timestep = args[Update.P_TIMESTEP].Float;
                Debug.Assert(this != null);
            });
        }
    }

    internal class Program
    {
        public static void Main(string[] args)
        {
            using (var context = new Context())
            {
                using (var application = new DemoApplication(context))
                {
                    application.Run();
                }
            }
        }
    }
}
```

## Requirements

* Mono (5.4+ is known to work)
* Clang

## Build instructions

### Linux

1. Install dependencies:
  * Archlinux: `pacman -S mono llvm clang`
  * Ubuntu: `apt-get install mono-devel llvm clang`
2. Configure build: `cmake -DURHO3D_CSHARP=ON /path/to/code`
3. `cmake --build`

### Windows

1. Install dependencies:
  * Latest llvm: http://releases.llvm.org/download.html
  * Latest mono: http://www.mono-project.com/download/stable/#download-win
2. Configure build: `-DURHO3D_CSHARP=ON -DLLVM_VERSION_EXPLICIT=5.0.1 -DLIBCLANG_LIBRARY="C:/Program Files/LLVM/lib/libclang.lib" -DLIBCLANG_INCLUDE_DIR="C:/Program Files/LLVM/include" -DLIBCLANG_SYSTEM_INCLUDE_DIR="C:/"Program Files"/LLVM/lib/clang/5.0.1/include" -DCLANG_BINARY="C:/Program Files/LLVM/bin/clang++.exe" /path/to/code`. Adjust parameters as necessary for LLVM version you have installed.
3. `cmake --build`

### Mac OS

1. Install dependencies:
  * `brew install pkg-config mono llvm`
2. Configure build: `-DURHO3D_CSHARP=ON LLVM_CONFIG_BINARY=/usr/local/opt/llvm/bin/llvm-config /path/to/code`. Adjust parameters as necessary for LLVM version you have installed.
3. `cmake --build`

## Useage

### Linux

1. Create C# project in IDE of your choice.
2. Add Urho3DNet.dll reference to your C# project. This file is located in `/path/to/buildtree/bin`.
3. Make sure `libUrho3D.so` and/or `libUrho3DCSharp.so` are in the same folder as `Urho3DNet.dll`. Referenced .net DLLs get copied to executable binaray dir, but native libraries used by these dlls are not.
4. Build and run.

### Windows

TODO
