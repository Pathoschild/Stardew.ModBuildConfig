**Stardew.ModBuildConfig** is an open-source NuGet package which automates the build configuration
for crossplatform [Stardew Valley](http://stardewvalley.net/) mods that use SMAPI.

## Contents
* [Usage](#usage)
* [Installation](#installation)
* [Configuration](#configuration)
* [Simplify mod development](#simplify-mod-development)

## Usage
Basically this package lets you write your mod once, and compile it on any computer. It detects
your current platform (Linux, Mac, or Windows) and game path, and injects the right references
automatically. You can also target a specific platform to create a mod package compatible with that
platform.

More specifically, the configuration...

1. detects the operating system and Stardew Valley path;
2. injects the right references to Stardew Valley, SMAPI, and XNA/MonoGame for your platform;
3. configures Visual Studio so you can launch the game for debugging (_Windows only_);
4. and adds a `GamePath` variable which can be used to script mod packaging if desired.

## Installation
### Creating a new mod
1. Create an empty library project.
2. Reference the [`Pathoschild.Stardew.ModBuildConfig` NuGet package](https://www.nuget.org/packages/Pathoschild.Stardew.ModBuildConfig).
3. [Write your code](http://canimod.com/guides/creating-a-smapi-mod).
4. Compile on any platform.

### Migrating an existing mod
1. Remove any references to `Microsoft.Xna.*`, Stardew Valley, `StardewModdingAPI`, and xTile.
2. Reference the [`Pathoschild.Stardew.ModBuildConfig` NuGet package](https://www.nuget.org/packages/Pathoschild.Stardew.ModBuildConfig).
3. Compile on any platform.

## Configuration
### Custom game path
If you customised where Stardew Valley is installed, you can specify where it is.

1. Get the full path to the directory containing the Stardew Valley executable.
2. Add this section to your `.csproj` file (anywhere before the added `<Import` line):
   
   ```
   <PropertyGroup>
     <GamePath>C:\Program Files (x86)\GalaxyClient\Games\Stardew Valley</GamePath>
   </PropertyGroup>
   ```

The configuration will check your custom path first, then fall back to the default paths. (That way
you can still compile it normally on a different computer.)

## Simplify mod development
### Package your mod into the game directory automatically
During development, it's helpful to have the mod files packaged into your `Mods` directory automatically each time you build. To do that:

1. Edit your mod's `.csproj` file.
2. Add this block of code at the bottom, right above the closing `</Project>` tag:

   ```cs
   <Target Name="AfterBuild">
      <PropertyGroup>
         <ModPath>$(GamePath)\Mods\$(TargetName)</ModPath>
      </PropertyGroup>
      <Copy SourceFiles="$(TargetDir)\$(TargetName).dll" DestinationFolder="$(ModPath)" />
      <Copy SourceFiles="$(TargetDir)\$(TargetName).pdb" DestinationFolder="$(ModPath)" Condition="Exists('$(TargetDir)\$(TargetName).pdb')" />
      <Copy SourceFiles="$(TargetDir)\$(TargetName).dll.mdb" DestinationFolder="$(ModPath)" Condition="Exists('$(TargetDir)\$(TargetName).dll.mdb')" />
      <Copy SourceFiles="$(ProjectDir)manifest.json" DestinationFolder="$(ModPath)" />
   </Target>
   ```
3. Optionally, edit the `<ModPath>` value to change the name, or add any additional files your mod needs.

That's it! Each time you build, the files in `<game path>\Mods\<mod name>` will be updated.

### Debugging
Debugging into your mod code when the game is running is pretty straightforward, since this package injects some of the configuration automatically. To do that:

1. [Package your mod into the game directory automatically](#package-your-mod-into-the-game-directory-automatically).
2. Launch the project with debugging in Visual Studio or MonoDevelop.

This will deploy your mod files into the game directory, launch SMAPI, and attach a debugger automatically. Now you can step through your code, set breakpoints, etc.

## Versions
1.3:
* Fixed non-default game paths on 32-bit Windows.
* Removed support for SilVerPLuM (discontinued).
* Removed support for overriding the target platform (never used).

1.2:
* Added support for non-default game paths on Windows by reading the registry.

1.1:
* Added support for overriding the target platform.

1.0:
* Initial release.
* Added support for detecting the game path automatically.
* Added support for injecting XNA/MonoGame references automatically based on the OS.
* Added support for mod builders like SilVerPLuM.

## See also
* [NuGet package](https://www.nuget.org/packages/Pathoschild.Stardew.ModBuildConfig)
* <s>Discussion thread</s>
