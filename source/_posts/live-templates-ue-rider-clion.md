---
title: Enhance Your Unreal Engine Workflow with Live Templates in Rider and CLion
date: 2024-05-27 14:40:48
tags: ["Unreal Engine", "C++", "GameDev"]
categories: ["Unreal Engine", "Programming"]
---
Working with Unreal Engine can be a complex yet rewarding experience for game developers. Streamlining your development process is key to maintaining productivity and creativity. One effective way to achieve this is by leveraging live templates in Rider and CLion. These powerful tools can automate repetitive tasks, reduce errors, and significantly speed up your workflow. 
In this post, we'll explore some of the most useful live templates that I use for Unreal Engine development and in general, and how to set them up in Rider and CLion to enhance your coding efficiency.

### What are Live Templates?

Live templates are predefined code snippets that can be quickly inserted into your code by typing a short abbreviation and pressing a designated key combination. They help automate repetitive coding tasks, enforce coding standards, and reduce the chance of errors. In the context of Rider and CLion, live templates can be customized to suit your specific needs, making your development process more efficient and streamlined.
And here is a base example of a live template in Rider:
![Basic Live Template usage](assets/live-templates-ue-rider-clion/live-templates-ue-rider-clion-1-updated.gif)

### Setting Up Live Templates in Rider and CLion

First you need to go to Rider/Clion settings **File | Settings** and then navigate to **Editor | Live Templates**. Here you can see all the available live templates and create new ones.
Many live templates are already available out of the box, but you can also create your own custom templates. To create a new live template, click the **+** button and select the desired context in which the template should be available (e.g., C++, Unreal Engine, etc.). You can then define the abbreviation, description, and template text. You can also specify variables that will be replaced with actual values when the template is expanded.
For example, you can create a live template for a simple `UE_LOG` statement in C++:
1. Click the **+** button to create a new live template.
2. Choose option `Live Template`.
3. Set the abbreviation to `uelog` or `ulog` as you wish.
4. Set the description (optional).
5. Put into `Template text` the following code snippet:
```sh
UE_LOG($LOG_CATEGORY$, $LOG_VERBOSITY$, TEXT("Actor: '%s', Debug info $TYPE$"), *GetName(), $END$);
```
6. At the bottom you can see `Define` section where you can define variables. Click and choose `Other`.
7. Click the button `Save`.

So by typing `uelog` and pressing `Tab` you will get the following code snippet, and caret will be placed at the variable `$END$`. 
We need to add additional variables to the template to make it more useful. For example, we can add variables for the log category and verbosity level, we can achieve this by adding the following variables to the template:
1. Click the button `Edit variables`.
2. For the row `LOG_CATEGORY` set expression to `enum("LogTemp", "LogClass")`, default variable `LogTemp`.
3. For the row `LOG_VERBOSITY` set expression to `enum("Warning", "Error", "Fatal", "Display", "All")`, default variable `Warning`.
4. For the row `TYPE` set expression to `enum("%s", "%f", "%d", "empty")`, default variable `%s`.
4. Click the buttons `OK` and `Save`.
You can add more items to `enum()` variables to the template and modify the text message to make it even more flexible.
![Base snippet configuration, settings window](assets/live-templates-ue-rider-clion/live-templates-ue-rider-clion-2-updated.png)
![Base snippet configuration, edit variables window](assets/live-templates-ue-rider-clion/live-templates-ue-rider-clion-3-updated.png)

### Adding `UPROPERTY()` snippet

By using the previous steps you can create a live template for `UPROPERTY()` macro. Here is an example of how you can set it up:
1. Create a new live template with the name `uprop` or `uproperty` as you wish.
2. Add this to the `Template text`:
```sh
UPROPERTY($FIRST$, $SECOND$, Category = "Category|Sub-category", 
	Meta = (DisplayName = "Custom Name", EditConditionHides, EditCondition = "bCustomBoolVar"))
$END$
```
3. Add variables:
   - `FIRST` with expression `enum("EditAnywhere", "VisibleAnywhere", "EditDefaultsOnly", "VisibleDefaultOnly", "EditInstanceOnly", "VisibleInstanceOnly")`, default variable `EditAnywhere`.
   - `SECOND` with expression `enum("BlueprintReadOnly", "BlueprintReadWrite", "EditFixedSize", "EditInline")`, default variable `BlueprintReadWrite`.


### Adding `UENUM()` snippet

1. Create a new live template with the name `uenum`.
2. Add this to the `Template text`:
```sh
UENUM(BlueprintType)
enum class E$END$ : uint8 {
    First UMETA(DisplayName = "First")
};
```

### Adding `UFUNCTION()` snippet

1. Create a new live template with the name `ufunc` or `ufunction` as you wish.
2. Add this to the `Template text`:
```sh
UFUNCTION($FIRST$, Category = "Category")
void $END$();
```
3. Add variable:
    - `FIRST` with expression `enum("BlueprintCallable", "BlueprintPure", "BlueprintImplementableEvent", "BlueprintNativeEvent")`, default variable `BlueprintCallable`.

### Adding general useful `todo` message

1. Create a new live template with the name `todo`.
2. Add this to the `Template text`:
```sh
//TODO author: $USER$, date: $DATE$ $TIME$, attention: $PRIORITY$ 
// - $END$
```
3. Add variable:
    - `USER` with expression `user()`.
    - `DATE` with expression `date()`.
    - `TIME` with expression `time()`.
    - `PRIORITY` with expression `enum("Fix", "Feature", "Warning", "Possible error")`, default variable `Fix`.

### Conclusion

Incorporating live templates into your Unreal Engine development workflow using Rider and CLion can significantly enhance your productivity and code quality. These predefined snippets automate repetitive tasks, enforce coding standards, and reduce the likelihood of errors, ultimately streamlining your development process. By customizing live templates to fit your specific needs, you can save valuable time and focus more on the creative aspects of game development.
From setting up basic snippets like `UE_LOG()` statements to more complex macros like `UPROPERTY()`, `UENUM()`, and `UFUNCTION()`, the examples provided demonstrate the flexibility and power of live templates. Additionally, the ability to create personalized `TODO` messages ensures that your code documentation remains consistent and informative.
Adopting these practices will not only improve your efficiency but also contribute to a more organized and maintainable codebase. By leveraging the full potential of live templates, you can optimize your workflow and make the most out of your development tools.