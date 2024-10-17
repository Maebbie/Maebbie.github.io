# How to swap out Baked Lightmaps for Day&Night mode

[<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a05.jpg" width="256">](https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a05.jpg)

This Guide will show you how you can swap Baked Lightmaps in VRChat. This is useful to create a Day and Night version of your world or multiple different lighting scenarios.   
Compared to other common solutions, such as utilizing real time lights or using a tinted overlay, this method is both optimized and represents light better.

The core principle behind this method is to bake light differently on multiple copies of the same static mesh by placing them in different areas of your scene and later stacking them to be at the same location. This needs to be done, because VRChat does not allow you access to change Lightmaps directly.

I have made a [**video guide**](https://www.youtube.com/watch?v=VLU6A4JWsWk&list=PLHPl0SFKkUjNIZs5K9BV4pgXiLrQpEu2D) on this 4 years ago, but this guide has all images retaken and is reworked.

## Setting up the Scene  

Start by putting everything in your scene you would like to bake as a child of an empty Gameobject. For this purpose calling it "static" would be fitting.   
Have another Gameobject, which will have your Light Entities, Reflection Probes and all other non-static objects that may have different materials set up for your different lighting conditions. "dynamics" may be a fitting name for it. Things such as Water shaders that you would like to appear darker for a night version by editing its material.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a01.jpg" width="512">

Use this as your base to adjust the lighting. Use a duplicate of that scene (via Ctrl+D) to adjust the lighting for your additional lighting conditions. These copies of your scene won't have an effect on your main scene.   
When adjusting the lighting please be aware, that you are limited to only one Directional Light later. You can use layers here to have multiple directional lights affect different meshes, however a cleaner approach would be to utilize a Spot Light to replace additional Directional Lights.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a02.jpg" width="512">

As you see a spotlight placed far away will serve as a great alternative.   
On open worlds such us Atoll of Ether, that has a baked Day and Night version you can utilize a large plane, set to "shadows only" to block the directional light used for the day version creeping into the night version.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a03.jpg" width="512">

## Baking the Lights

Once you are satisfied with having set up your different lighting scenarios, copy the Gameobject containing the non-static other objects and light entities (previously referred to as "dynamics" Gameobject) to your main scene.   
Then make a duplicate of your "static" gameobject within your hierarchy, that contains your static gameobjects.   
Let's assume you would like to do a Day and Night version of your world, keep your Day-static and Day-dynamic Gameobjects as is and move the freshly duplicated Night-static and Night-dynamic Gameobjects far below that. This is how they can be baked in different ways.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a03a.jpg" width="512">

Use the large plane set to "shadows only" mentioned earlier to separate the 2, so lighting remains separate.

Now Bake your scene.

## Stacking the Geometry

The reason to keep things tidied up within 2 Gameobjects per scene is to be able to handle them easily in this step.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a04.jpg" width="512">

As you see, after your bake you will now have 2 distinct lighting conditions spaced far apart. Make sure you baked Lightprobes and Reflection probes as well. Now simply take those versions and after that place them all on the same coordinates. Simply setting the relevant Gameobjects (the "static"'s and "dynamic"'s) to 0,0,0 will do.

Please note that it is not possible to bake 2 or more different versions of Lightprobes, so choose the set you think fits best for all lighting scenarios. You may also opt to have only Avatars lit by a Realtime Light without shadows, this comes at a relatively large performance cost.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a05.jpg" width="512">

## Changing Skyboxes

You cannot rely on just having 1 Skybox set in the Lighting settings if you would like to change them along with your lighting.   
Instead add an inverted sphere with a Skybox material applied for each of your lighting scenarios. Leave the skybox in your Lighting tab blank. Alternatively you can use Lyuma's SkyboxCubemapDual shader that allows you to change between 2 Skyboxes based on the rotation of the Sun entity: [https://gist.github.com/lyuma/38648c33c91a1994b98c5c5d0168a575](https://gist.github.com/lyuma/38648c33c91a1994b98c5c5d0168a575) .   
Using a Procedual Skybox and adjusting the rotation of the Sun entity is also a good solution.   
Make sure your Reflection Probes have full coverage of your World, as the fallback Reflection Probe is derived from the skybox set in the Lighting tab, which you may decide to leave blank.

### Interact Setup

Now that you have everything baked and stacked on each other, you will likely want to change between your bakes ingame. To do so simply create a udon behaviour that enables one version and disables the others.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a06.jpg" width="512">

.Alternativly you can also make a Animation that enables and disables the components, this may be useful should you use a Procedural Skybox and want to animate the sun to move across the sky, along with the baked lightmap change.   
Should you do this, a simple Animator Bool Toggle will be enough.

## Important Last Step

I would like to end this Guide with a very important note.   
Please have all your baked variants enabled by default when loading in! This is important for Unity to be able to batch everything.   
Let's say you want to start out in your Day version of your world and disable the Night version in unity, since it seems it wont be needed for now. If you do that and the user switches to the Night version ingame, it will render the Night version unbatched, meaning all the performance benefits gained by this method are for nothing.

Instead, have everything enabled when loading in, setup a OnEnable udon event and have that disable all your Lighting variants, except the one you wish to spawn in. This will ensure all Lighting variants are batched. The difference this makes is immense and crucial.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/Swap-Baked-Lightmaps-VRChat/images/a07.jpg" width="512">

Swapping out baked light has great performance benefits and will feel more natural than using real time light or a tinted camera overlay. Few Worlds use this technique, the one shown in this guide was Atoll of Ether, however the VRChat Home also swaps out baked light.

This article and its contents by Maebbie are licensed under [CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0/)
