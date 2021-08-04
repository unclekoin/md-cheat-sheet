[&#8678; get back to the start...](../README.md)

# Webpack / Rollup

- [webpack getting started](https://webpack.js.org/guides/getting-started/)
- [rollup.js](https://rollupjs.org/guide/en/)

## Node.js® & NPM

- [node.js](https://nodejs.org/en/)
- [nvm / node version manager](https://github.com/nvm-sh/nvm)
- [npm](https://www.npmjs.com/)

## Rollup

- [rollup](https://rollupjs.org/guide/en/)

```bash
> npm init -y
```

```bash
> npm install --global rollup
```

Create rollup.config.js

```js
// rollup.config.js

export default {
  input: './src/index.js',
  output: {
    file: './build/bundle.js',
    format: 'cjs',
  },
};
```

Let's get started

```bash
> rollup -c
```

```html
<!-- index.html -->

<script src="./build/bundle.js"></script>
```

```bash
> npm install --save-dev @rollup/plugin-babel
> npm install --save-dev rollup-plugin-styles
> npm install --save-dev rollup-plugin-img
> npm install --save-dev rollup-plugin-serve
> npm install --save-dev rollup-plugin-livereload
```

### Configuration

```bash
.
├── README.md
└── rollup
    ├── assets
    │   └── images
    ├── build
    │   └── bundle.js
    ├── index.css
    ├── index.html
    ├── node_modules
    ├── package-lock.json
    ├── package.json
    ├── rollup.config.js
    └── src
        └── index.js
```

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Rollup</title>
  </head>
  <body>
    <h1>Rollup</h1>

    <script src="./build/bundle.js"></script>
  </body>
</html>
```

```js
// index.js
import '../index.css';
import img from '../assets/images/logo.png';

const logo = document.createElement('img');
logo.className = 'logo';
logo.src = img;
document.body.prepend(logo);
```

```js
// rollup.config.js

import { babel } from '@rollup/plugin-babel';
import styles from 'rollup-plugin-styles';
import image from 'rollup-plugin-img';
import serve from 'rollup-plugin-serve';
import livereload from 'rollup-plugin-livereload';

export default {
  input: './src/index.js',
  output: {
    file: './build/bundle.js',
    format: 'cjs',
  },
  plugins: [
    babel({ babelHelpers: 'bundled' }),
    styles(),
    image({
      limit: 100000,
    }),
    serve({
      open: true,
      contentBase: './',
      port: 8000,
    }),
    livereload(),
  ],
};
```

```js
// packege.json

{
  "name": "rollup",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "rollup -c -w",
    "prod": "rollup -c"
  },
  "keywords": [
    "rollup"
  ],
  "author": "Pavel Koryakin",
  "license": "ISC",
  "devDependencies": {
    "@rollup/plugin-babel": "^5.3.0",
    "rollup-plugin-img": "^1.1.0",
    "rollup-plugin-livereload": "^2.0.5",
    "rollup-plugin-serve": "^1.1.0",
    "rollup-plugin-styles": "^3.14.1"
  }
}
```

Let's get started

```bash
> npm run dev
> npm run prod
```

## Webpack

- [webpack getting started](https://webpack.js.org/guides/getting-started/)

```bash
> npm init -y
> npm install webpack webpack-cli --save-dev
```

Create webpack.config.js

```js
// webpack.config.js

const path = require('path');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.resolve(__dirname, 'build'),
  },
};
```

```js
// package.json

  "scripts": {
    "dev": "webpack"
  },
```

Let's get started

```bash
> npm run dev
```

```bash
> npm install --save-dev html-webpack-plugin
> npm install --save-dev clean-webpack-plugin
> npm install --save-dev babel-loader @babel/core @babel/preset-env webpack
> npm install --save-dev style-loader
> npm install --save-dev css-loader
> npm install --save-dev file-loader
> npm install --save-dev webpack-dev-server
> npm install --save-dev source-map-loader
```

### Configuration

```bash
.
├── assets
│   └── images
│       └── logo.png
├── build
├── index.css
├── index.html
├── node_modules
├── package-lock.json
├── package.json
├── src
│   └── index.js
└── webpack.config.js

910 directories, 2888 files
```

```html
<!-- index.html -->

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Webpack</title>
  </head>
  <body>
    <h1>Webpack</h1>
  </body>
</html>
```

```js
// index.js

import '../index.css';
import img from '../assets/images/logo.png';

console.log('index.js');

const logo = document.createElement('img');
logo.className = 'logo';
logo.src = img;
document.body.prepend(logo);
```

```js
// webpack.config.js

const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    filename: 'build.js',
    path: path.resolve(__dirname, 'build'),
  },
  devServer: {
    contentBase: './build',
    port: 8080,
    open: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './index.html',
    }),
    new CleanWebpackPlugin(),
  ],
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [['@babel/preset-env', { targets: 'defaults' }]],
          },
        },
      },
      {
        test: /\.css$/i,
        use: ['style-loader', 'css-loader'],
      },
      {
        test: /\.(png|jpe?g|gif)$/i,
        use: [
          {
            loader: 'file-loader',
          },
        ],
      },
      {
        test: /\.js$/,
        enforce: 'pre',
        use: ['source-map-loader'],
      },
    ],
  },
};
```

```js
// package.json

{
  "name": "webpack-build",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "webpack serve"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.14.8",
    "@babel/preset-env": "^7.14.8",
    "babel-loader": "^8.2.2",
    "clean-webpack-plugin": "*",
    "css-loader": "^6.2.0",
    "file-loader": "^6.2.0",
    "html-webpack-plugin": "^5.3.2",
    "source-map-loader": "^3.0.0",
    "style-loader": "^3.2.1",
    "webpack": "^5.47.0",
    "webpack-cli": "^4.7.2",
    "webpack-dev-server": "^3.11.2"
  }
}
```

Let's get started

```bash
> npm run dev
```

One more method add image without plugin

```js
// webpack.config.js

module: {
    rules: [
      {
        test: /\.(?:ico|gif|png|jpg|jpeg)$/i,
        type: 'asset/resource',
      },

      {
        test: /\.(woff(2)?|eot|ttf|otf|svg|)$/,
        type: 'asset/inline',
      },
    ],
  },
```
