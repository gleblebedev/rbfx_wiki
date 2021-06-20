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

Tutorials:

* [First Application](first-application)
* [Using SDK](Using-SDK)

Folder hierarchy:

* `Source/Tools/Editor/`: WIP Editor, somewhat useful;
* `Source/Samples/`: Code-friendly samples showing how to use different features;
* `Source/Urho3D/`: Main code of the framework:
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

UI overview:

* `Urho3D/UI/` is legacy built-in Urho3D UI system. It is the most flexible UI solution, but it requires the biggest amount of hand-holding as well. You can do everything, but you will have to write a lot of code to make it work smoothly. Not actively maintained, bug fixes and minor tweaks only.
* `Urho3D/RmlUI/` is the integration of RmlUI library. Work in progress, but it is already useful. HTML/CSS like UI which requires certain knowledge to setup, but it supports data bindings and UI generation natively. DPI-aware. Check out RmlUI documentation.
* `Urho3D/SystemUI/` is the integration of Dear ImGUI library. Unlike other two UIs, it supports multiple native windows. It's not recommended to use it for actual game. Use this UI for Editor add-ons or other tools.

Rendering overview:

* [Low-level rendering API](Low-level-Rendering-API)
* [Scene rendering pipeline](Scene-Rendering-Pipeline)
* Components
  * [Scene subsystems](Scene-Rendering-Subsystems)
  * [Zones](Rendering-Zones)
  * [Geometries](Rendering-Geometries)
  * [Lights](Rendering-Lights)
* Resources
  * [Models](Model-Resources)
  * [Materials](Material-Resources)
  * [Textures](Texture-Resources)
  * [Techniques](Rendering-Techniques)
  * [Shaders](Rendering-Shaders)
