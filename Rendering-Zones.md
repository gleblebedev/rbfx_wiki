# Zone Components

Zone components is a generic term for all components that affect certain area of the scene and are not lights or geometries.

All zone components have property **Is Enabled** and can be disabled or enabled individually.

## Zone

`Zone` is special kind of `Drawable` that defines global rendering properties for OBB (oriented bounding box) region of the `Scene`.

Some properties affect individual objects within the zone, other properties affect `Camera` itself when it enters the zone.

### Basic Properties

|Property|Description|
|-|-|
|**Bounding&nbsp;Box**|Bounding box of affected area.|
|**Priority**|`Zone` with highest priority is chosen when multiple zones intersect in some point.|

### Mask Properties

Masks are used to split objects into different layers which don't affect each other.

**Zone Mask** controls whether the geometry or `LightProbeGroup` is affected by the `Zone`. Condition:

`zone.[Zone Mask] & geometry.[Zone Mask] != 0`

**Light Mask** controls whether the geometry or `LightProbeGroup` *affected by the zone* is lit by light source. Condition:

`zone.[Light Mask] & geometry.[Light Mask] & light.[Light Mask] != 0`

**Shadow Mask** controls whether the geometry *affected by the zone* cast shadows for given light source. Condition:

`zone.[Shadow Mask] & geometry.[Shadow Mask] & light.[Shadow Mask] != 0`

### Ambient Properties

Zone ambient is the most simple form of lighting that fills `Scene` with constant color.

Ambient is always dynamic and is applied immediately on change.

|Property|Description|
|-|-|
|**Ambient Color**|Color of ambient light.|
|**Ambient Brightness**|Arbitrary multiplier applied to **Ambient Color**.|

### Background Properties

Background lighting the lighting emitted by background fog or background texture.

If dynamic, it is applies in the same way as ambient.
If baked, it is emitted by the background.

Disabled by default.

|Property|Description|
|-|-|
|**Zone Texture**|Cube texture used for background lighting and reflections.|
|**Background&nbsp;Brightness**|Arbitrary multiplier applied to the color of fog or **Zone Texture**.|
|**Is&nbsp;Background&nbsp;Static**|Whether to bake background lighting into light maps and light probes. If enabled, no background lighting is applied in runtime!|

### Camera Properties

Camera properties don't affect scene geometries individually, they affect the rendering `Camera` itself.

|Property|Description|
|-|-|
|**Fog&nbsp;Color**|Color of background fog. Fills background if there's no `Skybox`. Defines background lighting if there's no **Zone Texture** assigned.|
|**Fog Start**|Distance where fog begins.|
|**Fog End**|Distance where everything fades into fog.|

## Light Probe Group

`LightProbeGroup` defines a group of light probes which are used to calculate volumetric global illumination.

It is usually filled only during light baking, but it's theoretically possible to change light probe group in runtime as long `GlobalIllumination` cache is properly updated.

Unless `GlobalIllumination` is updated, total number of `LightProbeGroup` instances in the `Scene` is irrelevant.
Only the number of light probes and their arrangement matters.

Component properties only allow probe placement in uniform grid.
It is possible to place light probes in arbitrary positions by `SetLightProbes()` call.
TODO: Implement Editor UI for light probe editing.

*All enabled light probes* in the `Scene` should satisfy certain requirements in order to have correct GI:

- Keep light probes slightly above geometry surfaces.
- Don't submerge light probes into geometry.
- Keep light probes sufficiently dense where lighting changes rapidly and these changes matter.
- Circumscribed convex volume of all enabled light probes shouldn't be too thin, narrow or flat to avoid tetrahedral mesh degradation.

### Basic Properties

|Property|Description|
|-|-|
|**Zone Mask**|Controls whether the `Zone` affects this group.|
|**Light Mask**|Controls whether the light source lit this group|

### Auto Placement Properties

|Property|Description|
|-|-|
|**Auto Placement**|Whether to enabled automatic placement. If enabled, erases all existing light probes.|
|**Auto Placement Step**|Desired distance between probes.|
|**Local&nbsp;Bounding&nbsp;Box**|Bounding box of placed probes.|
