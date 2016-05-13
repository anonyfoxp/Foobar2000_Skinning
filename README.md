# Foobar2000_Skinning

http://forum.foobar2000.com/forum/showthread.php?114-Skinning/

#Skinning
A foobar mobile skin (.fbskin) is created using the foobar mobile skin compiler (windows only), which compiles multiple skins and images into a single file.

A single skin will contain 8 skins, skins are resolution independent, the 8 skins are needed to accurately cater to the various aspect ratios needed for the various devices, each skin contains:

Code:
```
landscape-3-2-4000x2666
landscape-4-3-4000x3000
landscape-16-9-4000x2250
landscape-16-10-4000x2500
portrait-2-3-2666x4000
portrait-3-4-3000x4000
portrait-9-16-2250x4000
portrait-10-16-2500x4000
```

All files when creating a skin should be in a single folder, edit the skindef.skindef file (drop existing onto notepad). Then edit the .txt skin files. Finally double click the skindef.skindef file to run SkinDesigner.exe to compile the skin. 

Example skindef.skindef file:

Code:
```
name: Test Skin
author: Spoon
version: 1.0
defaultart: noart.png
iconplay: iconplay.png
iconpause: iconpause.png
iconfolder: iconfolder.png
backgroundcol: 255,255,255
genericfont: 0,0,0
artistfont: 128,128,128
titlefont: 0,0,0;bold
albumfont: 0,0,0


skin: 4000x2250 landscape-16-9-4000x2250.txt
skin: 4000x2500 landscape-16-10-4000x2500.txt
skin: 4000x2666 landscape-3-2-4000x2666.txt
skin: 4000x3000 landscape-4-3-4000x3000.txt


skin: 2250x4000 portrait-9-16-2250x4000.txt
skin: 2500x4000 portrait-10-16-2500x4000.txt
skin: 2666x4000 portrait-2-3-2666x4000.txt
skin: 3000x4000 portrait-3-4-3000x4000.txt

```

Running SkinDesigner, compiles the skin to a single .fbskin file and then renders the skin in an example program which allows the various devices to be tested against your skin. 

The Now Playing page can be 100% skinned, the browse and playlist pages (and settings) offer minimal skinning beyond colour setting.

In the .skindef file are the following elements:

- defaultart - a place holder image for when the player is stopped, or a file has no art.
- iconplay, iconpause, iconfolder are icons to be used in track listing, they should be at least 400x400 pixels and contain an alpha layer.
- backgroundcol - the background colour for Browsing and Playlist, in r,g,b
- genericfont - the font used, specifies colour in r,g,b and optional ;bold;itallic values, example 0,0,0;itallic
- artistfont - font used for artist
- titlefont - font used for title
- albumfont - font used for album

Each of the 8 skin files are made up from components, these components draw the skin they are:


A filled rectangle
Code:
```
[rectangle]
red,green,blue
x1,y1,w,h

example (of a dark red rectangle which fills the whole page):

[rectangle]
128,0,0
0,0,4000,2250
```

A frame rectangle (1 pixel wide frame), that is unfilled:
Code:
```
[frame]
red,green,blue
x,y,w,h

example (of a bright green framee):

[frame]
0,255,0
100,100,3800,2050
```

A line which is 1 pixel wide:

Code:
```
[line]
red,green,blue
x1,y1,x2,y2

example blue line running right across top part of screen:

[line]
0,0,255
```

An image, images should be png and can contain a transparency. The image should be designed with a total screen size of 4000x3000 in mind (or 3000x4000), in that if your image is to take up 1/4 of the screen, it should have a resolution of 4000/4 = 1000 pixels. For devices with much lower resolutions (such as a nokia Lumia 520, which is 480x800) the image will be scaled down, in this instance the image would be 1/10th its designed size!, images should be designed with this in mind.


Code:
```
[image]
<imagefilename>
x,y,w,h

example of an image placed in lower right corner, the actual image is drawn to be 200x200 pixels at 4000x3000 resolution:

[image]
testimage.png
3775,2025,200,200
0,200,4000,200
```

##A Label

The label can be of a fixed text value, or a recognized values which are replaced when playing back, elements can be:

[length]
[currentposition]
[remaining]
[volumelbl]
[artist]
[title]
[album]
[genre]
[year]
[composer]
[conductor]
[album artist]
[artistmulti]
[album artist multi]
[nextartist]
[nextalbum]
[nexttitle]

artistmulti: would list contributing artists as well as artist1; artist2; artist 3

In addition it is possible to set colour, bold and italic in the label with [b] [-b] [i] [-i] [rgb-255-0-0]. The [-b] and [-i] items remove the bold or italic. Colour changes are [rgb-red-green-blue].

For example it is possible to have a label as:

[b][artist][-b] - [rgb-128-128-128][title]

The default colour for a label is black.

Code:
```
[label]
x,y,w,h
flags
<label text>
```

Flags can be (combine multiple with , for example centered,noreducesize):

left
centered
right
noreducesize

The font size is determined by the height of the allocated label, normally if the label is too large to fit the width the size of the font is reduced, this only happens to 50% of the default font size. If the label is still too large, it is shortened with .. on the end.

