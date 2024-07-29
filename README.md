# Pets Enhanced Mod Content Pack Guide
A comprehensive guide on how to install and/or create content packs for the Stardew Valley mod "Pet's Enhanced - A Pet Mechanic Overhaul"
## Contents
* [Installation](#installation)
* [Getting Started](#getting-started)
* [Creating the Content Pack](#creating-the-content-pack)
* [Implementing Logic](#implementing-logic)
* * [EditContent](#editcontent)
* * * [EditContent • Entries](#editcontent--entries)
* * * [EditContent • Entries • AttackModel](#editcontent--entries--attackmodel)
* * [AddContent](#addcontent)
* * * [AddContent • Entries](#addcontent--entries)
* * * [AddContent • Entries • AttackModel](#addcontent--entries--attackmodel)
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
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#targettexture">Guides/TargetTexture</a> for more info.
</td><td>Yes</td>
</tr>
<tr>
<td><code>FromTexture</code></td><td>The path to the new texture that'll replace the "TargetTexture".<br><br/><em>*This literally replaces the previous texture with a new one.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#fromtexture">Guides/FromTexture</a> for more info.
<td>No</td>
</tr>
<tr>
<td><code>Entries</code></td><td>Contains several other methods that you can use to achieve more specific results.<br></br><em>*Such as: Add EdibleItems, replace pet Commands or modify the AttackModel for example.</em>
<td>No</td>
</tr>
</table>

### EditContent • Entries

The Entries property contains a series of methods and sub-methods that will help you edit content more easily.
<br>This is how it usually looks like:<br/>

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "dog3" ],
      "Entries": {
        "AddEdibleItems": [
          {
            "QualifiedItemID": "(O)211", //Pancakes
            "FriendshipPointsGained": 20
          }
        ],
        "RemoveCommands": [ "Search" ],
        "AttackModel": {
          "EditMinDamage": 5,
          "EditMaxDamage": 30,
          "EditIsViciousType": true
        }
      }
    }
  ]
}
```
<br><br/> 
Here's the list of methods:
<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
<tr>
<td><code>EditHatOffsetModel</code></td><td>Change this pet's previous HatOffSet model with the specified one.
<br>Example:<br/>
  
```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "dog2" ],
      "Entries": {
        "EditHatOffsetModel": "dog2"
      }
    }
  ]
}
```

<em>*A "HatOffsetModel" is a pre-made HatOffset dictionary you can use to avoid writing a new one yourself.<br>If your want to write the HatOffset dictionary yourself, use "custom".<br/></em><br><br/> 
Valid inputs: dog, dog1, dog2, dog3, dog4, dog5, cat, custom.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset-&-hatoffsetmodel">Guides/HatOffset & HatOffsetModel</a> for more info.
</tr>
<tr>
<td><code>AddHatOffset</code>, <code>EditHatOffset</code> & <code>ReplaceHatOffset</code></td><td>Add, edit or replace the entire HatOffset dictionary of this pet with the one provided.
<br>Example:<br/>

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "EditHatOffset": {    // AddHatOffset and ReplaceHatOffset follow the same structure! 
          "1, 2": [ 4, 4, 2 ],
          "35, 6, 23": [ 3, 4, 0 ],
          "24": [ 3.5, -10, 1 ]
        }
      }
    }
  ]
}
```

<em>*A "HatOffset" dictionary contains instructions the mod needs in order for the pet's Hat to be displayed correctly on its texture (on a specific frame).<br><br/> The mod uses coordinates relative to the center of the pet's sprite to position the hat on top of the pet.<br><br/>As such, it needs to know the direction of the hat and the position it should be at a certain frame. <br>For example: At frame 24, the hat should be 3.5px right, 10px higher, and it should be facing East.<br/></em>
<br><br/>A key in a HatOffset dictionary looks like this:
```js
//Directions: 0(North), 1(East), 2(South), 3(West).
"frame": [ xOffset, yOffset, direction]
```
<em>**Note that the "y" axis is inverted, which means that "-10" is read as: "Ten pixels up" instead.</em>
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset-&-hatoffsetmodel">Guides/HatOffset & HatOffsetModel</a> for more info.

</td>
</tr>
<tr><td><code>RemoveHatOffsetFrames</code></td><td>Remove a list of frames already present from this pet's HatOffset dictionary. 
<br></br>
Example:
  
```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "RemoveHatOffsetFrames": [ 1, 2, 35, 6, 23 ]

      }
    }
  ]
}
```
</td></tr>
<tr>
<td><code>AddCommands</code>, <code>RemoveCommands</code> & <code>ReplaceCommands</code></td>
<td>Add, remove or replace the entire list of commands inside the pet's CommandList.
<br></br>
Example:

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "AddCommands": [ "Search", "Wait" ]
        // Remove/ReplaceCommands follow the same structure!
      }
    }
  ]
}
```
<em>*Note that 'commands' it's refering to the tricks the pet can use and learn.</em>
<br><br/> 
Valid inputs: Wait, Hunt, Follow, Search.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#commands">Guides/Commands</a> for more info.
</td>
</tr>
<tr>
<td><code>AddEdibleItems</code> & <code>ReplaceEdibleItems</code></td>
<td>Add or replace the entire list of edible items inside the pet's EdibleItemList.
<br></br>
Example:

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "ReplaceEdibleItems": [     // AddEdibleItems also follows the same structure!
          {
            "QualifiedItemID": "(O)139", //Salmon
            "FriendshipPointsGained": 20
          },
          {
            "QualifiedItemID": "(O)130", //Tuna
            "FriendshipPointsGained": 20
          }
        ]
      }
    }
  ]
}
```
<em>*Edible items refer to what items can be gifted to pets regardless of whether the pet likes them or not.</em>
<br><br/> 
An element inside an EdibleItem list typically looks like this:

