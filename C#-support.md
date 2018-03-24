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

Planned features:

* Member variable and method renaming to match C# conventions
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
        private Scene scene_;
        private Viewport viewport_;
        private Node camera_;
        private Node cube_;
        private Node light_;

        public DemoApplication(Context context) : base(context)
        {
        }

        public override void Setup()
        {
            var currentDir = Directory.GetCurrentDirectory();
            engineParameters_[EngineDefs.EP_FULL_SCREEN] = false;
            engineParameters_[EngineDefs.EP_WINDOW_WIDTH] = 1920;
            engineParameters_[EngineDefs.EP_WINDOW_HEIGHT] = 1080;
            engineParameters_[EngineDefs.EP_WINDOW_TITLE] = "Hello C#";
            engineParameters_[EngineDefs.EP_RESOURCE_PREFIX_PATHS] = $"{currentDir};{currentDir}/..";
        }

        public override void Start()
        {
            Input.SetMouseVisible(true);

            context_.RegisterFactory<RotateObject>();

            // Viewport
            scene_ = new Scene(context_);
            scene_.CreateComponent<Octree>();

            camera_ = scene_.CreateChild("Camera");
            viewport_ = new Viewport(context_, scene_, camera_.CreateComponent<Camera>());
            Renderer.SetViewport(0, viewport_);

            // Background
            Renderer.DefaultZone.FogColor = new Color(0.5f, 0.5f, 0.7f);

            // Scene
            camera_.Position = new Vector3(0, 2, -2);
            camera_.LookAt(Vector3.ZERO);

            // Cube
            cube_ = scene_.CreateChild("Cube");
            var model = cube_.CreateComponent<StaticModel>();
            model.Model = Cache.GetResource<Model>("Models/Box.mdl");
            model.SetMaterial(0, Cache.GetResource<Material>("Materials/Stone.xml"));
            cube_.CreateComponent<RotateObject>();

            // Light
            light_ = scene_.CreateChild("Light");
            light_.CreateComponent<Light>();
            light_.Position = new Vector3(0, 2, -1);
            light_.LookAt(Vector3.ZERO);

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

