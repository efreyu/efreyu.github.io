---
title: Enhance Your Unreal Engine Workflow with Live Templates in Rider and CLion
date: 2024-05-27 14:40:48
tags: Unreal Engine, C++, GameDev
categories: Unreal Engine, Programming
---

### Introduction
Working with Unreal Engine can be a complex yet rewarding experience for game developers. Streamlining your development process is key to maintaining productivity and creativity. 
One effective way to achieve this is by leveraging live templates in Rider and CLion. These powerful tools can automate repetitive tasks, reduce errors, and significantly speed up your workflow. 
In this post, we'll explore some of the most useful live templates for Unreal Engine development and in general, and how to set them up in Rider and CLion to enhance your coding efficiency.

### What are Live Templates?
Live templates are predefined code snippets that can be quickly inserted into your code by typing a short abbreviation and pressing a designated key combination. They help automate repetitive coding tasks, enforce coding standards, and reduce the chance of errors. In the context of Rider and CLion, live templates can be customized to suit your specific needs, making your development process more efficient and streamlined.
And here is a base example of a live template in Rider:
![Basic Live Template usage](assets/live-templates-ue-rider-clion/live-templates-ue-rider-clion-1.gif)

### Setting Up Live Templates in Rider

First you need to go to Rider/Clion settings **File | Settings** and then navigate to **Editor | Live Templates**. Here you can see all the available live templates and create new ones.
Many live templates are already available out of the box, but you can also create your own custom templates. To create a new live template, click the **+** button and select the desired context in which the template should be available (e.g., C++, Unreal Engine, etc.). You can then define the abbreviation, description, and template text. You can also specify variables that will be replaced with actual values when the template is expanded.
For example, you can create a live template for a simple `UE_LOG` statement in C++:
1. Click the **+** button to create a new live template.
2. Choose option `Live Template`.
3. Set the abbreviation to `uelog` or `ulog` as you wish.
4. Set the description (optional).
5. Put into `Template text` the following code snippet:
```sh
UE_LOG($LOG_CATEGORY$, $LOG_VERBOSITY$, TEXT("Actor: '%s', Debug info %s"), *GetName(), $END$);
```
6. At the bottom you can see `Define` section where you can define variables. Click and choose `Other`.
7. Click button `Save`.

So by typing `uelog` and pressing `Tab` you will get the following code snippet, and caret will be placed at the variable `$END$`. 
We need to add additional variables to the template to make it more useful. For example, we can add variables for the log category and verbosity level, we can achieve this by adding the following variables to the template:
1. Click button `Edit variables`.
2. For the row `LOG_CATEGORY` set expression to `enum("LogTemp", "LogClass")`, default variable `LogTemp` and check `Skip if defined`.
3. For the row `LOG_VERBOSITY` set expression to `enum("Warning", "Error", "Fatal", "Display", "All")`, default variable `Warning` and check `Skip if defined`.
4. Click buttons `OK` and `Save`.
![Base snippet configuration, settings window](assets/live-templates-ue-rider-clion/live-templates-ue-rider-clion-2.png)
![Base snippet configuration, edit variables window](assets/live-templates-ue-rider-clion/live-templates-ue-rider-clion-3.png)