# How to Atlas Everything in your World (literally)
[<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/28.jpg" width="512">](https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/28.jpg)

One of the most powerful tools to optimize your World for best performance is to use as few materials as possible. This is important, since each material has a compounding requirement of drawcalls/batches to render your scene. Simplified this means, each material generates 1 drawcall inherently per object or batched object. However each reflection probe and lightmap texture applied does add an additional one each. Other factors compound this even further, but the bottom line here is to highlight that each material added to your world will have a substantial performance impact. Reducing Materials used, means reducing drawcalls. This is crucial, because drawcalls are an instruction from a CPU to a Graphics Card to render something in a certain way. This is something that happens every single frame. VRChat is a CPU bottlenecked platform, where you GPU has a lot of free headroom, therefore the less strain you put on your CPU the more performance you gain.

If you are striving to have visual fidelity combined with high performance, then making a single Atlas Texture of all your textures used in your world will be the key to that. This advanced Guide will go into great detail on how to best achieve this task. It showcases a small example to follow along, as well as how it was done in the World Eventide.

Please note that you should be Baked Light and marking all non-moving objects as Batching, before you continue. These are 2 easy methods that also have a huge impact on performance.

I made a **[video guide](https://www.youtube.com/watch?v=urrhgBHVYGQ&list=PLHPl0SFKkUjNcC8r3tLFW1-WCdNCAAFjV)** about this 4 years ago, which is a bit outdated. For this Guide I retook all images and reworked it, so i recommend this guide.

## Preparing Models

When applying a material in Unity it will project its texture the way its UV map is projected on the model. Often that means you will have the material tiling along the surface of the model. On the UV Map this looks as though the UV Face is going out of bounds of its texture.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/01.jpg" width="512">

However this means, you are limited to to only having one texture per material. If you were to add multiple textures to one image, your scene's textures would fall apart quickly.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/02.jpg" width="512">

This model can be adjusted to allow for tiling a single texture from an atlas however. All you would need to do is to add several vertical and horizontal edge cuts to your model where you would want to repeat your texture, so that each square can represent one instance of a texture tiling.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/03.jpg" width="512">

The process of getting there however can be unintuitive, which makes this a somewhat advanced technique. You will want to ensure, that each square you cut into an object is roughly the same size.

The best way to get these cuts is to use the Knife Project function in blender under Mesh -&gt; Knife Project (or search it up via F3). To use this tool correctly You should prepare a large grid of edges with no faces. To do so subdivide a large plane, then Shortcut: X while in edit mode and select "Only Faces".

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/04.jpg" width="512">

Then select the Object you wish to cut through using this grid.

Enter Edit Mode

Hold Ctrl while in edit mode of said object and only then select your grid, still holding Ctrl.

With all this done head to Mesh -&gt; Knife Project.

Line it up so the grid of edges is in front of your model. Make sure to enable Orthographic view before, to remove perspective (Shortcut: Numpad 5).

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/05.jpg" width="256">

Execute Knife Project and check the "Cut Through" option on the bottom left of the viewport. (See below)

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/06.jpg" width="512">#

By toggling "Cut Through" you can update the cuts made before confirming. Just make sure it is checked at the end.

## Setting up UV's for an Atlas

The model is now prepared to manually tile textures on. To make this easier, continue using your materials with a single texture and not a texture on your atlas.

To start out, find a square with as little cuts as possible. UV map the texture on it so that it fills out the entire uv space from 0,0 to 1,1. As in only where the texture is shown within UV Editing.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/07.jpg" width="512">

This will serve as your base.

Next use the blender plugin "Magic UV" to extend the UVs of this square onto adjacent squares. This can be done using UV -&gt; Texture Wrap in the 3D viewport, set the options "Refer" and "Set" as quick favourites or keybinds. I set them to C and D.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/08.jpg" width="512">

Now use "Refer" on the face that has your texture uv-mapped on and "Set" on a face next to it.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/09.jpg" width="512">

Do this until you have filled an entire adjacent square. Often a square will have other edges intersecting it or cut off part way through.

As you see, your UV editing window now has 1 square filing out the original 0,0 to 1,1 UV space and one of a very similar size adjacent to it, overshooting.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/10.jpg" width="512">

Since the goal is to use a texture from a texture atlas to tile everything, you should now move the newly UV mapped square from the adjacent UV space to your original 0,0 to 1,1 space. Stacking it on top of existing UV faces. Using Shortcut: Y will detach your selection into a separate island, use this before should it stick to adjacent faces.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/11.jpg" width="512">

After you stacked it on the space, fix up minor discrepancies so that this newly uv mapped square is taking up the full 0,0 to 1,1 UV space. Ensure the correct alignment of each UV face, by moving it in 1 UV Texture increments only.

Repeat this process for your entire model.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/12.jpg" width="512">

While the method described above will work for most parts of a model, this is not the only way to stack your uv maps. Smaller models may not need to have additional cuts at all.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/13.jpg" width="512">

For long roads you could make a 4 by 4 meter section of it, which you uv map and then have an array modifier repeat, resulting in no additional work needed afterwards.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/14.jpg" width="512">

Or sometimes it may just be worthwhile to manually uv map parts of a model again instead of using "Set" and "Refer".

Experimenting with normal UV project and UV Cube Project can help too.

As explained in the intro to this guide, the gpu has a lot of headroom available, meaning you can add many more polygons without any performance issues. This is what this method makes use of. The drawcalls saved by saving materials is far more beneficial than the polygons added to compensate.

## Creating an Atlas Texture

After finishing UV mapping all models within your World this way and moving everything to occupy the 0,0 to 1,1 UV space, it is now time to make an atlas for this setup.

This is as easy as selecting all individual textures and aligning them on a large texture.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/15.jpg" width="512">

Ensure all textures are aligned accurately in a power of 2 configuration. This means, should you use a 4096 x 4096 (2^12) large atlas and 1024x1024 (2^10) textures, align your textures so that you could fit exactly 16 textures within your atlas. Should you have other textures or colors, place them where a 1024x1024 would have fit with some empty space around it. I.e Solid Colors are grouped in 2 1024x1024 Spaces on the top Left of Eventide's Texture Atlas.

See the grid below as a helpful baseline for your arrangements.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/16.jpg" width="512">

The same applies should you decide to use up to 64 512x512 on a 4096x4096 atlas

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/17.jpg" width="512">

To accurately place textures on your atlas, it is important to keep mip-maps in mind. Mip mapping on single-texture materials help to have them display well at a distance by calculating the pixels shown, based on adjacent pixels. With this setup however, it would mean that mip-maps on the edge of each texture in your atlas are going to take pixels from other textures right next to them into account. This will produce a bad looking grid, especially in VR. This is highlighted right below.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/18.jpg" width="512">

Take that into account by scaling each texture down by 4 pixels on each side, meaning your 1024x1024 texture should be scaled down to 1016x1016 instead. 512x512 should be scaled to 504x504 instead.

After that repeat pixels on each edge of your textures by 4 pixels towards the outside. It will reclaim its original texture size of 1024x1024 by padding it.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/19.jpg" width="512">

Now paste that texture onto your atlas. And repeat this process for all your other textures, while ensuring an accurate power of 2 placement on your atlas.

Having such padding will ensure that mip mapping is calculated properly later.

Repeat all this for your normal maps, emissive maps and the kinds as well, if you like. Mip mapping is less of a concern for maps other than albedo. Ideally you would do this process for all maps.

## Replacing your individual Textures with the Atlas Texture

With your models prepared at **Setting up UV's for an Atlas** and your atlas ready at **Creating an Atlas Texture** it is now time to bring it all together. Follow along closely:

Make your atlas texture the texture of a new material in blender, so that your atlas can be selected as an image in your uv editing window.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/20.jpg" width="512">

Select every object in your blender scene in Object Mode. (Shortcut: A)

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/21.jpg" width="512">

Enter Edit mode and make sure no faces are selected. (Shortcut: Alt+A will remove all selections)

In your "Material Properties" select your first material and press the "Select" button. This will select all faces in your entire project that have that material applied.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/22.jpg" width="512">

In the UV Editing window, you should now see all those faces occupying the 0,0 by 1,1 space. If this is not the case, touch it up accordingly.

As you want to have that occupy a small part of your atlas, scale it down to the real proportions in your atlas.

Should you have used a 1024x1024 texture on a 4096x4096 atlas, this would mean you would want to scale it down to 1016x1016 instead.

As explained in **Creating an Atlas Texture** the importance of having padding, requires scaling down your textures slightly.

Therefore to calculate how far you need to scale down your UV selection to fit its space on your atlas is as follows:

**1016 (size of texture on your atlas: 1024 minus 8 pixels of total padding) / 4096 (total atlas size)**

in this case your scaling on all axis should be set to: 0.248046875

Using a scale of 0.25 would be wrong!

**For 512x512 textures on a 4096x4096 the calculation would be: 504/4096 leading to 0.123046875**

*I understand you may be skipping through this Guide as it is very extensive, but please be aware of how this calculation came to be from reading the whole guide before it.*

Make a seperate backup of your .blend file here before continuing.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/23.jpg" width="512">

You can also type these calculations as is in blender itself! As you can see on the bottom left on the screen above. Pasting pre-calculated decimals is alright too though.

Move the scaled down UV selection to the corresponding texture on your atlas. If you aligned your textures in a clean power of 2 grid, i.e using the grid above, you should be able to just easily snap it on the right texture. Zoom in and ensure its perfectly centered and that only the 4 pixels of padding on each side remain untouched.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/24.jpg" width="512">

Repeat this process for all other materials, always ensuring that nothing else is selected before you start, otherwise you risk moving previously scaled down textures a second time.

Once finished, do a sanity check by selecting everything in your scene while in edit mode and make sure all textures are filled with UV faces, except their sourronding 4 pixels of padding. Note the padding at the bottom center of the following picture.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/25.jpg" width="512">

Make another separate backup of your .blend file here.

Now that your UV mapping is finished up, remove all materials from every object in your .blend file except your Atlas Material. If an object does not have your Atlas Material yet, add it.

Your only Material remaining should be the Atlas Material.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/26.jpg" width="512">

If all has worked out you should experience an immediate relief in framerates within blender, while your World still looks just the same.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/27.jpg" width="512">

In a real project, it would look something like that:

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/28.jpg" width="512">

Inspect your .blend file for any oddities in textures before finally importing it all into Unity, along with the Atlas texture.

## Advantages of this Atlassing method

This is one of the most advanced techniques you can learn as a World Creator. It can be done for your entire World project, unless you need a special shader for some textures. Water, Audio Link, Video Players, transparent things and the sorts would still need a separate material. On very large projects you may also apply this technique to use 2 or more atlases.

Manually tiling your textures and condensing them all into one Atlas has many advantages;

You will gain a massive performance boost, which leaves you with plenty spare headroom for visual fidelity and Udon programming. Even for Android and IOS!

You are independent from any special shaders, you only need the Standard one. This means maintaining your work relies on no 3. Party. Very Easy! This also means you are completely platform and engine independent, as everything is in one .blend file. I am personally happy to report, that in my many years as a world creator I have never had any issues with things in my worlds breaking that a simple reupload did not fix.

You can also flex your atlas in your World at no performance cost.

<img src="https://raw.githubusercontent.com/Maebbie/Maebbie.github.io/refs/heads/main/UV-Atlas-Everything-Guide-Blender/images/29.jpg" width="512">

### Thanks for your interest in learning this secret art!


This article and its contents by Maebbie are licensed under [CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0/)
