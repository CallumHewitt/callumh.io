---
title: Express API - Project Setup
subtitle: Setting up a new TypeScript Express project
category: blog
date: 2023-09-25
permalink : blog/express-api-project-setup/
cover_image: 'foundation_cover.jpg'
header_image: 'foundation_post.jpg'
header_image_alt_text: A photo of a building site's foundations. Photo by Scott Blake on Unsplash.
tags:
show_date: true
---

I want to create an API for my website, for a few different reasons.

1. I want to better understand Express, especially using it in combination with TypeScript.
2. I want to experiment with different cloud deployment tools.

This write up is primarily about how I set the project up, and is going to be a basic step-by-step guide so that I can refer back to it later. Note that I'm mostly following [Colt Steele's "How To Use TypeScript With Express & Node"](https://www.youtube.com/watch?v=qy8PxD3alWw) tutorial for this.

## Express

To start with I set up a basic JavaScript Express app. I configured my project, installed the Express library and created an `index.js` file.

```shell
npm init
npm install express
touch index.js
```

The code below configures an Express application listening on port 8000. A request sent to the root endpoint will return the string 'Hello from Express!'. I also added a callback to log the port number once the application starts listening.

```js
const express = require('express');
const port = 8000;

const app = express();

app.get('/', (req, res) => {
    res.send('Hello from Express!');
});

app.listen(port, () => {
    console.log(`Now listening on port ${port}`);
});
```

This app can be kicked off and run locally using `node index.js`.

![A screenshot showing the response from the Express application as 'Hello from Express!' on localhost:8000](project-setup.png)

## TypeScript

To use TypeScript I needed to:

1. Re-write the `index.js` file as a TypeScript file
2. Transpile the TypeScript files into JavaScript
3. Serve those JavaScript files on localhost.

I installed the TypeScript library and also the type libraries for Express and Node. This gives type information for the Express objects preventing type errors and giving me useful hints in my editor.

```shell
npm install -D typescript @types/express @types/node
```

![A screenshot showing the type hints for Express](installing-typescript-1.png)

With these libraries installed I renamed `index.js` to `index.ts` to turn it into a Typescript file, and I also switched from using `require()` imports to using the `import` keyword instead.

```ts
// Previously
const express = require('express');
// Now
import express from 'express';
```

The TypeScript code is ready, but now I need to transpile it into JavaScript.

Firstly I needed a `tsconfig.json` file. This defines the settings for the TypeScript transpiler as well as other language options. The command below will create a basic file to start with.

```shell
npx tsc --init
```

The only property I changed was the `outDir` property. I moved the output to `./dist` so that the JavaScript files aren't written into the root project directory.

With this done I ran `npx tsc` to transpile the files, and the output was generated into the `dist/` folder.

![A screenshot showing the folder structure. index.js exists under the dist directory, while index.ts exists at the root](installing-typescript-2.png)

This file can now be run using `node dist/index.js`.

## Development Scripts

It would also be great if I could set up the development environment to automatically transpile TypeScript files into JavaScript files when they change, and have the locally running application automatically refresh these changes so that I don't have to restart the app.

To do this I installed `nodemon` and `concurrently`.

```shell
npm install -D nodemon concurrently
```

Then I added some scripts into `package.json`.

```json
"scripts": {
  "build": "npx tsc",
  "start": "node dist/index.js",
  "serve": "concurrently \"npx tsc -w\" \"nodemon dist/index.js\""
}
```

The `build` and `start` commands are the same as the commands I ran manually above. `serve` is new, and does two things in parallel. The first part `npx tsc -w` starts the TypeScript transpiler in watch mode. The transpiler watches for changes in the project's `.ts` files and re-transpiles them into `dist`. The second part `nodemon dist/index.js`, runs in parallel to the transpiler. It watches the `index.js` file for changes, and restarts the localhost server after it updates.

Now if I run `npm run serve`, I can make changes locally and they will automatically be reflected at `localhost:8000`.

I also installed `rimraf`, which is a tool for deleting files, to clear out the dist folder as part of the build, and before `start` and `serve` run. `prestart` and `preserve` automatically run before `start` and `serve` when using `npm run`.

```json
"scripts": {
  "build": "rimraf dist && npx tsc",
  "prestart": "npm run build",
  "start": "node dist/index.js",
  "preserve": "npm run build",
  "serve": "concurrently \"npx tsc -w\" \"nodemon dist/index.js\""
}
```

This completes the project set-up. The next step from here is to get the API deployed.
