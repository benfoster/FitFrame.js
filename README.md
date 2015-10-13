FitFrame.js
===========

A lightweight, easy-to-use jQuery plugin for responsive iframes.


## Setup

FitFrame.js requires jQuery. It has only been tested with jQuery 1.11 so if you have problems with any other versions let me know.

You can install via bower

    bower install fitframe --save

If you're using `mode: wrap` (the default) you'll want to reference `fitframe.css`:

	<link rel="stylesheet" href="fitframe.css" />
	
And of course you'll need to reference jQuery and FitFrame.js:

	<script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.0/jquery.min.js"></script>
	<script src="fitframe.js"></script>
	
## Usage (wrap mode)

The default mode of FitFrame.js is `wrap` and uses [intrinsic ratios](http://alistapart.com/article/creating-intrinsic-ratios-for-video) to make the iframes responsive.

By wrapping the `iframe` element in a `div` with `height: 0` and `padding: X` (where X is the ratio between the iframe height and width) we can create a responsive iframe that maintains aspect ratio as its container resizes. So for a 16:9 video the container elements gets a height of 56.25% of it's width `((9/16) * 100)`. Clever stuff I know!

This is the recommended mode since no JavaScript is actually executed when the viewport size changes.

To initialize FitFrame.js just call `fitFrame()` on your container element:

	$('.container').fitFrame();

### Adding elements at runtime

If you're adding DOM elements to your container at runtime you have two options.

1. Just call the `update` function to have FitFrame.js search for any new iframes that have been added and make them responsive:

		$('.container').fitFrame('update');
	
2. If you want to wrap the iframe before you add it to the DOM you can use the `wrap` function, passing the iframe object to wrap:

		var wrapped = $('.container').fitFrame('wrap', $(iframeHtml));      
		$('.add-embed').before(wrapped);


You can see an example of this in [demos/demo-wrap.html](demos/demo-wrap.html) that uses [embed.ly](http://embed.ly/) to dynamically load videos using a URL. Note that you'll need to set your embedly API key in the code.

## Usage (resize mode)       

The `resize` mode binds to the `window` resize event and sets the size of the iframe element to fit the container. No wrapper element is used and you don't need to add any additional CSS.

If you just want to fit your iframes to the width of the container then you are best off using the `wrap` mode. That said, with the use of CSS transitions we can create a more interesting experience when resizing the browser. You can see an example of this in [demos/demo-resize-width.html](demos/demo-resize-width.html). To use `resize` mode:

	$('.container').fitFrame({ mode: 'resize' });

### Adding elements at runtime

Resize mode also supports adding elements at runtime and again you can do these for all new iframes or individually:

1. Resize all new iframes

		$('.container').fitFrame('update');
	
2. Resize an individual element *before* adding it to the DOM:

        var resized = $('.container').fitFrame('resize', $(iframeHtml));      
        $('.add-embed').before(resized);
        

### Fit iframe to container height

This was the original reason for creating the plugin since I needed to make an iframe always fit the height of its container (without any awful black bars!).

To do this, set the `fitHeight` option to true:

	$('.container').fitFrame({
	 mode: 'resize',
	 fitHeight: true
	});
	
You can see an example of this in [demos/demo-resize-height.html](demos/demo-resize-height.html).

## Options

	defaults = {
	  wrapperCssClass: 'fitframe-wrap',
	  iframeCssClass: 'fitframe',
	  mode: 'wrap',
	  fitHeight: false
	};

## Cleanup

Like all good citizens, FitFrame.js will cleanup after itself:

	$('.container').fitFrame('destroy');
	
And can be safely re-initialized if required.