# Module 2: Express with Templates

*TODO:*
- [] host a final site in firestore
- [] migrate m3 sql course to markdown

## 1 Integrate the pug template system into your express server.

```bash
mkdir my-express-app
cd my-express-app

yarn init
// press enter for all prompts

yarn add express jimp
```

Create the file `app.js`:
```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(port, () => console.log(`Example app listening on port ${port}!`));
```

Test your server with `node .\app.js` and navigate to http://localhost:3000.


Follow the [steps to integrate a templating engine](https://expressjs.com/fr/guide/using-template-engines.html) into your express server.

```bash
yarn add pug
```

Add to your `app.js`:
```javascript
app.set('veiws', './views');
app.set('view engine', 'pug');

// Replace your existing app.get('/', ...) with this new route:
app.get('/', function (req, res) {
  res.render('index', { title: 'Hey', message: 'Hello there!'});
});
```

Create a pug template file in `views/index.pug`:
```pug
html
  head
    title= title
  body
    h1= message
```

Test your new index page by  navigating to http://localhost:3000. Open the chrome dev tools and look at the rendered HTML. Notice that the message and title and passed as variables.

Open the chrome dev tools and look at the source file.

### Question 2.1: What did the line `h1= message` in index.pug render to in HTML?

## 2 Static Files

Follow the [steps to add static files](https://expressjs.com/fr/starter/static-files.html) into your web server.

Add the following configuration into your `app.js`:
```javascript
app.use(express.static('public'));
```

Add some large image files into the directory `public/images`.

Test your static files by navigating to http://localhost:3000/images/abc.jpg

### Exercise 2.1: Add a new pug view at `views/originals.pug` that renders an html document that dynamically includes each image in an `img` tag and is served at http://localhost:3000/originals.

#### Hints:

* pug can iterate over an array of values by using the [`each`](https://pugjs.org/language/iteration.html#each) keyword.
* Using the method we passed the `message` and `title` variables to the template, we can pass an array of image paths.
* You will have to load the image paths using the `fs` module (refer to the code you wrote during the previous course).
* The goal is to render an `<img src="images/..." />` tag for each image in your images directory.

### Exercise 2.2: Add a new route `/thumb/:name` that renders a resized (200x200 max) version of an image using Jimp.

#### Hints:

* Be sure add Jimp to your project with `yarn add jimp`
* Jimp can return a binary buffer (a file in memory) by using the [`image.getBufferAsync()`](https://github.com/oliver-moran/jimp/tree/master/packages/jimp#writing-to-buffers) method.
* Express can send a binary buffer by calling [`res.send(buffer)`](https://expressjs.com/fr/4x/api.html#res.send). However, you will need to set the content type by calling [`res.type()`](https://expressjs.com/fr/4x/api.html#res.type) before sending the buffer. This allows your browser to decide what to do with the binary file, i.e. display the image.
* Both methods jimp `image.getBufferAsync()` and express `res.type()` requires the mime type to be sent. Fortunately, jimp has a method [`image.getMIME()`](https://github.com/oliver-moran/jimp/tree/master/packages/jimp#writing-to-buffers) that 

### Exercise 2.3: Add a new pug view at `views/thumbnails.pug` that renders each image, but with `src` urls pointing to the `/thumb/xyz.jpg` and is served at http://localhost:3000/thumbnails.

#### Hints:

* This exercise is similar to Exercise 2.1, except you will change the `src` attribute in the `<img />` tags to use the `/thumb/:name` route that you added in Exercise 2.2.