```js
{
  "QualifiedItemID": "(O)139", //Look it up at the stardew valley wiki to see more info.
  "FriendshipPointsGained": 20 //The usual amount of FP gained by petting a pet is 12. 
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#edibleitems">Guides/EdibleItems</a> for more info.
</td>
</tr>
<tr><td><code>RemoveEdibleItems</code></td><td>Remove a list of EdibleItems already present on this pet's EdibleItemList addressing them by their QualifiedItemID. 
<br></br>
Example:
  
```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "RemoveEdibleItems": [ "(O)139", "(O)720" ] //Salmon, shrimp
      }
    }
  ]
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#edibleitems">Guides/EdibleItems</a> for more info.
</td></tr>
<tr>
<td><code>AttackModel</code></td><td>Contains several other combat related methods that you can use to customize a litle bit more your combat experience.<br></br><em>*Such as: Edit Min Damage, edit Crit Chance or remove Enemies the pet is effective against for example.</em>
</tr>
</table>

### EditContent • Entries • AttackModel

The AttackModel property contains a series of methods that will help you edit some combat related fields more easily.
<br>This is how it usually looks like:<br/>

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "AttackModel": {
          "EditMinDamage": 5,
          "EditMaxDamage": 20,
          "EditCritChance": 0.40,
          "AddEnemiesEffectiveAgainst": [
            {
              "Name": "Skeleton",
              "DamageMultiplier": 3
            },
            {
              "Name": "Shadow Guy",
              "DamageMultiplier": 3
            }
          ]
        }
      }
    }
  ]
}
```
<br><br/> 
Here's the list of methods:
<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
<tr>
<td><code>EditMinDamage</code></td><td>Edit the minimum amount of damage this pet can inflict on enemies.<br></br>
<em>*The final min damage output is calculated using this formula: <br><code>(AttackModel.MinDamage * (AttackModel.DamageScaling * (Player.CombatLevel + 1))) * CombatSkillMasteryDamageModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>EditMaxDamage</code></td><td>Edit the maximum amount of damage this pet can inflict on enemies.<br></br>
<em>*The final max damage output is calculated using this formula: <br><code>(AttackModel.MaxDamage * (AttackModel.DamageScaling * (Player.CombatLevel + 1))) * CombatSkillMasteryDamageModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>EditCritChance</code></td><td>Edit the chances of this pet landing a critical hit on enemies.<br></br>
<em>*The final crit chance output is calculated using this formula: <br><code>(AttackModel.CritChance * CombatSkillMasteryCritChanceModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>EditCritMultiplier</code></td><td>Edit the multiplier applied to the pet's damage output when succesfully landing a critical hit (by default: 1.5).<br></br>
<em>*The final crit multiplier output is calculated using this formula: <br><code>(AttackModel.CritMultiplier * CombatSkillMasteryCritMultiplierModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>EditDamageScaling</code></td><td>Edit how much the pet's damage output scales with the player combat level (by default: 0.62).<br></br>
</td>
</tr>
<tr>
<td><code>EditKnockbackModifier</code></td><td>Edit how much knockback an enemy receives after being hit by this pet (dog default: 1.5, cat default: 1.25).<br></br>
</td>
</tr>
<tr>
<td><code>AddEnemiesEffectiveAgainst</code> & <code>ReplaceEnemiesEffectiveAgainst</code></td>
<td>Add or replace the entire list of Enemies this pet is effective against inside the pet's EnemiesEffectiveAgainstList.
<br></br>
Example:

```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "AttackModel": {
          "AddEnemiesEffectiveAgainst": [
            {
              "Name": "Skeleton",
              "DamageMultiplier": 3
            },
            {
              "Name": "Shadow Guy",
              "DamageMultiplier": 3
            }
          ]   //Its "replace" variant also follows the same structure!
        }
      }
    }
  ]
}
```
<em>*EnemiesEffectiveAgainst refers to enemies that receive more damage when being hit by this pet.</em>
<br><br/> 
An element inside an EnemiesEffectiveAgainst list typically looks like this:

```js
{
  "Name": "Skeleton", //The name of the monster.
  "DamageMultiplier": 3 //Effectiveness damage multiplier.
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#enemieseffectiveagainst">Guides/EnemiesEffectiveAgainst</a> for more info.
</td>
</tr>
<tr><td><code>RemoveEnemiesEffectiveAgainst</code></td><td>Remove a list of Enemies already present on this pet's EnemiesEffectiveAgainstList addressing them by their names. 
<br></br>
Example:
  
```js
{
  "EditContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "AttackModel": {
          "RemoveEnemiesEffectiveAgainst": [ "Grub", "Serpent" ]
        }
      }
    }
  ]
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#enemieseffectiveagainst">Guides/EnemiesEffectiveAgainst</a> for more info.
</td></tr>
<tr>
<td><code>EditAttackFrame</code></td><td>Edit what frame inside the pet's sprite sheet corresponds to the frame shown when the pet is attacking, if any.<br></br>
<em>*Choose a frame on the pet's sprite sheet to display as its attack frame. If you don't want the pet to have an attack frame: You can write null.</em>
</td>
</tr>
<tr>
<td><code>EditIsViciousType</code></td><td>Edit whether or not this pet is of a vicious kind. Type boolean.<br></br>
<em>*The 'IsViciousType' var is used by the mod to decide whether or not the pet can break the shell of Rock Crabs & interrupt grub metamorphosis.</em>
</td>
</tr>
</table>

And that's pretty much everything about EditContent. Next is AddContent.

<br></br>
<br></br>

### AddContent

The basic structure of an AddContent Action block usually looks like this:

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ], //The in-game path to the modded pet texture.
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {

        //More instructions here...
        //-------------------------

      }
      
    }
  ]
}
```
As you can see, it is made out of 5 fields: `PatchPriority`, `TargetTexture`, `FromTexture`, `PetType` and `Entries`.

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
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#targettexture">Guides/TargetTexture</a> for more info.
</td><td>Yes</td>
</tr>
<tr>
<td><code>FromTexture</code></td><td>The path to the new texture that'll replace the "TargetTexture".<br><br/><em>*This literally replaces the previous texture with a new one.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#fromtexture">Guides/FromTexture</a> for more info.
<td>Yes</td>
</tr>
<tr>
<td><code>PetType</code></td><td>Defines the type of pet, be it "Cat" or "Dog".<br></br><em>*This tells the mod what kind of sprite sheet this pet uses( a cat one or a dog one), what kind of attack effect type it uses, whether it should enter a Doghouse or a CatTree, what animations it uses, etc.</em>
<br></br>Valid Inputs: Cat, Dog.
<td>Yes</td>
</tr>
<tr>
<td><code>Entries</code></td><td>Contains several fields and properties that help configure this pet correctly.</em></td>
<td>Yes</td>
</tr>
</table>

### AddContent • Entries

The Entries property contains a series of fields and properties that help set up the modded pet correctly.
<br>This is how it usually looks like:<br/>

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "custom",
        "HatOffset": {
          "0,2": [ 0, -4, 2 ],
          "1,3": [ 0, -3, 2 ],
          "4,6": [ 5, -6, 1 ],
          "5,7": [ 5, -5, 1 ],
          "8,10": [ 0, -12, 0 ],
          "9,11": [ 0, -11, 0 ],
          "12,14": [ -6.5, -6, 3 ],
          "13,15": [ -6.5, -5, 3 ],
          "16": [ 0, -6, 2 ],
          "17": [ 0, -8, 2 ]
        },
        "Commands": [ "Follow", "Hunt", "Search", "Wait" ]

      }
      
    }
  ]
}
```
<br><br/> 
Here's the list of methods:
<table>
<tr>
<th>Field</th>
<th>Description</th>
<th>Required</th>
</tr>
<tr>
<td><code>HatOffsetModel</code></td><td>Chose the HatOffsetModel for this pet.
<br>Example:<br/>
  
```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "dog4"
      }
    }
    
  ]
}
```

<em>*A "HatOffsetModel" is a pre-made HatOffset dictionary you can use to avoid writing a new one yourself.<br>If you want to write the HatOffset dictionary yourself, use "custom".<br/></em><br><br/> 
Valid inputs: dog, dog1, dog2, dog3, dog4, dog5, cat, custom.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset-&-hatoffsetmodel">Guides/HatOffset & HatOffsetModel</a> for more info.</td><td>Yes</td>
</tr>
<tr>
<td><code>HatOffset</code></td><td>Configure the HatOffset dictionary for this pet.
<br>Example:<br/>

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "custom",
        "HatOffset": {
          "0,2": [ 0, -4, 2 ],
          "1,3": [ 0, -3, 2 ],
          "4,6": [ 5, -6, 1 ],
          "5,7": [ 5, -5, 1 ],
          "8,10": [ 0, -12, 0 ],
          "9,11": [ 0, -11, 0 ],
          "12,14": [ -6.5, -6, 3 ],
          "13,15": [ -6.5, -5, 3 ],
          "16": [ 0, -6, 2 ],
          "17": [ 0, -8, 2 ],
          "18,19,27,36,37,38": [ 0, -9, 2 ],
          "20": [ 5.5, -7, 1 ],
          "21": [ 4.5, -8.0, 1 ],
          "22": [ 3.5, -9.0, 1 ],
          "23,24,25,30,31,40,41": [ 3.5, -10, 1 ],
          "26": [ 3.5, -11, 1 ],
          "28,29": [ 5, 0, 2 ],
          "32": [ 5.5, -8, 1 ],
          "33": [ 5.5, -7, 1 ],
          "34": [ 5.5, -5, 1 ],
          "44,46": [ 0, -9, 2 ],
          "45,47": [ 0.0, -8.0, 2.0 ],
          "48,50": [ 5.5, -9, 1 ],
          "49,51": [ 5.5, -8, 1 ],
          "52": [ 0, -15, 0 ],
          "53": [ 0, -14, 0 ],
          "35,39,42,43,54,55": [ 0.0, 0.0, 2.0 ]
        }

      }
      
    }
  ]
}
```

