# Pets Enhanced Mod Content Pack Guide
A comprehensive guide on how to install and/or create content packs for the Stardew Valley mod "Pet's Enhanced - A Pet Mechanic Overhaul"
## Contents
* [Installation](#installation)
* [Getting Started](#getting-started)
* [Creating the Content Pack](#creating-the-content-pack)
  
## Installation
1. Install the latest version of [SMAPI](https://www.nexusmods.com/stardewvalley/mods/2400).
2. Install the latest version of [Pet's Enhanced Mod](https://www.nexusmods.com/stardewvalley/mods/24026) on [Nexus Mods](https://www.nexusmods.com/stardewvalley).
3. Unzip any Pet's Enhanced Mod content packs into `Mods` to install them.
4. Run the game using SMAPI.

That's it! Content packs unzipped into `Mods` will be loaded and applied automatically.

## Getting Started
"The following guide is for mod authors only, if you're not a mod author or you don't plan to make a content pack yourself, the instructions above are everything you need to know, otherwise keep reading."

Before we start learning how to create a Content pack for Pet's Enhanced Mod, we first need to know how does a finished content pack for the mod look like:
```
📁 Mods/
   📁 [PEM] YourModName/
      🗎 content.json
      🗎 manifest.json
      📁 assets/
         🗎 exampleTexture.png
```
As you can see, it is pretty similar to how a content pack for Content Patcher looks like. Here is an example of how a simple `content.json` file for the mod looks like:

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat3" ],
      "FromTexture": [ "assets", "exampleTexture.png" ]
    }
  ]
}
```
What this does is replace the texture of the vanilla cat nº3 with `exampleTexture.png`. You can do this and much more with the help of this guide (via content packs).

For example: Edit the max damage of a pet, its hat offset relative to its current frame, change what commands a pet is able to learn, etc... 

## Creating the Content Pack

Let's start by creating a new folder inside the `Mods` folder, then create a new json file named `manifest.json` inside it.
```
📁 Mods/
   📁 [PEM] YourModName/
      🗎 manifest.json
```
The `manifest.json` should look like this:
```js
{
  "Name": "Write the name of your content pack here",
  "Author": "Your name",
  "Version": "1.0.0", 
  "Description": "Write your description",
  "UniqueID": "YourName.YourContentPackName",
  "MinimumApiVersion": "3.0.0",
  "UpdateKeys": [ "Nexus:???" ], //Replace "???" with the id of your mod on NexusMods, otherwise leave as it is.
  "ContentPackFor": {
    "UniqueID": "Sunken_Lace.PetsEnhancedCode",
    "MinimumVersion": "0.2.0"
  }

}
```
With this, SMAPI will now recognize your content pack. Of course were not done yet, you still need to write down the logic of your content pack, otherwise it is empty and won't work.

For that, we need to create the `content.json` file, which will contain all our logic for the content pack. It should look like this for now:

```js
{

  // Here goes the content pack logic...
  //------------------------------------
  //          ----------------


}
```

## See also
* Pet's Enhanced Mod Content packs in [Nexus Mods](https://www.nexusmods.com/stardewvalley/mods/categories/8/)
* My [BuyMeACoffee](https://buymeacoffee.com/sunkenlace) page, if you wanna support me.
