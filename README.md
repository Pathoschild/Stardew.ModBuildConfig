**Stardew.ModBuildConfig** is an open-source NuGet package which automates the build configuration
for crossplatform [Stardew Valley](http://stardewvalley.net/) mods that use SMAPI.

The configuration...

1. detects the operating system (Linux, Mac, or Windows) and the Stardew Valley install path;
2. injects the correct references to Stardew Valley, SMAPI, and XNA/MonoGame;
3. (on Windows) configures Visual Studio so you can launch the game for debugging;
4. and adds a `GamePath` variable which can be used to automate mod installation during testing
   if desired.

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
If you customised where Stardew Valley is installed, you can add your path to the list to try.

1. Get the full path to the directory containing the Stardew Valley executable.
2. Add this section to your `.csproj` file (anywhere before the added `<Import` line):
   
   ```
   <PropertyGroup>
     <GamePath>C:\Program Files (x86)\GalaxyClient\Games\Stardew Valley</GamePath>
   </PropertyGroup>
   ```

### Compatibility with mod builders
The configuration is designed for compatibility with third-party mod compilers. [Silverplum](https://github.com/rumangerst/SilVerPLuM)
is officially supported, and you can inject the `GAMEPATH` environment variable to override the
detected game path.

## See also
* [NuGet package](https://www.nuget.org/packages/Pathoschild.Stardew.ModBuildConfig)
* <s>Discussion thread</s>