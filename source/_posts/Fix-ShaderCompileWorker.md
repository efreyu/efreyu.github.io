---
title: Fix ShaderCompileWorker issue in Unreal Engine project
date: 2024-06-07 22:04:11
tags: ["Unreal Engine", "C++", "GameDev"]
categories: ["Unreal Engine", "Programming"]
image: assets/fix_shader_compile_worker/fix_shader_compile_worker_image.png
---
![Preview](assets/fix_shader_compile_worker/fix_shader_compile_worker_image.png)

When working on Unreal Engine projects, you may encounter an issue where the ShaderCompileWorker process consumes a large amount of CPU resources, causing your system to slow down. This can be frustrating, especially when you need to compile shaders frequently during development.
Here is a quick fix to resolve this issue and improve the performance of your Unreal Engine project.
- Если проблема выглядит так де как на картинке выше то попробуйте проверить следующие варианты решения:
- Убелитесь что в папке проекта в файле Config/DefaultEngine.ini прописанна корректная строка *DefaultGraphicsRHI=DefaultGraphicsRHI_DX12* для *Windows* или *DefaultGraphicsRHI=DefaultGraphicsRHI_Vulkan* для *Linux/MacOs*. 
- Если проблема только в одном проекте - попробуйте удалить папку Intermediate Build Binaries  и перекомпилировать проект.
- Если проблема во всех проектах - попробуйте удалить папку DDC и перекомпилировать проект.