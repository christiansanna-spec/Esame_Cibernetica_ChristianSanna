# METADATA
# Software: ZBrush
# Argomento: Decimation Master Workflow
# Tipo Fonte: Official Documentation
# Data Parsing: 2026-01-10

# Decimation Master

With Decimation Master you can easily reduce the polygon count of your models in a very efficient way while keeping all their sculpted details. This solution is one of the fastest available and is able to optimize your high polycount models from ZBrush, allowing you to export them to your other 3D software packages.

Sculpt your model with ZBrush, add all your small details and push your artistic skills, then optimize your ZTool. Export it to your favorite 3D package which will now be able to open your sculpting to create specific textures like Normal Maps or Ambient Occlusion maps by baking the high resolution mesh information on a low resolution mesh.

Another use is to export your model for a Rapid Prototyping process (3D printing) and bring your virtual art to a real object but also displaying your model in a real-time viewer such as PDF 3D.

## Main features

- High quality optimization with accuracy details.
- Two different optimizations for a better control of the result.
- Optimization based on the polypainting information.
- Support of Masks for details protection.
- Border protections.
- Support of the symmetry and partial symmetry.
- Optimization of your UVs for exporting models for 3D Color printing.
- Presets for single click pre-process and decimation.

## Extras

- Export all SubTool as one OBJ file.
- Clone all SubTools.

> **Important!**
> Decimation Master is automatically installed with the default installation of ZBrush. To install Decimation Master run the full ZBrush installer again.

## Decimation Master in 3 Steps!

Using this plugin is very easy and can be done by just clicking on three buttons! Load a ZTool to optimize and in the **ZPlugin** palette, open the **Decimation Master** menu.

1. Set any desired options.
2. If you have one SubTool, press the **Preprocess current** button to launch the optimization computing. If you want to optimize all visible SubTools at once, press the **Preprocess all** button.
3. When it's done, choose a quality and press **Decimate** or **Decimate all**, depending of the previous step.

## Preparing the Model

Before decimating your ZTool, you can prepare your model using the ZBrush features described below. This will change the decimation's outcome.

### Symmetry

The plugin supports your ZTool's default symmetry. If you want a symmetric decimated result, then define the model's symmetry plane by using **Transform > Activate Symmetry** to choose the desired axis.

The vertices on the symmetry plane will be decimated like other vertices and won't be frozen, to improve the decimation vertex count.

Partial Symmetry is supported and the Plugin will try to keep the decimation as symmetrical as possible.

> **Important!**
> If you define a symmetry plane which does not match the model's symmetry, the decimation process can take more time because the internal process will check for a symmetry plane which doesn't exist. The result will be an asymmetrical decimation, but not a failure in the result.

### Masking

If you want to locally reduce the decimation on your model, you can use Masks. It's a good solution to protect specific areas of your mesh if you want to keep the maximum quality of your details. You can also change the intensity of the Mask to partially protect an area.

> **Important!**
> Decimating a model with or without a Mask will result in the same polygon count. Masking simply provides a simple way to control the density of the decimated model's polygons in specific parts of the model.

### SubTools

The plugin can work on the ZTool and its SubTools. You can decimate the current SubTool or all visible SubTools. However, working on all SubTools at once for the decimation process means that the same quality value will be applied to all SubTools.

Sometimes, depending on the visual aspect and the details of a model, it's better to work on each SubTool individually after Pre-processing and apply different quality settings to each. You can then recombine the new models together using the **Tool > SubTool > Append** feature.

## The Decimation process

The decimation process needs to be done in three separate steps. The first step (setting the options) is optional depending on your needs. The second and third steps work together and are required.

### 1. Setting the options

This is the first step in which you can choose some options:

**Freeze Borders**
This option avoids the decimation of edges and vertices which are on the border / opening of an object. If your model to decimate is part of an assembly, you will be able to weld them perfectly after the decimation process.

**Keep UV's**
This option will use the existing UV's of your original model and will pre-process them for a future decimation. The UV's will be optimizing the same way as the vertices. But because of the UV's seams, the optimization will be less important on UV's seams. The decimation of the UV's will give better results on hand unwrapped UV's than AUV or GUV tiles.

> **Important!**
> If you don't need the UV's on your decimated model, don't pre-process them. The computing of the UVs requires 50% more memory than the same decimation without the UV's.

**Polypaint Weight**
This slider uses the polypaint to adjust the quality of a decimation. The default value fits most needs in terms of quality, but if you want to have a specific optimization depending of one or the other criteria, change this slider setting.

**Keep & Use PolyPainting**
This preference will do a specific optimization to take in consideration the PolyPainting of your ZTool to have the best result after the decimation. The quality of decimation will be based on your polypainting which can be adjusted in the **Decimation Master Preferences**.

> **Important!**
> The **Keep & use PolyPainting** option will increase the pre-processing time.

### 2. Pre-processing

This is the second step. Internally, the plugin will compute the decimation for 100% quality to 0% quality and will create a series of temporary files (named "Progressive mesh"). Then in the decimation step that follows, the plugin will read this progressive mesh to apply the decimation result. This pre-process computing time depends on your current ZTool and its polygon / Active points count.

When clicking **Pre-process Current** or **Pre-process All**, a progress bar will appear with information about the preprocess' status.

