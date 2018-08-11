Title: Tripod Quick release mount
Date: 2014-06-13
Tags: Maker, 3DPrinting

I got a tripod for my birthday this year.  It is a really nice one, but I lost
the quick release mount.  Here is a picture with a quick release mount from
another tripod. Obviously, it doesn't fit:  

[![](http://1.bp.blogspot.com/-W5sRA0U8kIU/U4EX3Fs1CqI/AAAAAAAACrk/msWlZre9TrM/s1600/Tripod.jpg)](http://1.bp.blogspot.com/-W5sRA0U8kIU/U4EX3Fs1CqI/AAAAAAAACrk/msWlZre9TrM/s1600/Tripod.jpg)

  
After some shopping online this one looks fairly good, but possibly not the
right size.  
  
<http://www.tripodquickrelease.com/Acme_Lite_Quick_Releases.htm>  

  

So, I took some measurements and came up with this drawing  

[![](http://4.bp.blogspot.com/-oe1mhaWtLF0/U4EXoKoBbYI/AAAAAAAACrc/8c3a8Ovp3iE/s1600/quickreleasemount.jpg)](http://4.bp.blogspot.com/-oe1mhaWtLF0/U4EXoKoBbYI/AAAAAAAACrc/8c3a8Ovp3iE/s1600/quickreleasemount.jpg)

  
My goal is to see if I can print this out using a 3D printer. When I started
this project I was planning to use the one in the engineering department where
I work, but now I have an Ultimaker 2.  I know I need an stl file but I was
not sure the best way to generate one. After some quick internet searching I
found openscad.  
  
<http://www.openscad.org/>  
  
I downloaded the software and found the wiki quick start page.  It did not
take long to figure out the simple language.  I started by making a cube and
then creating the top.  Then I made objects to cut out the inside and the
bevels.  Here is the resulting model.  

[![](http://4.bp.blogspot.com/-KxmLidG-IlQ/U5Zd9qJwjCI/AAAAAAAACts/sI-
MG1OVNMA/s1600/QuickRelease.png)](http://4.bp.blogspot.com/-KxmLidG-
IlQ/U5Zd9qJwjCI/AAAAAAAACts/sI-
MG1OVNMA/s1600/QuickRelease.png)[![](http://1.bp.blogspot.com/-W8GoOF1PptE/U5ZeG-
Fe9FI/AAAAAAAACt0/K_AKh2LQjvY/s1600/QuickRelease2.png)](http://1.bp.blogspot.com/-W8GoOF1PptE/U5ZeG-
Fe9FI/AAAAAAAACt0/K_AKh2LQjvY/s1600/QuickRelease2.png)

  
  
The hardest part was to get the surface normals for the wedge shape object to
face in the right direction.  However, once I drew out my points and used the
right hand rule things settled in nicely.  Openscad outputs stl files so all I
did was import it into the cura program to generate the gcode for my printer
and save it to the SD card to print.  The first print was a little too close
to my tolerances and I could not get it to fit inside the tripod.  However,
with some simple modifications to the openscad file I was able to generate a
quick release mount that I think looks quite nice and works well:  
  
  

[![](https://lh3.googleusercontent.com/-4-U6DIDgzEY/U5uaNLs_Y3I/AAAAAAAACuk/C4IHGAOs0ec/s320/blogger-
image-138421032.jpg)](https://lh3.googleusercontent.com/-4-U6DIDgzEY/U5uaNLs_Y3I/AAAAAAAACuk/C4IHGAOs0ec/s640/blogger-
image-138421032.jpg)

  

A future design would fix the bottom to the mount somehow. I posted the STL
and OpenScad files on YouMagin.org if you are interested in makeing your own:  
  
<https://www.youmagine.com/designs/tripod-quick-release-mount>  
     
\- Dirk  
  
  
  

Blogpost migrated from [Blogger](https://apprenticemaker.blogspot.com/2014/06/tripod-quick.html) using costom python script. Comment on errors below.
