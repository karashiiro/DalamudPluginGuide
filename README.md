# Dalamud Plugin Guide
This minibook provides a brief explanation of how to create Dalamud plugins.

### Prerequisites
 * [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) with the .NET Framework 4.7.2 targetting pack

### Full Examples
Also see [SamplePlugin](https://github.com/ff-meli/SamplePlugin) and other existing plugins for real examples.

## Creating a project
There are currently two main methods for creating a project:
 * [From scratch](#from-scratch)
 * [Using DalamudPluginProject as a base](#using-dalamudpluginproject-as-a-base)

## From scratch
Open Visual Studio and create a new project. Select **Class Library (.NET Standard)** from the list. Set the
solution name to your plugin's name.

All C# projects are structured according to their `.csproj` files. .NET Framework projects have an old `.csproj`
format that looks awful and is harder to edit than the "SDK-style" equivalent that .NET Standard projects start
with, so we're going to create a .NET Framework project from a .NET Standard project.

Open your `.csproj` by double-clicking on it.

Set the `TargetFramework` tag to `net472`. While we're here, add the following line inside of that `PropertyGroup`:

```xml
<LangVersion>9.0</LangVersion>
```

[!Screenshot assets/open_csproj.png]

Setting `LangVersion` allows you to take advantage of new features in C# that would normally be disabled in .NET
Framework projects.

Before we begin programming, we also need to add project references to Dalamud, so that we have access to its types.
Right-click on "Dependencies" and click "Add Assembly reference...". Click Browse and navigate to
`%AppData%/XIVLauncher/addon/Hooks`. Add the following files to your project:
 * `Dalamud.dll`
 * `ImGui.NET.dll`
 * `ImGuiScene.dll`
 * `Lumina.dll`
 * `Lumina.Generated.dll`
 * `Newtonsoft.Json.dll`

Now we need to set up our code files. Right-click on the precreated `Class1.cs` and rename it to your own plugin's name.
Then confirm that you would like to rename every instance of `Class1` in your solution to your plugin's name.

Now we can program.

All Dalamud plugins implement the `IDalamudPlugin` interface. Do the same for yours:

[!Screenshot assets/idalamudplugin.png]

This should redline, noting that you have not actually implemented the interface. Hover over `IDalamudPlugin` until
the lightbulb appears, and click on it. Select "Implement interface" to generate stubs for the required class
members. Let's go over them one-by-one.

### `Name`
This is the property that your plugin's name is read from. It does not need to match your exact plugin name, but
there's generally no reason not to set it as such. Whatever you put here will show up in the chat log when your
plugin loads.

[!Screenshot assets/name_property.png]

### `Initialize()`
`Dispose()` is generated above this, but it makes more sense to explain `Initialize()` first. `Initialize()` runs
immediately when your plugin loads. Use this like a constructor. You'll want to save the plugin interface that
is passed in so you can do useful things with it later.

[!Screenshot assets/initialize.png]

### `Dispose()`
`Dispose()` is run when your plugin is unloaded. If you add any event hooks (discussed later) in your initializer,
you'll want to remove them here in order to avoid interfering with other plugins. If any resources you are using
have not been disposed yet (such as the plugin interface), you'll also dispose those here by calling their
respective `Dispose()` methods.

[!Screenshot assets/dispose.png]

### Moving on
We now have the bare-minimum of a Dalamud plugin. If we build this now and load it into Dalamud (using the
`%AppData%/XIVLauncher/devPlugins` folder), its name should be printed in the chat log when we restart our game.
We probably want to do something interesting with our plugin, though. Let's see how to do that in the
[next section](sections/chat_log.md).


## Using DalamudPluginProject as a base
Navigate to the [DalamudPluginProject](https://github.com/karashiiro/DalamudPluginProjectTemplate) repository
and either create your own repository from it, clone it down, and open the solution file, or download the zip
of the [latest release](https://github.com/karashiiro/DalamudPluginProjectTemplate/releases) and put it in your
`Documents\Visual Studio 2019\Templates\ProjectTemplates` folder and create a project from it in Visual Studio.

Make sure to change the values in `Plugin.json` to suit your own project, and rename the code files to include
your plugin name if you like.

Let's [move on](sections/chat_log.md).
