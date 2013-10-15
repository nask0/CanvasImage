CanvasImage
===========

Converts image to canvas with same look.

Useful for memory optimization in case of responsive images usage. Also is good for solving PhoneGap Android antialiasing issue.

Usage
-----

Simpliest way to use CanvasImage is by calling <code>new CanvasImage('selector')</code> or <code>new CanvasImage(DOMElement)</code>

There is also approach to convert all images on current page. For this use <code>CanvasImage.all()</code>

Memory consumption optimization use case
---

Responsive images are common citezens of web applications. Very often they are a bit larger than we need for particular screen size. 

For example you have universal web app which works on desktops as well as on iPhones. But for iPhone it is preferred to have smaller images than for desktops in order to decrease memory consumption. Most obvious way to  implement this is to create few sets of images for different screen sizes however this is not easy to do - you will need additional files, styles and bunch of time. Here CanvasImage is a game changer.

How it works: 

- Library finds all img tags on page
- Replaces them with canvas tags of same size
- Draws images content to canvases reducing size to needed one
- Removes old big images from memory

After this we have set of canvases which look absolutely like images we had before but with one difference - we removed all image data which is not needed for our current window size.


![image](http://pixelscommander.com/polygon/canvasimage/memory-consumption-diagramm.png)


Android anti-aliasing issue solution
---

A lot of Android devices have very annoying browser behaviour - it disables antialiasing on every touch to optimize rendering. This creates some kind of blinking effect when user touches screen.

It is possible to avoid such this problem with disabling browser anti-aliasing with CSS in this way there will be nothing to disable. Have sense?

Let`s add next CSS code to stylesheet:

		* {
    		
    		image-rendering: optimizeSpeed;
    		image-rendering: -moz-crisp-edges;
    		image-rendering: -o-crisp-edges;
    		image-rendering: -webkit-optimize-contrast;
    		image-rendering: optimize-contrast    			-ms-interpolation-mode: nearest-neighbor;
		}

Yes, this will work until we do not have responsive images in app. Since we will start to use them - issue will appear again. This happens because of difference between antialiasing mechanisms in CSS engine and Android WebView component. CSS antialiases images before they were scaled and browser antialiasing is being applied after scaling.

To avoid this effect you have to disable scaling somehow.

Here CanvasImage can be best approach possible beacuse it replaces scaled images with unscaled canvases.