##Buttons

Buttons are controls which have 2 states - up and down, a button is named either:

play
pause
stop
playnpause
skipback
skipnext
mute
menu

Code:
```
[button]
x,y,w,h
<name>
<image up/off>
<image up/off highlighted>
<image down/on>
<image down/on highlighted>

```
The highlighted buttons are shown whilst the user is actively clicking the button. The mute button has 3 special states which can be supplied with <image up/off>;<image2bar>;<image1bar> which effectively reduces the volume image when the volume is changed.

##Volume Control

Shows a volume control (on a bar), to change the volume a user must first pickup the marker and then move the marker. The bar is made up of 3 components, a selector mark, back image for the progress bar and an optional set bar image which shows how much volume is set, for example volume is set to 75% then 75% of the set bar is shown.

The bar height should be the same height as the bar marker, it is possible to make the bar appear thin by using a transparent png (above and below the bar image). The bar should be of a height which can be selected by a finger, even if most of the bar is transparent.

Code:
```
 [volume]
 x,y,w,h
< Volume Marker Image>
< Volume Back Image>
< Optional Set Image>
```

##Playback position

Shows the current track position (on a bar) and allows skipping to any point on the bar. The bar is made up of 3 components, a selector mark, back image for the progress bar and an optional progress bar image which shows progress made (to the left of the marker, it can be a different color to main back image). The bar height should be the same height as the bar marker, it is possible to make the bar appear thin by using a transparent png (above and below the bar image). The bar should be of a height which can be selected by a finger, even if most of the bar is transparent.

Code:

```
[position]
x,y,w,h
<Position Marker Image>
<Position Back Image>
<Optional Position Highlight>
```

##Album Art


The main feature of the Now Playing page is the display of album art


Code:

```
[albumart]
x,y,w,h
<type>


Where type can be: main, next, mainback, mainmirrored


main: copies 1:1 the art to the screen
next: copies 1:1 the art of the next track to play to screen
mainback: takes the art and zooms in, desaturates and dims the art so it can be used as a backdrop
mainmirrored: art drawn in reflective way, the reflection fades out. A normal 'main' should be drawn, then a mainmirrored drawn below main, with the height set to 1/3rd of main.
```

##Dynamic Skins


It is possible for a skin to be dynamic, that is change depending on certain circumstances, when designing a skin it can be segmented into conditional blocks, for example (important * should be replaced by the hash symbol):


Code:

```
 *if playing

< this skin code executed when playing>

 *else


< this skin code executed when not playing>


 *end
```

Code:

```
*if classical

<this skin code executed when playing classical tracks>

*else


<this skin code executed for all other tracks>


*end
```

Code:
```
 *if next

< when there is a track queued next>

*else

< when nothing will play next >


 *end
```

A word about transparencies, take the following zoomed images (1 and 2), image 1 is a simple red line with an alpha layer, the line is perfectly masked:

alphalayersize.jpg

Now image 2 is image 1 resized down (as the program might do to put the image on a display), it has been reduced to 21%. The alpha issue arises now with the edge pixels, in image 2 they are lightened red (because the background color under the alpha layer is white), this is called color bleeding. If the background was black the edge red would be darkened. Depending upon onto which the final image will be drawn to, the lighter or darker image might look totally wrong. With this in mind internally foobar will set the edge pixel to blend against the shown neighbour, but even this is not perfect. To create perfectly sizing images, it is best to design images with either a hard edge, or no alpha transparency (depending upon skin).

#The Actual Skin Designer

- Install Skin Designer:

http://mobile.foobar2000.com/install/Foobar2000MobileSkinDesigner.exe

(Windows only)

- Then download first the example Skin:

http://mobile.foobar2000.com/install/SpoonTestSkin.zip

Unzip, then double click the skindef file, it will show the skin on the screen.

- To create your own skin, start with a blank skin:

http://mobile.foobar2000.com/install/BlankSkin.zip

Rename the folder, open notepad and drop the skindef file onto it for editing, change the name, author, etc. Save then change each of the landscape or portrait files, to test double click your skindef file. The skin will be compiled and shown in the designer.

The designer and sample skins have been updated for the latest version of foobar2000 mobile, now including folder pictures.

#How to load user-made skin files
##iOS
Upload fbskin files to your device using iTunes file sharing. Uploaded fbskin files will show up on the "choose skin" page.

##Android
"Choose skin" page looks for *.fbskin on all mounted storage devices, so putting .fbskin files anywhere on your internal or external storage should be enough.
If your skin files are not being picked up by it, please do tell us (including exact phone model and system version) and we'll work around it.

##Windows
At the bottom of the "choose skin" page, there's an "open" command that lets you add skin files from any location, including any local folder as well as MS OneDrive. Skin files will be copied to foobar2000's configuration so you can safely delete them once they've been imported. To remove imported skin files, tap+hold the individual entry and choose "delete".

Please note as skinning is in it's infancy there is no provision for uploading skins to our server, this will change in the coming months. For now please post skins with download links to dropbox or similar.
