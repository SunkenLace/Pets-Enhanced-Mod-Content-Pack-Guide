# Property Info
A library of information about various fields and methods used in creating Content Packs for the Pet's Enhanced Mod.

## Contents
* [TargetTexture](#targettexture)
* [FromTexture](#fromtexture)
* * [Sprite Requirements](#sprite-requirements)
* [HatOffset & HatOffsetModel](#hatoffset--hatoffsetmodel)
* [Commands](#commands)
* [EdibleItems](#edibleitems)
* [EnemiesEffectiveAgainst](#enemieseffectiveagainst)

## TargetTexture

TargetTexture is a property used by the mod to identify the targets of the patches; as such, the mod applies patches to any Pet-type character whose texture is stored in its Content Library, using the specific ContentModel for that texture, which you can add yourself and edit with Content Packs.
<br></br>
By selecting a target texture, you essentially add it to the internal Content Library. Consequently, any pet sharing that texture becomes a valid patch target.
<br></br>

### Structure of a TargetTexture Property
The TargetTexture property is structured as a string array for easy readability across different devices. <br></br>The path always originates in the "StardewValley\Content" folder and can only traverse subfolders.
<br></br>Example:


```js

"TargetTexture": [ "Mods", "moddedDogTexture" ] //This translates to: StardewValley\Content\Mods\moddedDogTexture

```

### How to Select a Target Texture
A target texture is essentially an in-game file path to a specific texture. This makes it simple to locate the desired texture. <br>Here are some common methods:</br>
* <b>Vanilla Pets:</b> You can find the textures of vanilla animals in <code>Animals</code>. <br></br>For example:
  
  ```js
  "TargetTexture": [ "Animals", "dog3" ] //This is the texture for the 3º dog variant in vanilla.
  ```
  Valid vanilla textures include: dog, dog1, dog2, dog3, dog4, dog5, cat, cat1, cat2, cat3, cat4, cat5.
  <br></br>

* <b>Modded Pets:</b> The internal texture path for modded pets can be found within their ContentPatcher content pack, specifically in the <code>content.json</code> file.
  <br></br>Here you search for the name of the pet you're looking for. In this case it is "ModdedDog1".

  ```js
  {
    "Format": "2.0.0",
    "Changes": [
      {
        "LogName": "Add ModdedDog1 breed",
        "Action": "EditData",
        "Target": "Data/Pets",
        "TargetField": [ "Dog", "Breeds" ],
        "Entries": {
          "AuthorName.ModName.ModdedDog1": {       // Here it is!
            "Id": "AuthorName.ModName.ModdedDog1",
            "Texture": "Mods\\AuthorName.ModName\\ModdedDog1",       //And this is the path to its texture.
            "IconTexture": "Mods\\AuthorName.ModName\\ModdedDog1icon",
            "IconSourceRect": {
              "X": 0,
              "Y": 0,
              "Width": 16,
              "Height": 16
            },
            "CanBeChosenAtStart": false,
            "CanBeAdoptedFromMarnie": true,
            "BarkOverride": null,
            "VoicePitch": 1.0
          }
        }
      }
    ]
  }
  ```

  Once you've determined the path, implement it in the TargetTexture property like this:
  ```js
  "TargetTexture": [ "Mods", "AuthorName.ModName", "ModdedDog1" ]
  ```

## FromTexture
FromTexture is a property used by the mod to pinpoint the exact location of the Texture asset that will replace the old pet's texture.<br></br> It's main purpose apart from customization is to manually correct invalid textures into valid ones. <br></br><em>The mod uses different sprite sheets for cats and dogs, containing frames absent in vanilla, which can cause visual bugs due to missing frames.</em><br></br>To prevent this, the mod replaces the old vanilla sprite model with a new one that works well. As such, by providing a new edited texture you are correcting the old one by providing the missing frames, thus fixing visual bugs.

## HatOffset & HatOffsetModel

## Commands
  Commands refer to tricks a pet can learn and use. In order for the pet to learn those tricks, it needs to have the name of the trick on its CommandList.<br></br> As such, by adding the name of the trick on the Commands array you are allowing the pet to learn that trick.
<br></br>
The list of tricks are:
* <b>Wait:</b>
  Makes pet sit on the spot and wait until told otherwise.
* <b>Follow:</b>
  Makes pet follow the player at all places.
* <b>Hunt:</b>
  Pet will attack any monster near the player.
* <b>Search:</b>
  Pet will search any nearby bushes for loot.

## EdibleItems
Edible items refer to any item that can be gifted to a pet. <br></br>Edible Items have the properties of giving a friendship bonus via the "FriendshipPointsGained" field. You can edit its values to either increase or decrease your pet's friendship with the player after being gifted that particular item.
<br></br>Structure of an EdibleItem:

```js
{
"QualifiedItemID": "(O)139", //An Unique Identifier for this item formed by the (Item Type) + its Internal ID. More info on the Stardew Wiki.
"FriendshipPointsGained": 20 //This can be either negative or positive. The usual amount of FP gained by petting a pet is 12. 
}
```

## EnemiesEffectiveAgainst

