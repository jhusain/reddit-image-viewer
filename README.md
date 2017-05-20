# Reddit Image Viewer

In this exercise we'll create an image viewer for Reddit subs.

![screenshot](https://jhusain.github.io/reddit-image-viewer/screenshot.png)

A user can select from several image-heavy subs using the select box. Each time a new sub is selected your app must download the first 200 images from that sub using the `getSubImages` function. 

```js
getSubImages("pics").subscribe(images => console.log(JSON.stringify(images, null, 2));

// prints the following to the console:
// [
//   "https://upload.wikimedia.org/wikipedia/commons/3/36/Hopetoun_falls.jpg",
//   "https://upload.wikimedia.org/wikipedia/commons/3/38/4-Nature-Wallpapers-2014-1_ukaavUI.jpg",
//   ...
// ]

```

Once the posts have been loaded, the user can navigate through the images using the next and back buttons. When the user navigates to a new post, the image is displayed only once it has been successfully preloaded. Images can be preloaded like so:

```js
const loaderImage = new Image(url);
loaderImage.onerror = function() {
  // image failed to load
}
loaderImage.onload = function() {
  // image loaded succesfully
}
```

If the image load is not successful, the following error image is displayed (https://jhusain.github.io/reddit-image-viewer/error.png) instead of the image.

![screenshot](https://jhusain.github.io/reddit-image-viewer/error.png)

This app may appear simple, but naive implementations could suffer from any of the following race conditions:

* In the event requests for a subs posts complete out of order, images from old subs may be displayed after images from subs selected by the user more recently.
* In the event image preloads complete out of order, old images may be displayed after images selected by the user more recently.
* While a new sub is being loaded, the UI may continue responding to the navigation events for the current sub. Consequently images from the old sub may be displayed briefly before abruptly being replaced by those in the newly-loaded sub.

You must use the correct Observable flattening methods to avoid hitting these race conditions.
