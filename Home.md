Welcome to the Urho3D wiki!

* [First Application](first-application)
* [Using SDK](Using-SDK)
* [Profiling](Profiling)
* [Editor Overview](Editor)
  * [Plugins](Plugins)
* [C# Support](C%23-support)
* [External subsystems and samples](External-subsystems-and-samples)
* [rbfx and Urho3D differences](rbfx-and-Urho3D-differences)
* [Migration from vanilla Urho3D](Migration-from-vanilla-Urho3D)

---

[Draft documentation for ek/renderer branch]

**Disclaimer**: this wiki doesn't attempt to describe every single feature, class or function in the framework. It merely offers high-level overview of framework systems and features. Otherwise it would be too expensive to maintain. Use this wiki together with reading framework headers in `Source/Urho3D/*` and relevant comments.

Folder hierarchy:

* `Audio`: Audio playback utilities and scene components;
* `Container`: Generic containers and container-related code;
* `Core`: Generic utilities used by all other modules;
* `CSharp`: C# bindings;
* `Engine`: Connects all modules together and manages the framework as a whole;
* `Glow`: Lightmap and light probe baking tool;
* `Graphics`: Contains both low-level GAPI interop layer and high-level rendering components;
* `IK`: Inverse kinematics components;
* `Input`: User input handling (keyboard, mouse, touch, joystick);
* `IO`: Low-level and high-level serialization and file system management utilities (and logger);
* `Math`: Mathematical abstractions and geometry objects;
* `Navigation`: Navigation mesh generation and pathfinding;
* `Network`: Low-level networking utilities;
* `Physics`: Physics simulation and related scene components;
* `RenderPipeline`: Transforms resources and scene components to draw commands;
* `Resource`: Resource cache and generic resource types;
* `RmlUI`: [RmlUI](https://github.com/mikke89/RmlUi) integration;
* `Scene`: Scene graph and some generic scene components;
* `Script`: Script management;
* `SystemUI`: [Dear ImGUI](https://github.com/ocornut/imgui) integration;
* `UI`: Legacy Urho UI. **Not maintained.**
* `Urho2D`: 2D rendering and physics. **Not maintained.**

Rendering overview:

* Low-level GAPI interop
* Scene rendering pipeline
* Components
  * Global scene subsystems
  * Zones
  * Geometries
  * Lights
* Resources
  * Models
  * Materials
  * Textures
  * Techniques
  * Shaders