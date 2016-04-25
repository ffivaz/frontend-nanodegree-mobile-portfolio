## Readme file : what has been done ?

## Part 1: Optimize PageSpeed Insights score for index.html
To speed up the index.html main portofolio page, different changes have been carried, listed above. The page reaches a mobile PageSpeed of 96 and a desktop PageSpeed of 97.

### Inlining and minifying CSS
Since there are only few CSS commands, they can be inlined to boost performance. It has also been minified http://cssminifier.com

### Asynchronous scripts
According to Google (https://developers.google.com/analytics/devguides/collection/analyticsjs/#alternative_async_tracking_snippet), the Analytics script can be loaded asynchronously. 

### print.css command at the end of html
For performance reasons, the print.css file can be linked at the end of the file. But there are some discussions around the Internet if this is HTML5 compliant. See for instance : http://stackoverflow.com/questions/21058207/why-does-code-after-html-tag-get-moved-to-before-body-is-there-a-performa. I leaved it here, but it could have been linked before the body closing tag.

### Image compression
Image (pizzeria.jpg) was compressed to smaller sizes using jpegtran. Command:
 
```
jpegtran -copy none -optimize -progressive pizzeria.jpg > pizzeria_optimized.jpg
```

### Web Font Loader 
Using the webfontloader asynchronously boosts the load times, because fonts can now be loaded, and applied, asynchronously to the page. There are discussions around the Internet if this is the right way to do, since the user sees the page with the wrong font, before seeing the right one. See this discussions for instance: https://css-tricks.com/loading-web-fonts-with-the-web-font-loader/

## Part 2: Optimize Frames per Second in pizza.html

### Image compression
As above, image (pizzeria.jpg) was compressed to smaller sizes using jpegtran. Command:
 
```
jpegtran -copy none -optimize -progressive pizzeria.jpg > pizzeria_optimized.jpg
```

pizza.png was optimized/compressed using http://tinypng.com.
 
### Animation
Two main problems were identified :
 * Animation was based on a loop and not using requestAnimationFrame
 * .scrollTop was used in the loop, forcing reflow

Some ideas were borrowed from http://www.html5rocks.com/en/tutorials/speed/animations/

will-change attribute was added to the CSS class (.mover) to let the browser know that this element is changing (style.left)

#### RequestAnimationFrame
To smooth the animation, the original loop that moved the pizzas in the background was replaced with RequestAnimationFrame. This brings a lot of smoothness to the animation, removes jank and reaches constantly 60 FPS using the FPS Counter/HUD Display.

#### Layout trashing
Or it could also be replaced by items[i].style.transform = "translate(" + tr + "px, 0px)"; The are some battles around the webs about which is faster : style.left or CSS translate. I didn't find a clear, settled, answer. BTW, if we are using transform, we have to change the will-change CSS attribute for class .mover to transform.
 
### Resizing the pizzas
Resizing the pizzas now take 3.97 ms (at least on my older laptop). The problem here was Forced Synchronous Layout: the layout is called (offsetWidth) before the style is updated (style.width), and this is done repeatedly inside the loop. I also changed the way the width of the randomPizzaContainer class is changed : using fixed settings avoids the repeated layout calling (offsetWidth) to determine dx.

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>

### Sample Portfolios

Feeling uninspired by the portfolio? Here's a list of cool portfolios I found after a few minutes of Googling.

* <a href="http://www.reddit.com/r/webdev/comments/280qkr/would_anybody_like_to_post_their_portfolio_site/">A great discussion about portfolios on reddit</a>
* <a href="http://ianlunn.co.uk/">http://ianlunn.co.uk/</a>
* <a href="http://www.adhamdannaway.com/portfolio">http://www.adhamdannaway.com/portfolio</a>
* <a href="http://www.timboelaars.nl/">http://www.timboelaars.nl/</a>
* <a href="http://futoryan.prosite.com/">http://futoryan.prosite.com/</a>
* <a href="http://playonpixels.prosite.com/21591/projects">http://playonpixels.prosite.com/21591/projects</a>
* <a href="http://colintrenter.prosite.com/">http://colintrenter.prosite.com/</a>
* <a href="http://calebmorris.prosite.com/">http://calebmorris.prosite.com/</a>
* <a href="http://www.cullywright.com/">http://www.cullywright.com/</a>
* <a href="http://yourjustlucky.com/">http://yourjustlucky.com/</a>
* <a href="http://nicoledominguez.com/portfolio/">http://nicoledominguez.com/portfolio/</a>
* <a href="http://www.roxannecook.com/">http://www.roxannecook.com/</a>
* <a href="http://www.84colors.com/portfolio.html">http://www.84colors.com/portfolio.html</a>
