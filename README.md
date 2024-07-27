# Pets Enhanced Mod Content Pack Guide
A comprehensive guide on how to install and/or create content packs for the Stardew Valley mod "Pet's Enhanced - A Pet Mechanic Overhaul"
## Contents
* [Installation](#installation)
* [Getting Started](#getting-started)
* [Creating the Content Pack](#creating-the-content-pack)
* [Implementing Logic](#implementing-logic)
* * [EditContent](#editcontent)
* * <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/edit/main/README.md#editcontent--entries">EditContent • Entries</a>
<br><br/>
  
## Installation
1. Install the latest version of [SMAPI](https://www.nexusmods.com/stardewvalley/mods/2400).
2. Install the latest version of [Pet's Enhanced Mod](https://www.nexusmods.com/stardewvalley/mods/24026) on [Nexus Mods](https://www.nexusmods.com/stardewvalley).
3. Unzip any Pet's Enhanced Mod content packs into `Mods` to install them.
4. Run the game using SMAPI.

That's it! Content packs unzipped into `Mods` will be loaded and applied automatically.
<br><br/>

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
<br><br/>

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

For that, we need to create the `content.json` file, which will contain all our logic for the content pack.

```js
{

  // Here goes the content pack logic...
  //------------------------------------
  //          ----------------


}
```
Which should look like this for now.
<br><br/>

## Implementing Logic

When creating a content pack you might want to do one of two things: Edit content from the mod, or add content to the mod.

To help achieve both things, the mod provides two constructors called: `EditContent` and `AddContent`.

Their purpose is to help organize the logic into separate blocks.

```js
{
  "EditContent": [
    {

      // EditContent Action block Nº1

      // Here goes the logic...
      //-----------------------

    },
    {

      // EditContent Action block Nº2

      // Here goes the logic...
      //-----------------------

    }
  ]
}
```
Each block is an independent Action that provides the mod with instructions that help apply the changes correctly.
<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
<tr>
<td><code>AddContent</code></td><td>Include modded pets into the Internal Content Library of the mod.
<br><br/>
<em>*By default, the mod ignores any modded pet whose texture is not present on the Library.
<br><br/>
This is because the mod requires a lot of information in order to work correctly.
<br>For example: What commands can the pet learn, or what items can it eat.<br/>
<br>The mod also uses different sprite sheets for the cat & dog, which contains frames that aren't present on vanilla. This causes visual bugs due to missing frames.<br/></em>
</td>
</tr>
<tr>
<td><code>EditContent</code></td><td>Edit pets already present on the Internal Content Library of the mod.</td>
</tr>
</table>
<br><br/>

### EditContent

The basic structure of an EditContent Action block usually looks like this:

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "Entries":
        {

          //More instructions here...
          //-------------------------
        
      }
    }
  ]
}
```
As you can see, it is made out of 4 fields: `PatchPriority`, `TargetTexture`, `FromTexture` and `Entries`.

Each of them have their own purposes:

<table>
<tr>
<th>Field</th>
<th>Description</th>
<th>IsRequired</th>
</tr>
<tr>
<td><code>PatchPriority</code></td><td>Set this action's priority over its "TargetTexture". <br></br><em>*If another content pack targets the same texture as this one, whichever has the highest </em><code>PatchPriority</code><em> will be applied first.</em></td><td>Yes</td>
</tr>
<tr>
<td><code>TargetTexture</code></td><td>The texture that will be affected by our changes. 
<br><br/> 
<em>*The mod uses textures to identify which pet to patch. As such, all pets that share the same texture will be patched equal.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/Guides/EditContent.md">Guides/EditContent.TargetTexture</a> for more info.
</td><td>Yes</td>
</tr>
<tr>
<td><code>FromTexture</code></td><td>The path to the new texture that'll replace the "TargetTexture".<br><br/><em>*This literally replaces the previous texture with a new one.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/Guides/EditContent.md">Guides/EditContent.FromTexture</a> for more info.
<td>No</td>
</tr>
<tr>
<td><code>Entries</code></td><td>Contains several other methods that you can use to achieve more specific results.<br></br><em>*Such as: Add EdibleItems, replace pet Commands or modify the AttackModel for example.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/Guides/EditContent.md">Guides/EditContent.Entries</a> for more info.
<td>No</td>
</tr>
</table>

### EditContent • Entries

## See also
* Pet's Enhanced Mod Content packs in [Nexus Mods](https://www.nexusmods.com/stardewvalley/mods/categories/8/)
* My [BuyMeACoffee](https://buymeacoffee.com/sunkenlace) page, if you wanna support me.
