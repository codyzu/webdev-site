# Final Hackathon

[Steganography](https://en.wikipedia.org/wiki/Steganography) [Stéganographie](https://fr.wikipedia.org/wiki/St%C3%A9ganographie)

> the practice of concealing a file, message, image, or video within another file, message, image, or video - wikipedia

You will need these 2 packages:
* [lsb](https://github.com/hughsk/lsb)
* [jimp](https://github.com/oliver-moran/jimp/tree/master/packages/jimp)

```cmd
yarn add lsb jimp
```

There are 5 images with data encoded in them.

The data has the following format:
```javascript
{
  index: 0,
  message: 'a secret message...',
}
```

Using these images:

* [a.png](images/a.png)
* [b.png](images/b.png)
* [c.png](images/c.png)
* [d.png](images/d.png)
* [e.png](images/e.png)

Use the following `decode()` function to decode the embedded data.

```javascript
const jimp = require('jimp');
const lsb = require('lsb');

function decode(inputImage) {
  const image = await jimp.read(inputImage);
  const json = lsb.decode(image.bitmap.data, rgb);
  return JSON.parse(json);
}

function rgb(n) {
  return n + Math.floor(n / 3);
}
```

Deploy a page that **dynamically**:

1. orders the images based on the value of `index` field encoded in the image
1. displays the secret message by concatenating the message segments in the order determined from their `index`
1. deploy your site to firebase
1. consider using 1 (or more) of the following:
   * a cloud function that renders a pug template or an html string
   * a react app (import the 5 image files into your application)