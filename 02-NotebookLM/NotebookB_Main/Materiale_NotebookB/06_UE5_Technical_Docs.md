````
# METADATA
# Software: Unreal Engine 5.4+
# Argomento: Lumen & Nanite Technical Deep Dive
# Tipo Fonte: Official Documentation (Epic Games)
# Data Parsing: 2026-01-10

# CAPITOLO 1: LUMEN TECHNICAL DETAILS

Lumen uses multiple ray-tracing methods to solve Global Illumination and Reflections. Screen Traces are done first, followed by a more reliable method. Lumen supports two different ray tracing methods: Hardware Ray Tracing and Software Ray Tracing through signed distance fields. Hardware ray tracing provides higher quality and is enabled by default, but it requires dedicated video card support and is more expensive.

Lumen's Global Illumination and Reflections primary shipping target is to support large, open worlds running at 60 frames per second (FPS) on next-generation consoles. The engine's **High** scalability level contains settings for Lumen targeting 60 FPS.

## Surface Cache (Lumen)
Lumen generates an automatic parameterization of the nearby scene's surface called the Surface Cache. It is used to quickly look up lighting at ray hit points in the scene. Lumen captures the material properties for each mesh from multiple angles. These capture positions (called Cards) are generated offline for each mesh.

Cards can be visualized with the console command:
```cpp
r.Lumen.Visualize.CardPlacement 1
````

By default, Lumen only places 12 Cards on a mesh, but you can increase that amount by setting **Max Lumen Mesh Cards** in the Build Settings of the Static Mesh Editor. Adjusting the number of cards is useful for more complex interiors or single meshes with irregular shapes.

Areas that don't have Surface Cache coverage are colored **pink** in the Surface Cache View Mode of the Level Editor. These areas will not bounce light and will appear black in reflections. Issues like this can be fixed by increasing the number of Cards used with Max Lumen Mesh Cards, or by breaking the mesh into less complex pieces.

**Diagnostic Mode:** `View Mode > Lumen > Surface Cache`

**Limitations:**

- Materials with view-dependent logic (Pixel Depth, Camera Position) may appear incorrectly. Use the **Ray Tracing Quality Switch** node to provide a simplified version for Lumen.
    
- **Nanite:** High polygon meshes must use Nanite to have efficient captures. Foliage and Instanced Static Meshes are supported only if using Nanite.
    
- **Modular Design:** Only meshes with simple interiors are supported. Walls, floors, and ceilings should be separate meshes. Importing an entire room as a single mesh will fail.
    

## Screen Tracing

Lumen traces rays against the screen first (Screen Space Tracing), before using a more reliable method. This covers up mismatches between the Lumen Scene and triangle scene.

**Disadvantage:** Limits art direction controls (Indirect Lighting Scale, Emissive Boost) because Screen Traces cannot support them correctly, causing view-dependent GI artifacts.

**How to Disable:**

- Level Viewport: `Show > Lumen > Screen Traces` (uncheck).
    
- Post Process Volume: Uncheck `Screen Traces` under Lumen Global Illumination.
    

## Lumen Ray Tracing Methods

### 1. Software Ray Tracing (Default)

Uses **Mesh Distance Fields**. Supported on any hardware (SM6).

- **Requirements:** `Generate Mesh Distance Fields` enabled in Project Settings.
    
- **Logic:** Traces against individual mesh distance field for the first 2 meters, then Global Distance Field for the rest.
    
- **Console Command for Stats:** `r.DistanceFields.LogAtlasStats 1`
    
- **Limitations:** WPO not supported; Translucency treated as opaque.
    

### 2. Hardware Ray Tracing (High Quality)

Traces against actual triangles. Requires RTX-2000+ or RX-6000+.

- **Setup:** Enable `Support Hardware Ray Tracing` and `Use Hardware Ray Tracing when available` in Project Settings.
    
- **Nanite Fallback:** Can use Nanite Proxy Mesh or actual Nanite geometry.
    

Hybrid Strategy (Software Fallback):

To save memory, disable software fallback when using Hardware RT by adding to DefaultEngine.ini:

C++

```
r.DistanceFields.SupportEvenIfHardwareRayTracingSupported=0
```

## Lumen Troubleshooting Guide

### Small Meshes are Black in Mirror Reflections

- **Cause:** Lumen culls small objects from Lumen Scene for performance.
    
- **Fix:** In Post Process Settings, increase **Lumen Scene Detail**.
    

### Sky Occlusion / GI Disappears at 200 Meters

- **Cause:** Default range limit.
    
- **Fix:** In Post Process Settings, increase **Lumen Scene View Distance**.
    

### Light Leaking in Caves / Interiors

- **Cause:** Missing occluders or trace limits.
    
- **Fix:** Raise **Lumen Scene Detail** and **Max Trace Distance**. Ensure walls are thicker than 10cm.
    

---

# CAPITOLO 2: NANITE VIRTUALIZED GEOMETRY

Nanite is Unreal Engine's virtualized geometry system which uses a new internal mesh format and rendering technology to render pixel scale detail and high object counts. It intelligently does work on only the detail that can be perceived and no more. Nanite's data format is also highly compressed, and supports fine-grained streaming with automatic level of detail.

## Benefits of Nanite

- **Geometric Complexity:** Multiple orders of magnitude increase in geometry complexity.
    
- **No Manual LODs:** Nanite automatically handles Level of Detail (LOD). There is no need to manually create LODs or bake normal maps from high-poly to low-poly meshes.
    
- **Compression:** High compression ratios. A one-million-triangle Nanite mesh generally takes up less memory than a traditional mesh with a 4K normal map.
    
- **Performance:** Rendering is largely independent of geometric complexity and is more bound by screen resolution.
    

## Enabling Nanite

### Method 1: Static Mesh Editor

1. Open the Static Mesh in the **Static Mesh Editor**.
    
2. In the **Details** panel, locate the **Nanite Settings** section.
    
3. Check the box for **Enable Nanite Support**.
    
4. Click **Apply Changes**.
    

### Method 2: Content Browser (Batch)

1. Select one or more Static Meshes in the **Content Browser**.
    
2. Right-click and choose **Nanite > Enable**.
    

## Supported Features

A Nanite-enabled mesh can be used with the following Component types:

- Static Mesh
    
- Instanced Static Mesh
    
- Hierarchical Instanced Static Mesh
    
- Geometry Collection
    
- Foliage Painter & Landscape Grass
    
- Skeletal Mesh (Initial support in newer UE5 versions)
    

### Materials & Shadows

- **Materials:** Nanite meshes support multiple UVs and vertex colors. Materials are assigned to sections of the mesh such that those materials can use different shading models.
    
- **Virtual Shadow Maps:** Nanite is highly optimized to work with Virtual Shadow Maps (VSM) to render shadows for high-poly geometry efficiently.
    

## Limitations & Technical Constraints

### Deformation

Nanite has limited support for the deformation of rigid meshes. It supports dynamic translation, rotation, and non-uniform scaling.

- **World Position Offset (WPO):** Supported with caveats. Nanite meshes using WPO are split into smaller clusters, which may affect culling efficiency. You should clamp WPO usage to manage performance.
    

### Platforms & Hardware

- **Windows:** Windows 10 (version 1909.1350 or newer) or Windows 11.
    
- **DirectX:** DirectX 12 with Shader Model 6.6 atomics (or latest drivers).
    
- **Consoles:** PlayStation 5, Xbox Series S/X.
    

## Nanite Visualization Modes

Use these modes to debug geometry and performance. Access via Viewport menu: **View Mode > Nanite Visualization**.

- **Overview:** Shows the rendered scene.
    
- **Triangles:** Visualizes the individual triangles generated by Nanite.
    
- **Clusters:** Visualizes the cluster culling and streaming (Crucial for debugging WPO issues).
    
- **Instances:** Visualizes instance rendering.