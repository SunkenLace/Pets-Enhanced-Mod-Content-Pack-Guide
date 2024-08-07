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
The FromTexture property specifies the path to the texture asset that will replace the original pet texture. This feature is primarily used for customization but also serves to correct invalid textures.<br></br>The mod uses different sprite sheets for cats and dogs, containing frames absent in vanilla, which can cause visual bugs due to missing frames. By providing a new, edited texture with the necessary frames, you can effectively replace the original texture and resolve these visual issues.

### Structure of a FromTexture Property
The FromTexture property is structured as a string array for easy readability across different devices. <br></br>The path always originates at you content pack folder and can only traverse subfolders.
<br></br>Example:


```js

"FromTexture": [ "assets", "exampleTexture.png" ] //This translates to: StardewValley\Mods\MyContentPack\assets\exampleTexture.png

```

### Sprite Requirements
In order to replace a texture we first need to make sure our edited texture fulfills all the requirements for a succesfull patch.<br></br>
Requirements:

* The sprite sheet must ALWAYS be 4 frames wide.

* Make sure to respect the positioning of the frames.

* The new texture must contain all required frames for its type.
  <br><em>After that's done, you can add as many frames as you want (vertically).</em></br>
  
  <b>For dog types:</b>
  <br></br>![dog1](https://github.com/user-attachments/assets/60ff0d9d-3136-4a2e-83b6-725e8aa69620)
  <br></br>The bite animation can be customized by editing or adding more frames, and later be configured using the HoveringAttackEffect field.

  <b>For cat types:</b>
  <br></br>![cat2](https://github.com/user-attachments/assets/144d09b5-756d-4d17-9f8b-60e98cbe35a6)
  <br></br>You can add a hovering attack animation by adding new frames to the sprite, but make sure to register it using the HoveringAttackEffect field.
  <br></br>
And that's pretty much it!

## HatOffset & HatOffsetModel
HatOffset is a dictionary that provides instructions for the mod to display the pet's hat correctly on its texture (frame specific).<br></br>
This is because the mod needs to know the hat's direction and position at certain frames.
For example: At frame 24, the hat should be 3.5px to the right, 10px higher, and facing east.<br></br>
As such, a HatOffset dictionary fulfills that role by providing those instructions in an organized manner:

```js
"HatOffset": { 
    "1, 2": [ 4, 4, 2 ],
    "35, 6, 23": [ 3, 4, 0 ],
    "24": [ 3.5, -10, 1 ]
  }
```
Each key in the HatOffset dictionary follows a set of rules that help facilitate the process of iterating through each frame individually:

```js
"frame": [ xOffset, yOffset, direction]
```
* <b>"frame":</b> The specific frames these instructions are meant for.
  <br>Example: <code>"12, 21"</code> means that these instructions are meant for when the pet's current frame is either "12" or "21".</br>
  
* <b>xOffset:</b> Represents the horizontal offset relative to the pet's sprite center.
  <br>Example: <code>-12.5</code> means "Twelve and a half" pixels to the left relative to the center of the pet's sprite.</br>

* <b>yOffset:</b> Represents the vertical offset relative to the pet's sprite center.
  <br>Example: <code>-12.5</code> means "Twelve and a half" pixels higher relative to the center of the pet's sprite.</br>
  
  <em>*This is because the vertical axis is inverted, this means that negative numbers translate to higher altitudes in-game.</em>

* <b>direction:</b> The direction the hat should be facing at this frame.
  <br>Example: <code>1</code> means that the hat should be facing East at this current frame.</br>
  
  <em>*Directions (in-game) are writen as numbers, this means that `0` is North, `1` is East, `2` is South and `3` is West.</em>

### HatOffsetModel
A HatOffsetModel offers a pre-made solution for people that don't want to write a HatOffset dictionary themselves.

```js
"HatOffsetModel": "dog4"
```
There are quite a few HatOffsetModels available to use, as they are based on vanilla dog/cat breeds HatOffsets:

 * <code>cat</code>
 
   ```js
   "HatOffset":{
     "16":[0,-1,2],
     "24":[6.5,0,1],
     "25":[6.5,3,1],
     "26":[6.5,4,1],
     "27":[7.5,4,1],
     "32":[5.5,-3,1],
     "33":[4.5,-4,1],
     "47":[4.5,-4,1],
     "68":[0,-6,0],
     "69":[0,-5,0],
     "0,2":[0,0,2],
     "1,3":[0,1,2],
     "4,6":[6.5,-2,1],
     "5,7":[6.5,-1,1],
     "8,10":[0,-6,0],
     "9,11":[0,-5,0],
     "12,14":[-6.5,-2,3],
     "13,15":[-6.5,-1,3],
     "17,21,23":[0,-3,2],
     "18,19,20,22":[0,-4,2],
     "28,29":[-0.5,4,3],
     "30, 31":[5.5,0,1],
     "34,36,37,38,40,41,42,43,44,45,46,48,49,50,51,52,53,54,55,56,57,58":[2.5,-4,1],
     "60,62":[0,-7,2],
     "61,63":[0,-6,2],
     "64,66":[4.5,-8,1],
     "65,67":[4.5,-7,1],
     "59,35,39":[0,0,2]
   }
   ```

* <code>dog</code>
 
   ```js
   "HatOffset":{
     "16":[0,-6,2],
     "17":[0,-8,2],
     "20":[5.5,-7,1],
     "21":[4.5,-8,1],
     "22":[3.5,-9,1],
     "26":[3.5,-11,1],
     "32":[5.5,-8,1],
     "33":[5.5,-7,1],
     "34":[5.5,-5,1],
     "52":[0,-15,0],
     "53":[0,-14,0],
     "0,2":[0,-4,2],
     "1,3":[0,-3,2],
     "4,6":[5.5,-6,1],
     "5,7":[5.5,-5,1],
     "8,10":[0,-12,0],
     "9,11":[0,-11,0],
     "12,14":[-6.5,-6,3],
     "13,15":[-6.5,-5,3],
     "18,19,27,36,37,38":[0,-9,2],
     "23,24,25,30,31,40,41":[3.5,-10,1],
     "28,29":[5,0,2],
     "44,46":[0,-9,2],
     "45,47":[0,-8,2],
     "48,50":[5.5,-9,1],
     "49,51":[5.5,-8,1],
     "35,39,42,43,54,55":[0,0,2]
   }
   ```

* <code>dog1</code>
 
   ```js
   "HatOffset":{
     "16":[0,-6,2],
     "17":[0,-9,2],
     "20":[5.5,-7,1],
     "21":[4.5,-7,1],
     "22":[3.5,-11,1],
     "26":[3.5,-12,1],
     "32":[6.5,-9,1],
     "33":[6.5,-8,1],
     "34":[6.5,-6,1],
     "52":[0,-15,0],
     "53":[0,-14,0],
     "0,2":[0,-4,2],
     "1,3":[0,-3,2],
     "4,6":[5.5,-6,1],
     "5,7":[5.5,-5,1],
     "8,10":[0,-12,0],
     "9,11":[0,-11,0],
     "12,14":[-6.5,-6,3],
     "13,15":[-6.5,-5,3],
     "18,19,27,36,37,38":[0,-10,2],
     "23,24,25,30,31,40,41":[3.5,-11,1],
     "28,29":[5,-1,2],
     "44,46":[0,-9,2],
     "45,47":[0,-8,2],
     "48,50":[5.5,-9,1],
     "49,51":[5.5,-8,1],
     "35,39,42,43,54,55":[0,0,2]
   }
   ```

* <code>dog2</code>
 
   ```js
   "HatOffset":{
     "16":[0,-6,2],
     "17":[0,-8,2],
     "20":[5.5,-5,1],
     "21":[4.5,-7,1],
     "22":[3.5,-8,1],
     "26":[3.5,-9,1],
     "32":[5.5,-7,1],
     "33":[5.5,-6,1],
     "34":[5.5,-4,1],
     "52":[0,-14,0],
     "53":[0,-13,0],
     "0,2":[0,-3,2],
     "1,3":[0,-2,2],
     "4,6":[5.5,-4,1],
     "5,7":[5.5,-3,1],
     "8,10":[0,-11,0],
     "9,11":[0,-10,0],
     "12,14":[-6.5,-4,3],
     "13,15":[-6.5,-3,3],
     "18,19,27,36,37,38":[0,-9,2],
     "23,24,25,30,31,40,41":[3.5,-8,1],
     "28,29":[5,0,2],
     "44,46":[0,-8,2],
     "45,47":[0,-7,2],
     "48,50":[5.5,-7,1],
     "49,51":[5.5,-6,1],
     "35,39,42,43,54,55":[0,0,2]
   }
   ```

* <code>dog3</code>
 
   ```js
   "HatOffset":{
     "16":[0,-6,2],
     "17":[0,-8,2],
     "20":[5.5,-7,1],
     "21":[4.5,-8,1],
     "22":[3.5,-9,1],
     "26":[3.5,-11,1],
     "32":[5.5,-8,1],
     "33":[5.5,-7,1],
     "34":[5.5,-5,1],
     "52":[0,-15,0],
     "53":[0,-14,0],
     "0,2":[0,-4,2],
     "1,3":[0,-3,2],
     "4,6":[5.5,-6,1],
     "5,7":[5.5,-5,1],
     "8,10":[0,-12,0],
     "9,11":[0,-11,0],
     "12,14":[-6.5,-6,3],
     "13,15":[-6.5,-5,3],
     "18,19,27,36,37,38":[0,-9,2],
     "23,24,25,30,31,40,41":[3.5,-10,1],
     "28,29":[5,0,2],
     "44,46":[0,-9,2],
     "45,47":[0,-8,2],
     "48,50":[5.5,-9,1],
     "49,51":[5.5,-8,1],
     "35,39,42,43,54,55":[0,0,2]
   }
   ```

* <code>dog4</code>
 
   ```js
   "HatOffset":{
     "16":[0,-6,2],
     "17":[0,-8,2],
     "20":[5.5,-7,1],
     "21":[4.5,-8,1],
     "22":[3.5,-9,1],
     "26":[3.5,-11,1],
     "32":[5.5,-8,1],
     "33":[5.5,-7,1],
     "34":[5.5,-5,1],
     "52":[0,-15,0],
     "53":[0,-14,0],
     "0,2":[0,-4,2],
     "1,3":[0,-3,2],
     "4,6":[5.5,-6,1],
     "5,7":[5.5,-5,1],
     "8,10":[0,-12,0],
     "9,11":[0,-11,0],
     "12,14":[-6.5,-6,3],
     "13,15":[-6.5,-5,3],
     "18,19,27,36,37,38":[0,-9,2],
     "23,24,25,30,31,40,41":[3.5,-10,1],
     "28,29":[5,0,2],"44,46":[0,-9,2],
     "45,47":[0,-8,2],
     "48,50":[5.5,-9,1],
     "49,51":[5.5,-8,1],
     "35,39,42,43,54,55":[0,0,2]
   }
   ```

* <code>dog5</code>
 
   ```js
   "HatOffset":{
     "16":[0,-6,2],
     "17":[0,-8,2],
     "20":[5.5,-7,1],
     "21":[4.5,-8,1],
     "22":[3.5,-9,1],
     "26":[3.5,-11,1],
     "32":[5.5,-8,1],
     "33":[5.5,-7,1],
     "34":[5.5,-5,1],
     "52":[0,-15,0],
     "53":[0,-14,0],
     "0,2":[0,-4,2],
     "1,3":[0,-3,2],
     "4,6":[5.5,-6,1],
     "5,7":[5.5,-5,1],
     "8,10":[0,-12,0],
     "9,11":[0,-11,0],
     "12,14":[-6.5,-6,3],
     "13,15":[-6.5,-5,3],
     "18,19,27,36,37,38":[0,-9,2],
     "23,24,25,30,31,40,41":[3.5,-10,1],
     "28,29":[5,0,2],
     "44,46":[0,-9,2],
     "45,47":[0,-8,2],
     "48,50":[5.5,-9,1],
     "49,51":[5.5,-8,1],
     "35,39,42,43,54,55":[0,0,2]
   }
   ```
* <code>custom</code><br>This is an empty model, use it if you want to write the HatOffset dictionary yourself.</br>

<em>*Note that if you want to use a HatOffsetModel but need to alter some of its keys, you can write them in your HatOffset dictionary. The overlapping offsets will be added together and the direction will be replaced with a new one from the dictionary.</em> <br></br>For example: If frame "12" on the model is `[ -10, 10, 1 ]`, and frame "12" on the dictionary is `[ 4, 0, 0 ]`, the result will be `[ -6, 10, 0 ]`.


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
Refers to enemies that receives more damage from this pet via a DamageMultiplier variable.
<br></br>Structure:

```js
{
  "Name": "Skeleton", //The name of the monster.
  "DamageMultiplier": 3 //Effectiveness damage multiplier.
}
```
The final damage output is calculated by multiplying the finished damage output with the DamageMultiplier of that monster.
