# Pets Enhanced Mod Content Pack Guide
This comprehensive guide explains how to install and/or create content packs for the Stardew Valley mod "Pet's Enhanced - A Pet Mechanic Overhaul".
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
3. To install a content pack, unzip it into the `Mods` folder.
4. Run the game using SMAPI.

That's it! Content packs unzipped into `Mods` will be loaded and applied automatically.
<br><br/>

## Getting Started
"The following guide is for mod authors only, if you're not a mod author and don't plan to make your own content pack, the installation instructions above are sufficient."

Before diving into content pack creation, let's explore what a finished content pack looks like:
```
📁 Mods/
   📁 [PEM] YourModName/
      🗎 content.json
      🗎 manifest.json
      📁 assets/
         🗎 exampleTexture.png
```
As you can see, the structure resembles a Content Patcher content pack. Here's an example of a simple `content.json` file for this mod:

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
This piece of code replaces the texture of the vanilla cat #3 with `exampleTexture.png`. You can do this and much more with the help of this guide (via content packs).
<br><br/>
Content packs allow you to achieve various modifications, like editing pet damage, hat placement, learnable commands, and more.
<br><br/>

## Creating the Content Pack

  1. Inside the Mods folder, create a new folder for your content pack.
  2. Within this folder, create a new JSON file named `manifest.json`.
     
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
With this, SMAPI will now recognize your content pack.

Next, we'll need to create the content.json file, which defines the logic for your content pack.

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

When creating a content pack, you'll likely want to either edit existing content from the mod or add new content to it. The mod provides two constructors, `EditContent` and `AddContent`, to help you organize your logic into separate blocks.

#### Structure:
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
Each block represents an independent action that instructs the mod how to apply changes.
<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
<tr>
<td><code>AddContent</code></td><td>Include modded pets into the Internal Content Library of the mod.
<br><br/>
<em>*By default, the mod ignores modded pets with textures not present in the Library.
<br><br/>
This is because the mod requires information like learnable commands and edible items for proper functionality.
<br></br>
Additionally, the mod uses different sprite sheets for cats and dogs, containing frames absent in vanilla, which can cause visual bugs due to missing frames.</em>
</td>
</tr>
<tr>
<td><code>EditContent</code></td><td>Edits content already present in the mod's internal Content Library.</td>
</tr>
</table>
<br><br/>

## EditContent

The basic structure of an `EditContent` action block typically looks like this:

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
This block consists of four fields: `PatchPriority`, `TargetTexture`, `FromTexture`, and `Entries`.

<table>
<tr>
<th>Field</th>
<th>Description</th>
<th>IsRequired</th>
</tr>
<tr>
<td><code>PatchPriority</code></td><td>Set this action's priority over its <code>TargetTexture</code>. <br></br><em>*If another content pack targets the same texture, the one with the highest <code>PatchPriority</code> is applied first.</em></td><td>Yes</td>
</tr>
<tr>
<td><code>TargetTexture</code></td><td>The texture to be affected by the changes. 
<br><br/> 
<em>*The mod uses textures to identify which pet to patch. As such, all pets that share the same texture will be patched similarly.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#targettexture">PropertyInfo:TargetTexture</a> for more info.
</td><td>Yes</td>
</tr>
<tr>
<td><code>FromTexture</code></td><td>The path to the new texture that replaces the <code>TargetTexture</code>(visually).<br><br/><em>*This literally replaces the previous texture with a new one.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#fromtexture">PropertyInfo:FromTexture</a> for more info.
<td>No</td>
</tr>
<tr>
<td><code>Entries</code></td><td>Contains additional methods for achieving more specific results, such as adding edible items, replacing pet commands, or modifying the attack model.</td>
<td>No</td>
</tr>
</table>

### EditContent • Entries

The <code>Entries</code> property offers a collection of methods and sub-methods to edit the content more easily.
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
<td><code>EditHatOffsetModel</code></td><td>Change this pet's previous <code>HatOffSetModel</code> with the specified one.
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

