Experimental C# support is available. Progress may be tracked at https://github.com/rokups/Urho3DNet repository.

Current C# wrapper features:

* Extensive API coverage estimated at 90%+
* Support for inheriting native classes and overriding their virtual methods
* Support for `ref` parameters
* Support for native containers like `VariantMap` and `StringVector`
* Mapping of `PODVector<T>` and `Vector<SharedPtr<T>>` containers to C# arrays
* Serialization and deserialization of managed components
* Conversion of getters and setters to C# properties
* mono and .net framework support requiring version 4.7
* Member variable and method renaming to match C# conventions

Planned features:

* Managed plugins for applications and editor
* Support for generating bindings for user code

How the code looks:

```cs
using System;
using System.Diagnostics;
using System.IO;
using Urho3D;

namespace DemoApplication
{
    class RotateObject : LogicComponent
    {
        public RotateObject(Context context) : base(context)
        {
            UpdateEventMask = USE_UPDATE;
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
            EngineParameters[EngineDefs.EP_FULL_SCREEN] = false;
            EngineParameters[EngineDefs.EP_WINDOW_WIDTH] = 1920;
            EngineParameters[EngineDefs.EP_WINDOW_HEIGHT] = 1080;
            EngineParameters[EngineDefs.EP_WINDOW_TITLE] = "Hello C#";
            EngineParameters[EngineDefs.EP_RESOURCE_PREFIX_PATHS] = $"{currentDir};{currentDir}/..";
        }

        public override void Start()
        {
            Input.SetMouseVisible(true);

            Context.RegisterFactory<RotateObject>();

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
            _camera.LookAt(Vector3.ZERO);

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
            _light.LookAt(Vector3.ZERO);

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
                    application.Run();
            }
        }
    }
}
```

You may find sample project for testing at https://github.com/rokups/Urho3DNet-Demo/.