<em>*A "HatOffset" dictionary contains instructions the mod needs in order for the pet's Hat to be displayed correctly on its texture (on a specific frame).<br><br/> The mod uses coordinates relative to the center of the pet's sprite to position the hat on top of the pet.<br><br/>As such, it needs to know the direction of the hat and the position it should be at a certain frame. <br>For example: At frame 24, the hat should be 3.5px right, 10px higher, and it should be facing East.<br/></em>
<br><br/>A key in a HatOffset dictionary looks like this:
```js
//Directions: 0(North), 1(East), 2(South), 3(West)
"frame": [ xOffset, yOffset, direction]
```
<em>**Note that the "y" axis is inverted, which means that "-10" is read as: "Ten pixels up" instead.</em>
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset-&-hatoffsetmodel">Guides/HatOffset & HatOffsetModel</a> for more info.

</td><td>No</td>
</tr>
<tr>
<td><code>Commands</code></td>
<td>Configure the CommandList for this pet.
<br></br>
Example:

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "dog4",
        "Commands": [ "Follow", "Hunt", "Search", "Wait" ]
      }
      
    }
  ]
}
```
<em>*Note that 'commands' it's refering to the tricks this pet can use and learn.</em>
<br><br/> 
Valid inputs: Wait, Hunt, Follow, Search.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#commands">Guides/Commands</a> for more info.
</td><td>No</td>
</tr>
<tr>
<td><code>EdibleItems</code></td>
<td>Configure the EdibleItemList for this pet.
<br></br>
Example:

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "dog4",
        "EdibleItems": [
          {
            "QualifiedItemID": "(O)139", //Salmon
            "FriendshipPointsGained": 20
          },
          {
            "QualifiedItemID": "(O)130", //Tuna
            "FriendshipPointsGained": 20
          }
        ]

      }
      
    }
  ]
}
```
<em>*Edible items refer to what items can be gifted to pets regardless of whether the pet likes them or not.</em>
<br><br/> 
An element inside an EdibleItem list typically looks like this:

