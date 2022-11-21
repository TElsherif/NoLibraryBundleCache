# NoLibraryBundleCache
This Unity project aims to investigate and test the isolation of all AssetBundles' BuildCache and artifacts outside of the Library folder. This project uses the **Addressables** and **ScriptableBuildPipeline** APIs.

* Unity version: *2020.3 LTS*
* ScriptableBuildPipeline version: *1.19.2*
* Addressables version: *1.18.19*

## Guide

In order to build Addressables with the custom Library settings, select *Window > Asset Management > Addressables > Build Addressables only*. After building, you'll find all the produced cache and artifacts stored in *"/CustomLibrary/"* folder instead of the deafult *"/Library/"* folder.

### How to implement this approach in your Unity project

1. Create a custom build script that uses the [Addressables](https://docs.unity3d.com/Packages/com.unity.addressables@1.20/manual/index.html) API with a custom build function that is exposed to the Editor. Detailed approach can be found [here](https://docs.unity3d.com/Packages/com.unity.addressables@1.20/manual/BuildPlayerContent.html).
2. Before calling [AddressableAssetSettings.BuildPlayerContent](https://docs.unity3d.com/Packages/com.unity.addressables@1.20/api/UnityEditor.AddressableAssets.Settings.AddressableAssetSettings.BuildPlayerContent.html), set the [Addressables.LibraryPath](https://docs.unity3d.com/Packages/com.unity.addressables@1.18/api/UnityEngine.AddressableAssets.Addressables.LibraryPath.html) to the desired location (e.g Addressables.LibraryPath = "CustomLibrary/") This will change location of the produced artifacts from the Assetbundle build (e.g. all platform specific artifacts).
3. Then you'll need to allow modification in the [ScriptableBuildPipeline](https://docs.unity3d.com/Packages/com.unity.scriptablebuildpipeline@1.19/manual/index.html) package. Moving the *com.unity.scriptablebuildpipeline* folder from *“Library/PackageCache"* to *“Packages”* does the trick.
4. Inside the ScriptableBuildPipeline package, modify the *k_CachePath* string in *BuildCache.cs* to the desired path. (e.g. k_CachePath = "CustomLibrary/BuildCache/"). This ensures the AsseBundle’s BuildCache will be produced in the desired folder.
5. Build the bundles using the custom build function.

## Notes

* *Assets are taken from [AddressablesMemoryOptimizations](https://github.com/patrickdevarney/AddressablesMemoryOptimizations) project.*
* *Extra Assets used from this [Asset Store package](https://assetstore.unity.com/packages/3d/environments/historic/mountainpalace-pack-153961).*
