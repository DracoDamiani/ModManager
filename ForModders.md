# Modder Info
Mod Manager tries to read version information from a `Manifest.xml` file. This file should be located in the `About` folder, next to `About.xml`. A manifest will look something like this;

``` xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Manifest>
    <identifier>ModManager</identifier>
    <version>0.1.0.0</version>
    <dependencies>
        <li>FrameWorkMod</li>
        <li>ModThatDoesntExist</li>
        <li>SomeMod >= 4.0</li>
        <li>SomeOtherMod <= 2.0</li>
        <li>YetAnotherMod == 4.3.0.0</li>
        <li>SpecificRangeMod >= 2.0</li>
        <li>SpecificRangeMod <= 2.999.999.999</li>
    </dependencies>
    <incompatibleWith>
        <li>SomeOtherMod</li>
    </incompatibleWith>
    <loadBefore>
        <li>HowManyModsDoesOneNeed >= 9000.1</li>
    </loadBefore>
    <loadAfter>
        <li>ToModOrNotToMod</li>
    </loadAfter>
    <manifestUri>https://rawgit.com/FluffierThanThou/ModManager/master/About/Manifest.xml</manifestUri>
    <downloadUri>https://github.com/FluffierThanThou/ModManager/releases/latest</downloadUri>
</Manifest>
```

# Manifest properties
## identifier
A unique identifier for your mod. We need this because the mod's name might be too long, and the in-game identifier is based on the mod's folder name - which is different for steam and local mods, as well as for local copies made by Mod Manager. 

*NOTE: Identifiers should never contain spaces.*

### resolving identifiers
Most of the manifests' properties use identifiers to identify other mods. Since not all mods will have a manifest, we have to be creative in resolving identifiers. Mod Manager attempts to match mods to identifiers in this order;
 - their manifest identifier.
 - their name, stripped of all spaces.
 - their mod identifier (folder name), stripped of all spaces.

## version
A 2 to 4 element version number, as implemented in [System.Version](https://docs.microsoft.com/en-us/dotnet/api/system.version?view=netframework-3.5). Versions may only contain numbers, separated by periods, in the format `major.minor.build.revision`. Alphabetical characters (e.g. `v1.0` or `0.2-beta`) are **not allowed**.

*NOTE: `2.0.0.0` is **not the same** as `2.0`, any version elements that are `null` are considered to be lower than `0`.*

## dependencies
Any number of dependencies may be given in the format `identifier [operator version]`. An [identifier](#identifier) is required, the [operator](#operator) and [version](#version) are optional - but if either is given, both must be given. See [resolving identifiers](#resolving-identifiers) on how mods are matched to identifiers.

*NOTE: creating a dependency for a mod also implies [loadAfter](#loadAfter) for that mod.*

### operator
A dependency can use one of the following operators; `==`, `>=` or `<=` to specify an *exact* version, *greater or equal* version or *lesser or equal* version respectively. You can use multiple dependencies on the same mod to specify a specific range.

## incompatibleWith
A list of [dependencies](#dependencies) that your mods conflict with, and that never should be loaded together. See [resolving identifiers](#resolving-identifiers) on how mods are matched to identifiers.

## loadBefore
A list of optional [dependencies](#dependencies) that should be loaded _after_ your mod (i.e. your mod should load _before_ these mods), if they are present. Can be used to help resolve potential mod conflicts, without creating an explicit dependency. See [resolving identifiers](#resolving-identifiers) on how mods are matched to identifiers.

## loadAfter
A list of optional [dependencies](#dependencies) that should be loaded _before_ your mod (i.e. your mod should load _after_ these mods), if they are present. Can be used to help resolve potential mod conflicts, without creating an explicit dependency. See [resolving identifiers](#resolving-identifiers) on how mods are matched to identifiers.

## manifestUri
[URI](https://docs.microsoft.com/en-us/dotnet/api/system.uri?view=netframework-3.5) to an online, up-to-date version of the `Manifest.xml`. Typically, this will be in your mods' github repository, but Mod Manager makes no assumptions on where the manifest is located - as long as it can be downloaded directly, and is not hidden behind a login, paywall, etc.

## downloadUri
[URI](https://docs.microsoft.com/en-us/dotnet/api/system.uri?view=netframework-3.5) to a download location for the latest version of this mod. Mod Manager does not (yet) download, extract or install mods directly, but will open a browser tab to this location (if requested by the user).