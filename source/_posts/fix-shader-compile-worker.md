---
title: Fix ShaderCompileWorker issue in Unreal Engine project
date: 2024-06-07 22:04:11
tags: ["Unreal Engine", "GameDev", "Optimization", "Python"]
categories: ["Unreal Engine"]

---

![Preview](assets/fix_shader_compile_worker/fix_shader_compile_worker_image.png)

When working on Unreal Engine projects, you may encounter an issue where the ShaderCompileWorker process consumes a large amount of CPU resources, causing your system to slow down significantly. This can be particularly frustrating when you need to compile shaders frequently during development. From my own experience with similar issues and after researching solutions on the official Unreal Engine forums, I have compiled several effective steps to address this problem and improve the performance of your Unreal Engine project.

#Step-by-Step Solutions to Optimize Shader Compilation Performance in Unreal Engine
## Verify Graphics RHI Configuration
Ensure that the correct Graphics RHI (Rendering Hardware Interface) settings are specified in your project's configuration file. This can prevent excessive CPU usage during shader compilation.
- Navigate to the *`Config/DefaultEngine.ini`* file in your project directory.
- In the section *`[Script/WindowsTargetPlatform.WindowsTargetSettings]`*, ensure the following settings are correctly specified:
- For Windows: *`DefaultGraphicsRHI=DefaultGraphicsRHI_DX12`*
- For Linux/MacOS: *`DefaultGraphicsRHI=DefaultGraphicsRHI_Vulkan`*
Properly setting these parameters ensures that Unreal Engine uses the appropriate graphics API, which can help mitigate high CPU usage during shader compilation and help to avoid ShaderCompileWorker issues.
## Clean Up Project-Specific Intermediate Files
If the issue persists in a specific project, try cleaning up intermediate files and recompiling the project:
Delete the following directories within your project folder:
- `Intermediate`
- `Build`
- `Binaries`
- `DerivedDataCache` 
After deleting these directories, recompile your project. This process helps in removing any corrupted or outdated cache files that might be causing the high CPU usage.
## Global DerivedDataCache Cleanup
If the problem occurs across all projects, consider deleting the global DerivedDataCache folder and recompiling your project:
- The `DerivedDataCache` folder can be located in one of the following paths:
  - *`C:\Users\YourUserName\AppData\Local\UnrealEngine\Common\DerivedDataCache`*
  - *`%PathToUnrealEngineFolder%\UE_5.4\Engine\DerivedDataCache`*
Deleting this folder forces Unreal Engine to regenerate shader cache, which can resolve issues related to excessive CPU usage.
## Automating DerivedDataCache Cleanup
To streamline the cleanup process, you can use a script to automate the deletion of the DerivedDataCache folder. Below is a sample Python script that can help you automate this process. 

Remember to replace the path to your Unreal Engine installation folder in the script:
```python
import os
import shutil
import glob

unreal_engine_root = "C:/Program Files/Epic Games/UE_5.4"

def remove_directory(directory):
    if os.path.exists(directory):
        try:
            shutil.rmtree(directory)
        except Exception as e:
            print(f"Failed to remove {directory}: {e}")
        print(f"Removed directory: {directory}")

def clean_project(project_dir):
    # Define directories and files to be cleaned
    intermediate_dir = os.path.join(project_dir, 'Intermediate')
    build_dir = os.path.join(project_dir, 'Build')
    binaries_dir = os.path.join(project_dir, 'Binaries')
    derived_data_cache_dir = os.path.join(project_dir, 'DerivedDataCache')
    local_derived_data_cache_dir = os.path.join(project_dir, 'LocalDerivedDataCache')
    saved_dir = os.path.join(project_dir, 'Saved')

    # Clean directories and files
    remove_directory(intermediate_dir)
    remove_directory(build_dir)
    remove_directory(binaries_dir)
    remove_directory(derived_data_cache_dir)
    remove_directory(local_derived_data_cache_dir)
    remove_directory(saved_dir)

def remove_temp_files(directory, unused_files=None):
    if unused_files is None:
        unused_files = ['.DS_Store', '._DS_Store']

    for root, dirs, files in os.walk(directory):
        for file in files:
            if file in unused_files:
                file_path = os.path.join(root, file)
                try:
                    os.remove(file_path)
                    print(f"Removed: {file_path}")
                except Exception as e:
                    print(f"Failed to remove {file_path}: {e}")

        for dir in dirs:
            remove_temp_files(os.path.join(root, dir), unused_files)

def cleanup_ddp_cache(project_dir, unreal_engine_path):
    derived_data_cache_dir = os.path.join(project_dir, 'DerivedDataCache')
    remove_directory(derived_data_cache_dir)

    local_derived_data_cache_dir = os.path.join(project_dir, 'LocalDerivedDataCache')
    remove_directory(local_derived_data_cache_dir)

    home_user_folder = os.path.expanduser("~")
    appdata_local_unreal_engine_cache = os.path.join(home_user_folder, 'AppData', 'Local', 'UnrealEngine', 'Common', 'DerivedDataCache')
    remove_directory(appdata_local_unreal_engine_cache)

    unreal_engine_cache = os.path.join(unreal_engine_path, 'Engine', 'DerivedDataCache')
    remove_directory(unreal_engine_cache)

if __name__ == "__main__":
    project_dir = os.path.dirname(os.path.abspath(__file__))
    clean_project(project_dir)
    remove_temp_files(project_dir)
    cleanup_ddp_cache(project_dir, unreal_engine_root)
```

By following these steps, you can effectively manage and resolve the high CPU usage issue caused by ShaderCompileWorker in Unreal Engine, ensuring smoother and more efficient development.

Useful links:
- [Unreal Engine Forum post "ShaderCompileWorker failed: Mismatched shader version"](https://forums.unrealengine.com/t/shadercompileworker-failed-mismatched-shader-version/438865)
- [Unreal Engine Forum post "Derived Data Cache Issue"](https://forums.unrealengine.com/t/derived-data-cache/324747)
- [Unreal Engine Forum post "Fix ShaderCompileWorker"](https://forums.unrealengine.com/t/fix-shadercompileworker/150306)