<em>*"HatOffsetModel" refers to pre-defined HatOffset dictionaries that save you from writing them yourself.<br><br/>Use `custom` to write the HatOffset dictionary yourself.</em><br><br/> 
Valid inputs: `dog`, `dog1`, `dog2`, `dog3`, `dog4`, `dog5`, `cat`, `custom`.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset--hatoffsetmodel">PropertyInfo:HatOffset & HatOffsetModel</a> for more info.
</tr>
<tr>
<td><code>AddHatOffset</code>, <code>EditHatOffset</code> & <code>ReplaceHatOffset</code></td><td>Add, edit or replace the entire HatOffset dictionary for this pet.
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

<em>*A "HatOffset" dictionary provides instructions for the mod to display the pet's hat correctly on its texture (frame specific).<br><br/> The mod uses relative coordinates to the pet's sprite center to position the hat.<br><br/>Because of that, it needs to know the hat's direction and position at certain frames.  <br>For example: At frame 24, the hat should be 3.5px to the right, 10px higher, and facing east.<br/></em>
<br><br/>A key in a HatOffset dictionary looks like this:
```js
//Directions: 0(North), 1(East), 2(South), 3(West).
"frame": [ xOffset, yOffset, direction]
```
<em><b>Note:</b> The "y" axis is inverted. "-10" is read as "Ten pixels up" in-game.</em>
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset--hatoffsetmodel">PropertyInfo:HatOffset & HatOffsetModel</a> for more info.

</td>
</tr>
<tr><td><code>RemoveHatOffsetFrames</code></td><td>Removes a list of existing frames from this pet's <code>HatOffset</code> dictionary.
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
<td>Add, remove, or replace the entire list of commands within the pet's CommandList.
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
<em>*"Commands" refer to the tricks a pet can learn and use.</em>
<br><br/> 
Valid inputs: `Wait`, `Hunt`, `Follow`, `Search`.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#commands">PropertyInfo:Commands</a> for more info.
</td>
</tr>
<tr>
<td><code>AddEdibleItems</code> & <code>ReplaceEdibleItems</code></td>
<td>Add or replace the entire list of edible items within the pet's <code>EdibleItemList</code>. 
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
<em>*"Edible items" refer to any item that can be gifted to a pet, regardless of whether the pet likes it.</em>
<br><br/> 
An element inside an `EdibleItem` list typically looks like this:

