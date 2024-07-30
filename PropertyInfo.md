# Property Info
A library of information about different fields and methods used in Content Pack making for Pet's Enhanced Mod.

## Contents
* [TargetTexture](#targettexture)
* [FromTexture](#fromtexture)
* [HatOffset & HatOffsetModel](#hatoffset-&-hatoffsetmodel)
* [Commands](#commands)
* [EdibleItems](#edibleitems)
* [EnemiesEffectiveAgainst](#enemieseffectiveagainst)

### TargetTexture

TargetTexture is a property used by the mod to identify the targets of the patches, as such, the mod applies patches to any Pet type character whose texture is stored in its Content Library equally.
<br></br>
By choosing a target texture, you're basically adding it to the internal Content Library. And by extent, making any pet whose texture is the chosen one a valid patch target.
<br></br>

#### Structure of a TargetTexture property
It's structure is that of an string array, this makes it easier to read for all devices. The path starts at StardewValley\Content\, and can only go down folders.
<br></br>Example:


```js

"TargetTexture": [ "Mods", "moddedDogTexture" ] //This translates to: StardewValley\Content\Mods\moddedDogTexture

```
<br></br>

#### How to pick a Target Texture?
A target texture is basically an in-game path to a texture, because of that, there's an easy way to find the path to your desired texture. <br>Here are some common practices:</br>
* The


### FromTexture

### HatOffset & HatOffsetModel

### Commands

### EdibleItems

### EnemiesEffectiveAgainst