Each SubTool must have a unique name to be processed. If you have several SubTools with the same name, rename them first.

If you launch a new pre-process on a decimated model, the current quality will become the original model for the future decimations.

### 3. Decimating

This is the third step. Choose the quality of the decimation to apply. 100% means no decimation, 0.01% means maximum decimation.

You can also choose a target value for the number of vertices/points or polygons, in thousands of polygons.

When you have chosen the quality, click on **Decimate Current** to decimate the selected ZTool/SubTool, or **Decimate All** for all the visible SubTools.

For high quality decimation (40%-100%), the visual result will be almost the same as the original model even with so many polygons removed. You may need to scale your model to see the modification. For low quality decimation (2%-40%), you will start to see noticeable changes in the model's details. This will vary from model to model depending on its details and structure. You can also choose to display the wireframe to better see the changes on your ZTool. Do this by clicking the **Frame** option in ZBrush.

> **Notes:**
> - The decimation is applied based on the ZTool and its parameters as they existed when doing the pre-process. If you remove a subdivision level, add a mask, etc. after launching the pre-process, the decimation won't take these changes into consideration.
> - A short delay can happen between the time you click on the **Decimate** button and the result, based upon the amount of information to read from the hard drive. A progress bar will appear with information about the decimation's status.
> - **Decimate All** can take some time depending of the number of SubTools.

## Presets

The preset buttons give the option to pre-process and decimate your model with a single-click. Choose from the different buttons or set a custom value in the slider before pressing the **Custom** button. The Custom setting is saved between ZBrush sessions.

## Utilities

This set of utilities will help you when working with the decimation:

### Delete caches
This button deletes all the temporary files (progressive meshes). This action can't be undone.

### Export All SubTools
This button will export the current ZTool and its SubTools to one unique OBJ file. Used in place of ZBrush's single OBJ file export, this utility will export only the geometry and the UV's, with a single group for each SubTool. No polygroups are exported.

### Temporary files
The Decimation plugin uses temporary files for two purposes:
1. To reduce the amount of memory overhead, thus increasing the number of polygons that can be handled by this decimation process.
2. To let you reload a previously pre-processed ZTool and decimate it directly, without the need for another time consuming pre-process phase.

If you don't need to decimate a ZTool anymore you can choose to delete these temporary files by pressing the **Delete Caches** button in the utilities section.

> **Important!**
> These files can become quite large on very high polygon meshes. ZBrush won't automatically delete these files when quitting, so don't forget to delete them from time to time.

## Preferences

Some parameters of Decimation Master have been moved in the Preferences palette. This makes the default behaviors easier to understand. Change these settings only if you have very specific needs.

### Uniform Mesh
This option keeps a constant aspect to the decimated polygons by creating a kind of uniform decimation defined by areas, like low details and high details. Activating this option can slightly change the result of your decimation.

### Number of Threads
This slider lets you choose how many threads you want to use, based on your computer's processor. The Decimation Master plugin is multithreaded, and it will use your computer's resources as much as possible to improve the computing time. Reducing this number will increase the pre-process time but will allow you to work on other tasks at the same time.

### Delete caches at start
This option lets you definie the behavior of the plugin regarding the temporary files in its data cache. These files can take a lot of disk space if you never clean them. This option when activated will erase all the temporary files on ZBrush Startup. If you would like to decimate a ZTool or Subtool(s) in multiple sessions without having to pre-process again uncheck this setting.

### Save Preferences
Click on this button to store the plugin preferences. Using the ZBrush default **Store Config** function won't save the Decimation Master plugin preferences.

### 64-bit Decimator
This preference is activated by default when ZBrush is running on a 64 bit system. Then when in action, Decimation Master will use all the memory available for the pre process which is very useful when decimating a model and keeping its UVs.

> **Notes:**
> The Decimation Master preferences are not bound to ZBrush ones. You don't need to do a **Store Config** to save them. They are automatically saved in the `DecimationMaster.cfg` file, located in the DecimationMaster Data folder.

## Troubleshooting

- **Polygroups are not optimized:** The resulting model won't have the original polygroups. This is because a model's polygroups are dependent upon the points of its lowest resolution level. Decimation Master's very purpose is to change the lowest resolution of the mesh, which will of course affect the polygroups.
- **Model edited after pre-process:** If the model has been edited between the pre-process and the decimation, you must re-launch a pre-process. If not, the decimation will be based on the original model. In case you are not sure, do a new pre-process.
- **Loading different ZTool with same name:** If a model has been pre-processed in a previous ZBrush session and you load in a new session a different ZTool with the same name (for example: `PM3D_sphere`) it is necessary to do a new pre-process or any decimation will be based on the previous ZTool.
- **Wrong SubTool displayed:** After you optimize, if ZBrush is displaying a different SubTool from the one you just processed it's because these SubTools have the same name. To solve this problem, rename the SubTools and pre-process them again. Another solution is to delete the caches by clicking the plugin's **Utilities > Delete Caches** button.
- **Killing the process:** If you decide to kill the Decimation Master process in your operating system's process manager, ZBrush won't be affected. However, the ZScript which is working with the plugin will continue to run and keep the focus over the application. To stop the ZScript, press the ESC key. You may have a remaining progress bar which is just an artifact of the process you just killed.