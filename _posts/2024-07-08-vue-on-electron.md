---
title: How to use Vue + Vite on Electron
description: This is a simple tutorial on how to use Vue + Vite on Electron.
---
Hello, developer!
Interested in making an application with [Electron][electron], and using [Vue][vue] in it instead of pure HTML?
Come with me and I'll help you out!

![Vue + Vite on Electron](https://telegra.ph/file/d5ecee3913b9896607914.jpg)
_Vue + Vite on Electron_

[Electron][electron] is one of the most popular frameworks for building cross-platform desktop apps with JavaScript. So many popular apps are using [Electron][electron]. A special example is Visual Studio Code.

There are some tutorials on the same subject here on the internet, but they are out of date and don't even fix most of the problems caused by old and outdated examples.
I'm here to bring you a definitive solution (until things change again) and help anyone who wants to make their own app in [Electron][electron] and [Vue][vue].

<hr>

## Introduction
First of all, make sure you have [Node.js][njs] installed.

You can use the ``node --version`` and ``npm --version`` commands to check if it's really installed.

``` sh
$ npm --version
10.8.1
$ node --version
v22.3.0
```

If not, you can visit its website ([nodejs.org][njs]) to download and install it.

<hr>

## Creating the Vite project

First, let’s make our [Vite][vite] app. If you want to learn more about creating your [Vue][vue] project in [Vite][vite], there is a plenty of tutorials on [YouTube][youtube].
   
But basically, let’s go to our terminal and run the commands below (``[project-name]`` is the name of the project that you have created):

``` sh
$ npm create vite@latest
$ cd [project-name]
$ npm install
```

Now, you can test your project on the browser via the good old ``npm run dev`` command.

This is the [Vite][vite] template on the browser:

![Vue + Vite on Browser](https://telegra.ph/file/fc73116d5380309c1b738.jpg)
_Vue + Vite on Browser_

<hr>

## Creating and the Electron app
For now, we will be following the [Electron’s own quick start guide from the official documentation][electron-docs], but tweaking it a little bit to work inside our [Vite][vite] project.

The first thing we have to do is actually install [Electron][electron], so let’s head over to the terminal and do that.

``` sh
$ npm install --save-dev electron
```

The [Electron guide][electron-docs] says that a simple [Electron][electron] app needs four main files, but here we will only use two of them, as one is already been created by [Vite][vite] and the other is unnecessary for the project.

The files that we need here are:
- `package.json` (we already have that)
- `index.html` (we already have that too)
- `main.js`

<hr>

## Setting up our Vue + Vite + Electron app
The next step is to create our main.js file in our root directory.
Once it’s created, we can just copy and paste the code from the [Electron][electron] quick start guide, but there is one change that we have to make though.

On the part that "says" to [Electron][electron] where to load our ``index.html`` file, we will have to change that path to ``./dist/index.html``, so we are using the file that [Vite][vite] builded inside our ``dist`` folder.
Also, we can remove the part that loads the ``preload.js`` file, as for now we won't need of that.

The final code of ``main.js`` will be something like that:

``` javascript
const { app, BrowserWindow } = require("electron");
const path = require("path");

function createWindow() {
  const win = new BrowserWindow({
    width: 800,
    height: 600,
  });

  win.loadFile("./dist/index.html");
}

app.whenReady().then(() => {
  createWindow();

  app.on("activate", () => {
    if (BrowserWindow.getAllWindows().length === 0) {
      createWindow();
    }
  });
});

app.on("window-all-closed", () => {
  if (process.platform !== "darwin") {
    app.quit();
  }
});
```
{: file="main.js" }

Now, we will need to modify our ``package.json`` file, removing the ``"type": "module"`` part of the file, also adding ``"description"`` and ``"author"`` sections, as this will be needed when building.

Also, you will need to add some things in the ``"scripts"`` section to make the commands to load our [Electron][electron] project.

``` json
{
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "start": "electron .",
    "test": "vite build && electron ."
  },
}
```
{: file="package.json (snippet)" }

Here is my personal tip: you can run ``npm test`` to compile the [Vite][vite] app and run [Electron][electron] with the files provided by [Vite][vite] at the same time.

Your final ``package.json`` will look like this:

``` json
{
  "name": "vite-electron",
  "private": true,
  "version": "0.0.0",
  "description": "The example code for a tutorial on my blog",
  "main": "main.js",
  "author": "Lucas Gabriel (lucmsilva)",
  "license": "BSD-3-Clause",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview",
    "start": "electron .",
    "test": "vite build && electron ."
  },
  "dependencies": {
    "vue": "^3.4.29"
  },
  "devDependencies": {
    "@vitejs/plugin-vue": "^5.0.5",
    "electron": "^31.1.0",
    "vite": "^5.3.1"
  }
}
```
{: file="package.json" }

> Note: I have modified my license on the example JSON because I prefer to use [BSD-3][bsd-3-license] instead of [MIT][mit-license]. But you can stick with [MIT][mit-license], as you may have different license requirements than me.

Finally, we will need to modify the ``vite.config.js`` (or ``vite.config.ts`` if you are using TypeScript) a little bit to specify the build directory to ``/dist`` and make the exported HTML file use relative paths instead of fixed ones.

There is the ``vite.config.js`` file with all changes already made, just copy, paste and enjoy!

``` javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  base: '',
  plugins: [vue()],
  build: {
    outDir: 'dist'
  }
})
```
{: file="vite.config.js" }

<hr>

## Testing the app
Now, you can test the app using ``vite build`` to make the files from [Vite][vite] and ``npm start`` to run the [Electron][electron] app. It should work like magic.

![Vue + Vite on Electron](https://telegra.ph/file/d5ecee3913b9896607914.jpg)
_Vue + Vite on Electron_

> Note: to make the Vue logo appear on the project, you will need to move the ``vue.svg`` file located in ``/src/assets`` to ``/public`` and change the ``App.vue`` to point the SVG to ``/vue.svg`` instead of ``./assets/vue.svg``.

<hr>

## Outro
And that's it! I hope that this tutorial helped you. You can see the final files on [my GitHub][my-github].

Also, here are the repository link with the final files: [https://github.com/lucmsilva651/vite-electron][repo-link]

[njs]: https://www.nodejs.org
[vue]: https://www.vuejs.org
[vite]: https://www.vitejs.dev
[electron]: https://www.electronjs.org
[youtube]: https://www.youtube.com
[electron-docs]: https://www.electronjs.org/docs/tutorial/quick-start
[bsd-3-license]: https://opensource.org/license/BSD-3-clause
[mit-license]: https://opensource.org/license/MIT
[my-github]: https://github.com/lucmsilva651
[repo-link]: https://github.com/lucmsilva651/vite-electron