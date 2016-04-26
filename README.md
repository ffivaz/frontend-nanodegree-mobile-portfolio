# Run the app ?

For the portofolio, go to http://fabienfivaz.ch/frontend-nanodegree-mobile-portfolio/index.html

For the pizzas, got to http://fabienfivaz.ch/frontend-nanodegree-mobile-portfolio/views/pizza.html

# What has been changed ?

## Part 1: Optimize PageSpeed Insights score for index.html
To speed up the index.html main portofolio page, different changes have been carried, listed above. The page reaches a mobile PageSpeed of 96 and a desktop PageSpeed of 97.

### Inlining and minifying CSS
Since there are only few CSS commands, they can be inlined to boost performance. It has also been minified with http://cssminifier.com.

### Asynchronous scripts
According to Google (https://developers.google.com/analytics/devguides/collection/analyticsjs/#alternative_async_tracking_snippet), the Analytics script can be loaded asynchronously.

### print.css command at the end of html
For performance reasons, the print.css file can be linked at the end of the file. But there are some discussions around the Internet if this is HTML5 compliant. See for instance : http://stackoverflow.com/questions/21058207/why-does-code-after-html-tag-get-moved-to-before-body-is-there-a-performa. I leaved it here, but it could have been linked before the body closing tag.

### Image size
pizzeria.jpg has been reduced to 100px width.

### Web Font Loader 
Using the webfontloader asynchronously boosts the load times, because fonts can now be loaded, and applied, asynchronously to the page. There are discussions around the Internet if this is the right way to do, since the user sees the page with the wrong font, before seeing the right one. See this discussions for instance: https://css-tricks.com/loading-web-fonts-with-the-web-font-loader/

## Part 2: Optimize Frames per Second in pizza.html

### Code

```"Use strict"``` has been added to force a cleaner and more secure code.

```document.querySelector("#...")``` has been replaced by ```document.getElementId("...")```. ```document.querySelectorAll("#...")``` has been replaced by ```document.getElementByTagName("...")```. They are meant to be faster (but lack some functionality). See http://ryanmorr.com/abstract-away-the-performance-faults-of-queryselectorall/ for example.

Many ```document.getElementByTagName("...")``` or ```document.getElementId("...")``` are called inside loops. In this case, the DOM is accessed each time, which slows the process. Whenever possible, the calls have been moved outside of the loops, DOM is now accessed once.

Variable declarations have been moved outside of loops.

### Image compression
As above, image (pizzeria.jpg) was compressed to smaller sizes using jpegtran. Command:
 
```
jpegtran -copy none -optimize -progressive pizzeria.jpg > pizzeria_optimized.jpg
```

pizza.png was optimized/compressed using http://tinypng.com.
 
### Animation
Two main problems were identified :
 - Animation was based on a loop and not using requestAnimationFrame
 - ```.scrollTop``` was used in the loop, forcing reflow

Some ideas were borrowed from http://www.html5rocks.com/en/tutorials/speed/animations/

```will-change``` attribute was added to the CSS class (.mover) to let the browser know that this element is changing (```style.left```)

#### RequestAnimationFrame
To smooth the animation, the original loop that moved the pizzas in the background was replaced with ```RequestAnimationFrame```. This brings a lot of smoothness to the animation, removes jank and reaches constantly 60 FPS using the FPS Counter/HUD Display.

#### Layout trashing
In the animation, the repeated (in the for loop) call to ```.scrollTop``` forces the browser to redraw the entire layout in each loop, which trashes the layout, basically. By using .ScrollY and moving the functions out of the loop, the number of redraws in far lower, and better for the fluidity.

Further down, ```.style.left``` could also be replaced by:

```
items[i].style.transform = "translate(" + tr + "px, 0px)"; 
```

The are some battles around the webs about which is faster : style.left or CSS translate. I didn't find a clear, settled, answer. BTW, if we are using transform, we have to change the ```will-change``` CSS attribute for class .mover to transform.
 
### Resizing the pizzas
Resizing the pizzas now take 3.97 ms (at least on my older laptop). The problem here was Forced Synchronous Layout: the layout is called (offsetWidth) before the style is updated (```style.width```), and this is done repeatedly inside the loop. I also changed the way the width of the randomPizzaContainer class is changed : using fixed settings avoids the repeated layout calling (```offsetWidth```) to determine dx.
