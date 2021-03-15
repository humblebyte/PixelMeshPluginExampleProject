# Runtime Pixel Mesh Component / PixelMeshPluginExampleProject
Demo and documentation of the Pixel Mesh / Runtime Pixel Mesh Component
This github is not actively supported. Use the support email on the Unreal Marketplace Product Page

YouTube Video of Example Project:
https://www.youtube.com/watch?v=mcTQVM4ccio&lc=Ugywt6BV082_U6yv8MN4AaABAg

# Overview

Great for prototyping quickly or use in your shipping game! Runtime Pixel Mesh Component is a plugin that allows fast rendering of pixel art and other textures at runtime as a mesh with depth. Complete with collision generation, different fill techniques, support to load exported static meshes and delegates/events when the mesh has finished.

## Getting Started

To use this project you must have a version of the Runtime Pixel Mesh Component purchased and installed from the Unreal Engine Marketplace.

If you would like to install the plugin seperate from the engine or just for this example place the plugin in the /Plugins/ folder of the project.

## Example Project Usage

The project has been written to act as additional documentation. Open the different levels within 'FeatureExamples' to view different settings in action.

## Texture Settings

For best results and if pixel art is your thing make sure textures are unfiltered, have no mips, and are a VectorDisplacementmap.

Textures should be power of 2 (...64...128...256...512...1024...)

## Implementation
### Static
Simply drop the RealtimePixelMesh component into an actor and set its SourceTexture. There is a default material contained within the plugin content, any material can be used as long as it has a Texture Parameter and has it specified by name in the component's TextureParameterName.

### Dynamic
Without a SourceTexture specified the component will wait to generate its mesh. Use SetSourceTexture() on the component of a spawned actor to start generation. Note: If using MeshOverride it must be set prior to the initial SetSourceTexture() call.

### Static Blueprint Helper Function
![Alt text](/SpawnPixelMeshActorHelper.PNG?raw=true "Component Variables")

## Exporting Meshes
Meshes can be exported to a project's content folder using UE4's functionality built into the Procedural Mesh Component, which Runtime Pixel Mesh Component extends.

![Alt text](/SaveStaticMesh.PNG?raw=true "Save Static Mesh")

## Stepped Generation
While meshes can be displayed instantanously, stepped mesh generation can be used to spread out the mesh's generation and increase performance.

## Completion Event and Delegate
OnPixelMeshComplete on the component is overridable in blueprint. Alternatively bind to OnPixelMeshCompleteDelegate.

## Balancing Performance 
The complexity of the resulting mesh must be considered at the same time as the method of generation. Here are some tips and setting combinations to experiment with.

HiddenUntilComplete - Enabling this will delay the mesh from being rendered until it's data has been processed. Removing any hitching that could be related to redrawing the mesh during generation.

SteppedMeshGeneration - Experiment with longer times and more steps to draw out generation. Depending on your texture's resolution the maximum steps required to draw your texture is it's area (Size X * Size Y). The maximum steps per second is capped at your processing speed. 

GenerateCollision - Turn off if not required.

DrawInteriorFaces - Uses a simple check during generation to remove faces that are deemed 'interior'. This will reduce the polycount of the mesh but at a cost at runtime.

## Blueprintable and Extendable
Notable Variables
```
MeshGenerationProgress - Scalar between 0 - 1, can be used to monitor the mesh progress by actors and materials.
OverrideStaticMesh - Set before generation to skip mesh generation and use the specified Static Mesh instead.
CorrectTextureMipsAndCompressionSettings - Will attempt to correct for incorrect texture settings. Disable to skip.
```
Blueprintable
```
URuntimePixelMeshComponent
```
Blueprint Callable
```
Generate3DPixelMesh() <-- Will regenerate the current mesh
GetSourceTexture()
SetSourceTexture() <-- use to initiate initial generation on delayed meshes
SpawnPixelMeshActorHelper() <-- Static Blueprint Function
```

Blueprint NativeEvents
```
OnPixelMeshComplete()
```

Blueprint/C++ Delegates
```
OnPixelMeshCompleteDelegate
```

## Component Variables
![Alt text](/PixelMeshSettings.PNG?raw=true "Component Variables")
