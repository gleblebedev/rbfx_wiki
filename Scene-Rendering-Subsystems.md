# Render Pipeline

`RenderPipeline` is required root-level `Scene` component that manages scene rendering and keeps current rendering settings.

For compatibility reasons it is created automatically unless legacy renderer is used.

See [Scene rendering pipeline](Scene-Rendering-Pipeline) for details.

# Global Illumination

`GlobalIllumination` is root-level `Scene` component that manages baked GI data.

`GlobalIllumination` is required if light baking is used, otherwise it's unused and unnecessary.

|Name|Description|
|-|-|
|**Emission&nbsp;Brightness**|Emission brightness multiplied for GI baking.|

# Light Baker

`LightBaker` is root-level `Scene` component that controls light baking quality.

### Basic settings

|Name|Description|
|-|-|
|**Output&nbsp;Directory**|Output directory to store baking results. If empty, results are stored nearby `Scene` file in separate directory.|
|**Lightmap Size**|Size of lightmap texture.|
|**Texel Density**|Number of lightmap texels allocated per scene unit.|

### Quality settings

|Name|Description|
|-|-|
|**Quality**|Quality presets.|
|> Custom|Custom settings.|
|> Low|Fast baking, low quality.|
|> Medium|Slower baking, medium quality.|
|> High|Slow baking, high quality.|
|**Direct Samples (Lightmap)**|Number of rays traced per lightmap texels per light source.|
|**Direct Samples (Light&nbsp;Probes)**|Number of rays traced per light probe per light source.|
|**Indirect Bounces**|Number of indirect light bounces.|
|**Indirect Samples (Texture)**|Number of rays traced per lightmap texels for indirect light baking.|
|**Indirect&nbsp;Samples (Light&nbsp;Probes)**|Number of rays traced per light probe for indirect light baking.|
|**Filter Radius (Direct)**|Gauss blur radius for direct light in lightmap.|
|**Filter Radius (Indirect)**|Gauss blur radius for indirect light in lightmap.|

### Chunk settings

Scene is split into multiple chunks baked separately to limit complexity and hardware requirements for large scenes.

|Name|Description|
|-|-|
|**Chunk&nbsp;Size**|Size of chunk in units.|
|**Chunk Indirect Padding**|Size of extra space with nearby geometry used for indirect light baking.|
|**Chunk Shadow Distance**|Max distance of cast shadows from light sources.|

### Miscellaneous settings

|Name|Description|
|-|-|
|**Stitch&nbsp;Iterations**|Number of iterations in lightmap seam stitching. Higher is slower, but lightmap seams are less visible.|