```js
{
  "QualifiedItemID": "(O)139", //Look it up at the stardew valley wiki to see more info.
  "FriendshipPointsGained": 20 //The usual amount of FP gained by petting a pet is 12. 
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#edibleitems">PropertyInfo:EdibleItems</a> for more info.
</td>
</tr>
<tr><td><code>RemoveEdibleItems</code></td><td>Remove a list of existing edible items from the pet's <code>EdibleItemList</code> by specifying their QualifiedItemID.
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
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#edibleitems">PropertyInfo:EdibleItems</a> for more info.
</td></tr>
<tr>
<td><code>AttackModel</code></td><td>This sub-entry offers various methods for customizing pet combat behavior.<br></br><em>*Examples include editing minimum damage, critical hit chance, or removing enemies the pet is effective against.</em>
</tr>
</table>

### EditContent • Entries • AttackModel

The `AttackModel` property provides methods for modifying various combat-related aspects of your pet.
<br>Here's an example:<br/>

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
List of methods:
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
<td><code>EditCritChance</code></td><td>Edit the chance of this pet landing a critical hit on enemies. <br></br>
<em>*The final crit chance output is calculated using this formula: <br><code>(AttackModel.CritChance * CombatSkillMasteryCritChanceModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>EditCritMultiplier</code></td><td>Edit the multiplier applied to the pet's damage when it lands a critical hit (default: 1.5). <br></br>
<em>*The final crit multiplier output is calculated using this formula: <br><code>(AttackModel.CritMultiplier * CombatSkillMasteryCritMultiplierModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>EditDamageScaling</code></td><td>Edit how much the pet's damage scales with the player's combat level (default: 0.62).<br></br>
</td>
</tr>
<tr>
<td><code>EditKnockbackModifier</code></td><td>Edit the amount of knockback applied to enemies after being hit by this pet (default: 1.5 for dogs, 1.25 for cats).<br></br>
</td>
</tr>
<tr>
<td><code>AddEnemiesEffectiveAgainst</code> & <code>ReplaceEnemiesEffectiveAgainst</code></td>
<td>Add or replace the entire list of enemies this pet is effective against within its <code>EnemiesEffectiveAgainstList</code>. 
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
<em>*"EnemiesEffectiveAgainst" refers to enemies that receive more damage against this pet.</em>
<br><br/> 
An element inside an <code>EnemiesEffectiveAgainst</code> list typically looks like this:

```js
{
  "Name": "Skeleton", //The name of the monster.
  "DamageMultiplier": 3 //Effectiveness damage multiplier.
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#enemieseffectiveagainst">PropertyInfo:EnemiesEffectiveAgainst</a> for more info.
</td>
</tr>
<tr><td><code>RemoveEnemiesEffectiveAgainst</code></td><td>Remove a list of enemies already present in this pet's <code>EnemiesEffectiveAgainstList</code> by specifying their names.
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
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#enemieseffectiveagainst">PropertyInfo:EnemiesEffectiveAgainst</a> for more info.
</td></tr>
<tr>
<td><code>EditAttackFrame</code></td><td>Specify the frame from the pet's sprite sheet to be displayed during an attack.<br></br>
<em>*If you want the pet to have no attack animation, set this to null.</em>
</td>
</tr>
<tr>
<td><code>EditIsViciousType</code></td><td>Determine whether the pet is considered "vicious." Type boolean.<br></br>
<em>*This property influences the pet's ability to break Rock Crab shells and interrupt Grub metamorphosis.</em>
</td>
</tr>
</tr>
<tr>
<td><code>EditHoveringAttackEffect</code></td><td>Defines a custom animation to be displayed over the target when attacked by this pet.<br></br>The animation consists of a series of frames with specific properties.
<br>Structure:</br>
  
```js
{
  "EditContent": [
    {
      "PatchPriority": 2,
      "TargetTexture": [ "Animals", "cat4" ],
      "Entries": {
        "AttackModel": {
          "EditHoveringAttackEffect": [
            {
              "Frame": 63,
              "Duration": 2,
              "Offset": [ -5, 4 ],
              "PlaySound": {
                "Name": "serpentHit",
                "Delay": 2,
                "Pitch": null
              },
              "Flip": false
            },
            {  
              "Frame": 45,
              "Duration": 5
            },
            {
              "Frame": 56,
              "Duration": 10,
              "Flip": true
            }
          ]
        }
      }
    }
  ]
}
```

Each block within the array represents a frame in the animation. <br></br> List of properties for each frame:
<table>
<th>Field</th><th>Description</th><th>Required</th>
<tr>
<td><code>Frame</code></td><td>The specific frame from the pet's sprite sheet to be displayed.<br></br>Example:
  
```js
  "Frame": 23 // The frame nº23 of the pet sprite sheet.
```
</td><td>Yes</td>
</tr>
<tr>
<td><code>Duration</code></td><td>The duration of this frame (in ticks). <br></br>Example:
  
```js
  "Duration": 2 //The frame lasts 2 ticks.
```
</td><td>Yes</td>
</tr>
<tr>
<td><code>Flip</code></td><td>Whether to horizontally flip the frame.<br></br>Example:
  
```js
  "Flip": true // Flips the frame horizontally.
```
</td><td>No</td>
</tr>
<tr>
<td><code>Scale</code></td><td>The scale modifier for this frame. <br></br>Example:
  
```js
  "Scale": 2.0 // Means 2x bigger. 
```
</td><td>No</td>
</tr>
<tr>
<td><code>Rotation</code></td><td>The rotation value for this frame. <br></br>Example:
  
```js
  "Rotation": 45 // Rotate the frame 45º degrees clockwise.
```
</td><td>No</td>
</tr>
<tr>
<td><code>Alpha</code></td><td>The opacity of the frame (0.0 to 1.0). <br></br>Example:
  
```js
  "Alpha": 0.5 // Means 50% transparent.
```
</td><td>No</td>
<tr>
<td><code>Offset</code></td><td>The position offset (x, y) for the frame.<br></br>Example:
  
```js
  "Offset": [ xOffset, yOffset ]
```
</td><td>No</td>
</tr>
<tr>
<td><code>PlaySound</code></td><td>The sound that should play this frame with specific timing and pitch. <br></br>Example:
  
```js
"PlaySound": {
    "Name": "serpentHit", // The in-game name of the cue.
    "Delay": 2, //The delay in ticks for this cue.
    "Pitch": null // The pitch value of this cue.
  }
```
</td><td>No</td>

</tr>
</table>
</td>
</tr>
</table>
<br></br>
This concludes the explanation of the <code>EditContent</code> section. Next, we'll delve into the <code>AddContent</code> functionality.

<br></br>

## AddContent

The basic structure of an `AddContent` action block typically looks like this:

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
This block consists of five fields: `PatchPriority`, `TargetTexture`, `FromTexture`, `PetType` and `Entries`.

<table>
<tr>
<th>Field</th>
<th>Description</th>
<th>IsRequired</th>
</tr>
<tr>
<td><code>PatchPriority</code></td><td>Set this action's priority over its <code>TargetTexture</code>. <br></br><em>*If another content pack targets the same texture, the one with the highest <code>PatchPriority</code> is applied first.</em></td><td>Yes</td>
</tr>
<tr>
<td><code>TargetTexture</code></td><td>The texture to be affected by the changes. 
<br><br/> 
<em>*The mod uses textures to identify which pet to patch. As such, all pets that share the same texture will be patched similarly.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#targettexture">PropertyInfo:TargetTexture</a> for more info.
</td><td>Yes</td>
</tr>
<tr>
<td><code>FromTexture</code></td><td>The path to the new texture that replaces the <code>TargetTexture</code>(visually).<br><br/><em>*This literally replaces the previous texture with a new one.</em>
<br><br/>See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#fromtexture">PropertyInfo:FromTexture</a> for more info.
<td>Yes</td>
</tr>
<tr>
<td><code>PetType</code></td><td>Specifies the type of pet, either <code>"Cat"</code> or <code>"Dog"</code>.<br></br><em>*This determines the sprite sheet model, attack effect type, housing (Doghouse or CatTree), animations, and other pet-specific behaviors.</em></td>
<td>Yes</td>
</tr>
<tr>
<td><code>Entries</code></td><td>Contains various fields and properties to configure the pet's specific attributes and behaviors.</em></td>
<td>Yes</td>
</tr>
</table>

### AddContent • Entries

The `Entries` property lets you configure various aspects of your modded pet's behavior and appearance.
<br>Here's an example:<br/>

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
List of methods:
<table>
<tr>
<th>Field</th>
<th>Description</th>
<th>Required</th>
</tr>
<tr>
<td><code>HatOffsetModel</code></td><td>Choose the <code>HatOffsetModel</code> for this pet.
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

<em>*"HatOffsetModel" refers to pre-defined HatOffset dictionaries that save you from writing them yourself.<br><br/>Use `custom` to write the HatOffset dictionary yourself.</em><br><br/> 
Valid inputs: `dog`, `dog1`, `dog2`, `dog3`, `dog4`, `dog5`, `cat`, `custom`.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset--hatoffsetmodel">PropertyInfo:HatOffset & HatOffsetModel</a> for more info.</td><td>Yes</td>
</tr>
<tr>
<td><code>HatOffset</code></td><td>Define the <code>HatOffset</code> dictionary for this pet.
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

<em>*A "HatOffset" dictionary provides instructions for the mod to display the pet's hat correctly on its texture (frame specific).<br><br/> The mod uses relative coordinates to the pet's sprite center to position the hat.<br><br/>Because of that, it needs to know the hat's direction and position at certain frames.  <br>For example: At frame 24, the hat should be 3.5px to the right, 10px higher, and facing east.<br/></em>
<br><br/>A key in a HatOffset dictionary looks like this:
```js
//Directions: 0(North), 1(East), 2(South), 3(West).
"frame": [ xOffset, yOffset, direction]
```
<em><b>Note:</b> The "y" axis is inverted. "-10" is read as "Ten pixels up" in-game.</em>
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#hatoffset--hatoffsetmodel">PropertyInfo:HatOffset & HatOffsetModel</a> for more info.

</td><td>No</td>
</tr>
<tr>
<td><code>Commands</code></td>
<td>Define the <code>Command</code> list for this pet.
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
<em>*"Commands" refer to the tricks a pet can learn and use.</em>
<br><br/> 
Valid inputs: `Wait`, `Hunt`, `Follow`, `Search`.
<br><br/> 
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#commands">PropertyInfo:Commands</a> for more info.
</td><td>No</td>
</tr>
<tr>
<td><code>EdibleItems</code></td>
<td>Define the <code>EdibleItem</code> list for this pet.
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
<em>*"Edible items" refer to any item that can be gifted to a pet, regardless of whether the pet likes it.</em>
<br><br/> 
An element inside an `EdibleItem` list typically looks like this:

```js
{
  "QualifiedItemID": "(O)139", //Look it up at the stardew valley wiki to see more info.
  "FriendshipPointsGained": 20 //The usual amount of FP gained by petting a pet is 12. 
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#edibleitems">PropertyInfo:EdibleItems</a> for more info.
</td><td>No</td>
</tr>
<tr>
<td><code>TrickLearningTreat</code></td><td>Specify the item required to progress in teaching your pet new commands.<br></br>

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
<em>*Use the item's <code>QualifiedItemID</code> to define the desired item.</em></td><td>No</td>
</tr>
<tr>
<td><code>AttackModel</code></td><td>This sub-entry offers various methods for customizing pet combat behavior.<br></br><em>*Examples include editing minimum damage, critical hit chance, or adding enemies the pet is effective against.</em></td><td>No</td>
</tr>
</table>

### AddContent • Entries • AttackModel

The `AttackModel` property provides methods for modifying various combat-related aspects of your pet.
<br>Here's an example:<br/>

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
List of methods:
<table>
<tr>
<th>Field</th>
<th>Description</th>
</tr>
<tr>
<td><code>MinDamage</code></td><td>Determine the minimum amount of damage this pet can inflict on enemies.<br></br>
<em>*The final min damage output is calculated using this formula: <br><code>(AttackModel.MinDamage * (AttackModel.DamageScaling * (Player.CombatLevel + 1))) * CombatSkillMasteryDamageModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>MaxDamage</code></td><td>Determine the maximum amount of damage this pet can inflict on enemies.<br></br>
<em>*The final max damage output is calculated using this formula: <br><code>(AttackModel.MaxDamage * (AttackModel.DamageScaling * (Player.CombatLevel + 1))) * CombatSkillMasteryDamageModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>CritChance</code></td><td>Determine the chance of this pet landing a critical hit on enemies. <br></br>
<em>*The final crit chance output is calculated using this formula: <br><code>(AttackModel.CritChance * CombatSkillMasteryCritChanceModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>CritMultiplier</code></td><td>Determine the multiplier applied to the pet's damage when it lands a critical hit (default: 1.5). <br></br>
<em>*The final crit multiplier output is calculated using this formula: <br><code>(AttackModel.CritMultiplier * CombatSkillMasteryCritMultiplierModifiers</code></br></em>
</td>
</tr>
<tr>
<td><code>DamageScaling</code></td><td>Determine how much the pet's damage scales with the player's combat level (default: 0.62).<br></br>
</td>
</tr>
<tr>
<td><code>KnockbackModifier</code></td><td>Determine the amount of knockback applied to enemies after being hit by this pet (default: 1.5 for dogs, 1.25 for cats).<br></br>
</td>
</tr>
<tr>
<td><code>EnemiesEffectiveAgainst</code></td>
<td>Define the list of <code>EnemiesEffectiveAgainst</code> for this pet. 
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
<em>*"EnemiesEffectiveAgainst" refers to enemies that receive more damage against this pet.</em>
<br><br/> 
An element inside an <code>EnemiesEffectiveAgainst</code> list typically looks like this:

```js
{
  "Name": "Skeleton", //The name of the monster.
  "DamageMultiplier": 3 //Effectiveness damage multiplier.
}
```
See <a href="https://github.com/SunkenLace/Pets-Enhanced-Mod-Content-Pack-Guide/blob/main/PropertyInfo.md#enemieseffectiveagainst">PropertyInfo:EnemiesEffectiveAgainst</a> for more info.
</td>
</tr>
<tr>
<td><code>AttackFrame</code></td><td>Specify the frame from the pet's sprite sheet to be displayed during an attack.<br></br>
<em>*If you want the pet to have no attack animation, set this to null.</em>
</td>
</tr>
<tr>
<td><code>IsViciousType</code></td><td>Determine whether the pet is considered "vicious." Type boolean.<br></br>
<em>*This property influences the pet's ability to break Rock Crab shells and interrupt Grub metamorphosis.</em>
</td>
</tr>
<tr>
<td><code>HoveringAttackEffect</code></td><td>Defines a custom animation to be displayed over the target when attacked by this pet.<br></br>The animation consists of a series of frames with specific properties.
<br>Structure:</br>
  
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
        "HoveringAttackEffect": [
          {
            "Frame": 63,
            "Duration": 2,
            "Offset": [ -5, 4 ],
            "PlaySound": {
              "Name": "serpentHit",
              "Delay": 2,
              "Pitch": null
            },
            "Rotation": 45,
            "Alpha": 0,
            "Scale": 1,
            "Flip": false
          }
        ]
      }

    }

  }
]
}
```

Each block within the array represents a frame in the animation. <br></br> List of properties for each frame:
<table>
<th>Field</th><th>Description</th><th>Required</th>
<tr>
<td><code>Frame</code></td><td>The specific frame from the pet's sprite sheet to be displayed.<br></br>Example:
  
```js
  "Frame": 23 // The frame nº23 of the pet sprite sheet.
```
</td><td>Yes</td>
</tr>
<tr>
<td><code>Duration</code></td><td>The duration of this frame (in ticks). <br></br>Example:
  
```js
  "Duration": 2 //The frame lasts 2 ticks.
```
</td><td>Yes</td>
</tr>
<tr>
<td><code>Flip</code></td><td>Whether to horizontally flip the frame.<br></br>Example:
  
```js
  "Flip": true // Flips the frame horizontally.
```
</td><td>No</td>
</tr>
<tr>
<td><code>Scale</code></td><td>The scale modifier for this frame. <br></br>Example:
  
```js
  "Scale": 2.0 // Means 2x bigger. 
```
</td><td>No</td>
</tr>
<tr>
<td><code>Rotation</code></td><td>The rotation value for this frame. <br></br>Example:
  
```js
  "Rotation": 45 // Rotate the frame 45º degrees clockwise.
```
</td><td>No</td>
</tr>
<tr>
<td><code>Alpha</code></td><td>The opacity of the frame (0.0 to 1.0). <br></br>Example:
  
```js
  "Alpha": 0.5 // Means 50% transparent.
```
</td><td>No</td>
<tr>
<td><code>Offset</code></td><td>The position offset (x, y) for the frame.<br></br>Example:
  
```js
  "Offset": [ xOffset, yOffset ]
```
</td><td>No</td>
</tr>
<tr>
<td><code>PlaySound</code></td><td>The sound that should play this frame with specific timing and pitch. <br></br>Example:
  
```js
"PlaySound": {
    "Name": "serpentHit", // The in-game name of the cue.
    "Delay": 2, //The delay in ticks for this cue.
    "Pitch": null // The pitch value of this cue.
  }
```
</td><td>No</td>

</tr>
</table>
</td>
</tr>
</table>
<br></br>
And that's pretty much everything about Content Packs for Pet's Enhanced Mod, thanks for reading!

## See also
* Pet's Enhanced Mod Content packs in [Nexus Mods](https://www.nexusmods.com/stardewvalley/mods/categories/8/)
* My [BuyMeACoffee](https://buymeacoffee.com/sunkenlace) page, if you wanna support me.
