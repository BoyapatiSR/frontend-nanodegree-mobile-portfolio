#Website Optimization

######Project Overview

You will optimize a provided website with a number of optimization- and performance-related issues so that it achieves a target PageSpeed score and runs at 60 frames per second.
##Getting Started

######Live

Project link  https://boyapatisr.github.io/frontend-nanodegree-mobile-portfolio/

######Locally

**1.** Clone this repo:

```
$ git clone https://github.com/BoyapatiSR/frontend-nanodegree-mobile-portfolio.git
````

######Tools used:

```
ImageMagic : ImageMagic installation instructions can been found [here](http://www.imagemagick.org/script/binary-releases.php).
```

##PageSpeed Score Optimization

###Index Page

The index page originally had a Google PageSpeed score of 35/100 for mobile and 47/100 for desktop. After making changes the score increased to [94/100](https://developers.google.com/speed/pagespeed/insights/?url=https%3A%2F%2Fboyapatisr.github.io%2Ffrontend-nanodegree-mobile-portfolio%2F) for  mobile and [93/100] to desktop. Other suggestions for optimization is Leverage browser caching.caching can be done at server side and github does not allow  to change the settings.


######Resource used:

```
- [HTTP Caching](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching) by google
```

```
- [Page speed optimization](https://varvy.com/pagespeed/) by varvy
```


The following changes were made:

######CSS

[Inlined](https://developers.google.com/speed/pagespeed/module/filter-css-inline) all of the styple(except media query portion) CSS into the head of the document and added the HTML [media="print"](https://developer.mozilla.org/de/docs/Web/HTML/Element/link) attribute to the external style sheet link for print styles.

Created separate file named portrait.css and used [media="orientation:portrait"] to stop CCS blocking the rendering.


######JS

Added the [HTML async attribute](https://developer.mozilla.org/en-US/docs/Games/Techniques/Async_scripts) to all script tags.

Replaced ( href="//fonts.googleapis.com/css?family=Open+Sans:400,700" rel="stylesheet) with inline CCD

```js
   WebFontConfig = {
            google: {
                families: ['Open+Sans:400,700']
            }
        };
        (function() {
            var wf = document.createElement('script');
            wf.src = ('https:' == document.location.protocol ? 'https' : 'http') +
                '://ajax.googleapis.com/ajax/libs/webfont/1.5.18/webfont.js';
            wf.type = 'text/javascript';
            wf.async = 'true';
            var s = document.getElementsByTagName('script')[0];
            s.parentNode.insertBefore(wf, s);
        })();

```


######Images

Resized images that were too large and compressed all images with the imageMagic  tool.

- [mageMagic](http://www.imagemagick.org/Usage/) from imagemagick

###### compression

**1.** Commands to optimize the pizzeria image:

```
convert pizzeria.jpg -quality 10 -strip pizzeria-min.jpg
```

**2.** Commands to optimize the pizzeria 100X100 image:

```
convert -strip -resize 100x100 -quality 90 pizza.png pizza.png
```

**3.** Commands to optimize the profile image:

```
convert profilepic.jpg -quality 10% -strip profilepic.jpg
```


##Computational Efficiency

The following changes where made to fix the low FPS and produce a consistent 60FPS frame rate when scrolling the page and time to resize pizzas should be less than 5 ms using the pizza size slider on the pizza.html page


######resize pizzas

```
1.Replaced querySelectorAll with getElementsByClassName to improve performance
2.Optimized for loop by moving dom reference and length calc outside for loop
3.Consolidated the functions determineDx and sizeSwitcher into changePizzaSizes function by removing unnecessary logic
```

```
Changes starts at line number 454(main.js)
```

######Frame Rate

```
Changed the image name to pizzeria-mini.jpg in Pizza.html at line number 41 to use compressed image.
```

######addEventListener function

Optimized the loops contained in the updatePositions function and the onDOMContentLoaded event handler.

#######Changed starts at line(main.js): 580

```
1.Moved dom access outside function and for loop
2.Replaced querySelector with getElementById
3.Changed pizza count from 200 to 30
4.Reduced number of calculation by using variable left
5.Replace .class with classList
6.Set left property
7.Cached background pizzas to variable to avoid Forced Synchronous Layouts
8.Wrapped updatePositions call in requestAnimationFrame to optimize the performance of animation.
```

######addEventListener function

Applied translateX()  transform functions to the sliding pizza elements within the updatePositions function.

#######Changed starts at line(main.js): 524

```
1.Created global variables to cache background pizzas and number of background pizzas
2.Moved document access outside for loop  (get the current y coordinate)
3.Moved window.performance.mark after document access to avoid nested dom read and writes.
4.Simplified phase calculation as it always returns only 5 values and moved out of main forloop to improve FPS.
5.Moved the calculation which utilizes the scrollTop method outside of the loop.
6.Replaced style.left with translateX() to optimize the performance of animation.
```

######Optimized Animations

Used requestAnimationFrame to call updatePositions on scroll

```js
window.addEventListener('scroll', function() {
    window.requestAnimationFrame(updatePositions);
});
```

######Changes to mover class in style.css

Added will-change: transform to indicate browser that pizza elements will change hence reduces unnecessary style recalculations.

```css
.mover {
  position: fixed;
  width: 256px;
  z-index: -1;
  will-change: transform;
  transform: translateZ(0);
  backface-visibility: hidden;
}
```

##Resources


####Index Page


######Critical Fold CSS

- [CSS-Tricks: Authoring Critical Above-the-Fold CSS](http://css-tricks.com/authoring-critical-fold-css/)

######Page speed optimization
- [Page speed optimization](https://varvy.com/pagespeed/) by varvy

######Owning your performance
- [Owning your performance](https://www.youtube.com/watch?v=w0O2znkSBXA) RAIL (Chrome Dev Summit 2015)

######Simulate Mobile Devices
- [Simulate Mobile Devices](https://developers.google.com/web/tools/chrome-devtools/device-mode/?utm_source=dcc&utm_medium=redirect&utm_campaign=2016q3) by google
######CSS Optimization

- [Google Developers: Optimize CSS Delivery](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery#example)
- [Gift of Speed: Inline CSS](http://www.giftofspeed.com/inline-your-css-code/)
- [Critical Path CSS Generator](http://jonassebastianohlsson.com/criticalpathcssgenerator/) By Jonas Ohlsson
- [Loading CSS Asynchronously](http://deanhume.com/home/blogpost/loading-css-asynchronously/7104) By Dean Hume

####Pizza Page

######Layout & Rendering

- [Jank Free](http://jankfree.org/)
- [Solving rendering performance puzzles](https://jakearchibald.com/2013/solving-rendering-perf-puzzles//) By Jake
- [What forces layout / reflow](https://gist.github.com/paulirish/5d52fb081b3570c81e3a#getcomputedstyle) from GitHub
- [[VIDEO] Gone in 60fps - Making A Site Jank-Free](http://addyosmani.com/blog/making-a-site-jank-free/) By Addy Osmani


######CSS Transform

- [Treehouse: Increase Your Site’s Performance with Hardware-Accelerated CSS](http://blog.teamtreehouse.com/increase-your-sites-performance-with-hardware-accelerated-css)
- [css-transforms](https://www.w3.org/TR/css-transforms-1/#transform-property) By David Walsh
- [Why Moving Elements With Translate Is Better Than Pos:abs Top/left](https://www.paulirish.com/2012/why-moving-elements-with-translate-is-better-than-posabs-topleft/) By Paul Irish
- [Fixing a parallax scrolling website to run in 60 FPS](http://kristerkari.github.io/adventures-in-webkit-land/blog/2013/08/30/fixing-a-parallax-scrolling-website-to-run-in-60-fps/)

######Animations

- [HTML5 Rocks: High Performance Animations](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/) By Paul Lewis and Paul Irish

######requestAnimationFrame

- [Fixing a parallax scrolling website to run in 60 FPS](http://kristerkari.github.io/adventures-in-webkit-land/blog/2013/08/30/fixing-a-parallax-scrolling-website-to-run-in-60-fps/) By Krister Kari
- [Mozilla Developer Network: window.requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window.requestAnimationFrame)

######Chrome Devtools

- [Cheatsheet](http://anti-code.com/devtools-cheatsheet/)
- [Performance profiling with the Timeline](https://developer.chrome.com/devtools/docs/timeline)

######Chrome Office Hours

- [[VIDEO] Paul Lewis & Paul Irish investigate rendering problems](https://www.youtube.com/watch?v=z0_jD8nO5Zw)

######Udacity Office Hours

- [[VIDEO] Office Hours: P3 and P4, Strategies for Web Optimization](https://plus.google.com/events/cjk2bief153ofdink5eln6nv8f8)