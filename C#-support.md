⚠️ C# support is experimental ⚠️

## Current C# wrapper features

* Extensive API coverage estimated at 90%+
* Support for inheriting native classes and overriding their virtual methods
* Support for `ref` parameters
* Support for native containers (`HashMap<T>`, `Vector<T>`, `PODVector<T>`)
* Transparently unwrapping `SharedPtr<T>` and `WeakPtr<T>`
* Serialization and deserialization of managed components
* Member variable and method renaming to match C# conventions

## Planned features

* Managed plugins for applications and editor
* Wrap physics subsystem
* Wrap networking subsystem
* Wrap Urho3D subsystem

## How the code looks

```cs
//
// Copyright (c) 2018 Rokas Kupstys
//
// Permission is hereby granted, free of charge, to any person obtaining a copy
// of this software and associated documentation files (the "Software"), to deal
// in the Software without restriction, including without limitation the rights
// to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
// copies of the Software, and to permit persons to whom the Software is
// furnished to do so, subject to the following conditions:
//
// The above copyright notice and this permission notice shall be included in
// all copies or substantial portions of the Software.
//
// THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
// IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
// FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
// AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
// LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
// OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
// THE SOFTWARE.
//
using System;
using System.Diagnostics;
using System.IO;
using Urho3DNet;
using ImGuiNet;

namespace DemoApplication
{
    [ObjectFactory]
    class RotateObject : LogicComponent
    {
        public RotateObject(Context context) : base(context)
        {
            SetUpdateEventMask(UpdateEvent.UseUpdate);
        }

        public override void Update(float timeStep)
        {
            var d = new Quaternion(10 * timeStep, 20 * timeStep, 30 * timeStep);
            GetNode().Rotate(d);
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
            EngineParameters[Urho3D.EpFullScreen] = false;
            EngineParameters[Urho3D.EpWindowWidth] = 1920;
            EngineParameters[Urho3D.EpWindowHeight] = 1080;
            EngineParameters[Urho3D.EpWindowTitle] = "Hello C#";
            EngineParameters[Urho3D.EpResourcePrefixPaths] = $"{currentDir};{currentDir}/..";
        }

        public override void Start()
        {
            GetInput().SetMouseVisible(true);

            // Viewport
            _scene = new Scene(GetContext());
            _scene.CreateComponent<Octree>();

            _camera = _scene.CreateChild("Camera");
            _viewport = new Viewport(GetContext());
            _viewport.SetScene(_scene);
            _viewport.SetCamera(_camera.CreateComponent<Camera>());
            GetRenderer().SetViewport(0, _viewport);

            // Background
            GetRenderer().GetDefaultZone().SetFogColor(new Color(0.5f, 0.5f, 0.7f));

            // Scene
            _camera.SetPosition(new Vector3(0, 2, -2));
            _camera.LookAt(Vector3.Zero);

            // Cube
            _cube = _scene.CreateChild("Cube");
            var model = _cube.CreateComponent<StaticModel>();
            model.SetModel(GetCache().GetResource<Model>("Models/Box.mdl"));
            model.SetMaterial(0, GetCache().GetResource<Material>("Materials/Stone.xml"));
            var rotator = _cube.CreateComponent<RotateObject>();

            // Light
            _light = _scene.CreateChild("Light");
            _light.CreateComponent<Light>();
            _light.SetPosition(new Vector3(0, 2, -1));
            _light.LookAt(Vector3.Zero);

            SubscribeToEvent(E.Update, args =>
            {
                var timestep = args[E.Update.TimeStep].GetFloat();
                Debug.Assert(this != null);

                if (ImGui.Begin("Urho3D.NET"))
                {
                    ImGui.TextColored(Color.Red, $"Hello world from C#.\nFrame time: {timestep}");
                }
                ImGui.End();
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

* Mono (latest) or .NET framework (4.7.1)
* SWIG (prebuilt binaries available for windows/macos/ubuntu 14.04)

## Build

1. Configure build: `cmake -DBUILD_SHARED_LIBS=ON -DURHO3D_FEATURES=CSHARP;SYSTEMUI /path/to/code`
2. `cmake --build .`
3. Create C# project in IDE of your choice, targeting .NET framework 4.7.1.
4. Copy `bin/CoreData` and `bin/Data` to output directory (where .exe will be created).
5. Add `bin/{Urho3D,ImGui}Net.dll` as references to your project.
  * Linux: Copy `bin/lib{Urho3D,Urho3DCSharp,ImGuiCSharp}.so` to output directory.
  * Windows: Copy `bin/{Urho3D,Urho3DCSharp,ImGuiCSharp}.dll` to output directory.
  * MacOS: Copy `bin/lib{Urho3D,Urho3DCSharp,ImGuiCSharp}.dylib` to output directory.
6. Build and run. For a quick test you may copy code from `Source/Samples/102_CSharpProject/Project.cs`.

## Linux

Project uses customized version of SWIG. Chances are prebuilt binaries will not run on your distribution.
1. Download https://github.com/rokups/swig/archive/Urho3D.zip and build it.
2. Additionally pass `-DSWIG_EXECUTABLE=/path/to/bin/swig -DSWIG_DIR=/path/to/share/swig/4.0.0` to cmake pointing to custom SWIG build.