```js
{
  "QualifiedItemID": "(O)139", //You can find more info about it in the stardew wiki.
  "FriendshipPointsGained": 20 //The amount of FP gained by petting a pet is 12. 
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#edibleitems">Guides/EdibleItems</a> for more info.
</td><td>No</td>
</tr>
<tr>
<td><code>TrickLearningTreat</code></td><td>Define what item should be used in exchange for progressing in the command learning lessons.<br></br>

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "dog4",
        "TrickLearningTreat": "(O)131" // Sardines QualifiedItemID

      }
      
    }
  ]
}
```
<em>*Pets require a certain amount of 'treats' in order to progress and learn commands, input the QualifiedItemID of your desired treat to make it a valid exchange treat.</em></td><td>No</td>
</tr>
<tr>
<td><code>AttackModel</code></td><td>Contains several other combat related fields you can use to customize a litle bit more your combat experience.<br></br><em>*Such as: Min Damage, Crit Chance or EnemiesEffectiveAgainst for example.</em><td>No</td>
</tr>
</table>

### AddContent • Entries • AttackModel

The AttackModel property contains a series of fields you can use to customize more your pet combat experience.
<br>This is how it usually looks like:<br/>

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "dog4",
        "AttackModel": {
          "MinDamage": 5,
          "MaxDamage": 20,
          "CritChance": 0.40,
          "EnemiesEffectiveAgainst": [
            {
              "Name": "Skeleton",
              "DamageMultiplier": 3
            },
            {
              "Name": "Shadow Guy",
              "DamageMultiplier": 3
            }
          ]
        }

      }
      
    }
  ]
}
```
<br><br/> 
Here's the list of methods:
<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
<tr>
<td><code>MinDamage</code></td><td>Change the minimum amount of damage this pet can inflict on enemies.<br></br>
<em>*The final min damage output is calculated using this formula: <br><code>(AttackModel.MinDamage * (AttackModel.DamageScaling * (Player.CombatLevel + 1))) * CombatSkillMasteryDamageModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>MaxDamage</code></td><td>Change the maximum amount of damage this pet can inflict on enemies.<br></br>
<em>*The final max damage output is calculated using this formula: <br><code>(AttackModel.MaxDamage * (AttackModel.DamageScaling * (Player.CombatLevel + 1))) * CombatSkillMasteryDamageModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>CritChance</code></td><td>Change the chances of this pet landing a critical hit on enemies.<br></br>
<em>*The final crit chance output is calculated using this formula: <br><code>(AttackModel.CritChance * CombatSkillMasteryCritChanceModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>CritMultiplier</code></td><td>Change the multiplier applied to the pet's damage output when succesfully landing a critical hit (by default: 1.5).<br></br>
<em>*The final crit multiplier output is calculated using this formula: <br><code>(AttackModel.CritMultiplier * CombatSkillMasteryCritMultiplierModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>DamageScaling</code></td><td>Change how much the pet's damage output scales with the player combat level (by default: 0.62).<br></br>
</td>
</tr>
<tr>
<td><code>KnockbackModifier</code></td><td>Change how much knockback an enemy receives after being hit by this pet (dog default: 1.5, cat default: 1.25).<br></br>
</td>
</tr>
<tr>
<td><code>EnemiesEffectiveAgainst</code></td>
<td>Configure the list of Enemies this pet is effective against.
<br></br>
Example:

```js
{
  "AddContent": [
    {
      "PatchPriority": 1,
      "TargetTexture": [ "Mods", "moddedDogTexture" ],
      "FromTexture": [ "assets", "exampleTexture.png" ],
      "PetType": "Dog",
      "Entries": {
        "HatOffsetModel": "dog4",
        "AttackModel": {
          "EnemiesEffectiveAgainst": [
            {
              "Name": "Skeleton",
              "DamageMultiplier": 3
            },
            {
              "Name": "Shadow Guy",
              "DamageMultiplier": 3
            }
          ]
        }

      }
      
    }
  ]
}
```
<em>*EnemiesEffectiveAgainst refers to enemies that receive more damage when being hit by this pet.</em>
<br><br/> 
An element inside an EnemiesEffectiveAgainst list typically looks like this:

```js
{
  "Name": "Skeleton", //The name of the monster.
  "DamageMultiplier": 3 //Effectiveness damage multiplier.
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#enemieseffectiveagainst">Guides/EnemiesEffectiveAgainst</a> for more info.
</td>
</tr>
<tr>
<td><code>AttackFrame</code></td><td>Change what frame inside the pet's sprite sheet corresponds to the frame shown when the pet is attacking, if any.<br></br>
<em>*Choose a frame on the pet's sprite sheet to display as its attack frame. If you don't want the pet to have an attack frame: You can write null.</em>
</td>
</tr>
<tr>
<td><code>IsViciousType</code></td><td>Change whether or not this pet is of a vicious kind. Type boolean.<br></br>
<em>*The 'IsViciousType' var is used by the mod to decide whether or not the pet can break the shell of Rock Crabs & interrupt grub metamorphosis.</em>
</td>
</tr>
</table>

And that's pretty much everything about Content Packs for Pet's Enhanced Mod, thanks for reading!

## See also
* Pet's Enhanced Mod Content packs in [Nexus Mods](https://www.nexusmods.com/stardewvalley/mods/categories/8/)
* My [BuyMeACoffee](https://buymeacoffee.com/sunkenlace) page, if you wanna support me.
