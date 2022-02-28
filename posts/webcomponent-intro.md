---
title: Getting Started with open-wc and Web Components
description: Learning how to design web components under the open-wc standard
date: 2022-02-28
tags:
  - javascript
  - webdev
  - webcomponents
  - tutorial
layout: layouts/post.njk
---

*One month ago, I hadn't written a single line of JavaScript.*

I knew that JavaScript was important, but I seemed to be scared away by the massive ecosystem. As I looked deeper into the powerful things that you can do with JavaScript, I knew I had to get my feet wet. If you're looking to get started with open-wc and Web Components, you're going to need to understand the fundamentals of plain vanilla JavaScript. That might sound daunting, but **getting started with Javascript is easier than you think.** Today I'll show you why.

## JavaScript Basics

Before starting my journey with JavaScript I had intermediate programming experience. Most of my academic career I have used Java, and I've written my personal projects in python. If you've never written *any* code, thats ok. We all start somewhere. Most language fundamental tutorials are beginner friendly.

From a JavaScript newbie, **I recommend the [JavaScript Essential Training LinkedIn Learning Course](https://www.linkedin.com/learning-login/share?account=76811570&forceAccount=false&redirect=https%3A%2F%2Fwww.linkedin.com%2Flearning%2Fjavascript-essential-training%3Ftrk%3Dshare_ent_url%26shareId%3DtagQlFJZTBmp1uXvG335Dw%253D%253D).** This course helped me to get an understanding od the language as a whole while also understanding the JavaScript ecosystem. I'm about 30% of the way through it, and plan on banging out some more after I finish this article.

One more thing- one of the biggest surprises for me is that **JavaScript's native runtime is in the browser** (Like Google Chrome). It took some getting used to, as I was more familiar with working completely in the command line. It is intuitive though, as a majority of JavaScript use cases involve the web in some capacity.

## Preparing the JavaScript Environment

Unlike many languages (like Python, Java, and Go), you don't need to download a language interpreter to your machine. In fact, you run JavaScript code every time you visit *most* websites. So, if you have a modern web browser installed, then you're golden.

### VS Code

I use Visual Studio Code to write my JavaScript Code. It is very lightweight bare bones out of the box, but it has a rich extension ecosystem that you can use to increase its functionality. 

Head over to [code.visualstudio.com](https://code.visualstudio.com/) to install VS Code. Click on the big blue installation button to download the installer and follow the prompts when running it.

One key extension that you will need when writing and testing vanilla JS is "Live Server". This allows users to start a local web server for their current VS Code project with just one click.

### Node.js

The next step is to download Node.JS. You want to get the current LTS (Long Term Support) version for increased stability. To install, head to [nodejs.org](https://nodejs.org/) and click the nice big green "LTS" button. The site should recognize the OS that you are working with and give you the right installer.

Node.JS enables you to run server-side JavaScript applications on your machine. Some people think that Node.JS is a JavaScript library or a framework or its own separate language, but **Node.JS is none of those. Node.JS is a runtime for JavaScript.**

After running the installer, check that Node.js was installed properly with `node -v`

![node -v output](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/njdfut6p7hnara15c1is.png)

### npm

Node comes with a package manager for JavaScript called [npm](https://npmjs.com/). npm enables you to use other people's code in your projects without needing to go and write it yourself. Users can find npm packages on the npm registry, then use them in their code by using the Node require() function and defining them in their projects' `package.json` file, creating a dependency. All of this can sound confusing at first, but for now, just know that you can piggyback off of other projects with npm.

To verify that npm was installed correctly when you installed Node.js, you can run `npm -v`

![npm -v output](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4vxheujhvs0mzp23ec19.png)

### Yarn

[Yarn](https://yarnpkg.com) is another package manager for JavaScript. It is very similar to npm as it enables users to reuse code from other developers by helping them register dependencies in their project. It registers dependencies to the `package.json` just like npm.

With newer versions of Node, yarn comes pre-installed and can be installed without much hassle. Simply run `corepack enable`.

After doing this, you can verify that yarn is installed properly with `yarn -v`.

![varn -v output](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/kvilukqczym3hsfmtwlo.png)

## Initializing an open-wc starter project

Now that the basic software is installed and running, a new open-wc component can be created. This process is also relatively simple. First, create a new directory where your project will live and navigate to it. Then, run `npm init @open-wc` If the command runs successfully, that means that you've installed everything correctly and you are beginning to work on your first web component!

![Shows expected output of the initialization for open-wc](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/aauwov28d7m9coqn8kz5.png)

This is the screen that you will see with instructions to set up the project.

I've initialized my starter project with the following settings:

- New project scaffold
- Web Component
- Linting, Testing, and Demoing enabled
- No TypeScript
- installed using yarn

Once these settings are selected, a browser window should open and connect to the Node web server that is serving the project. If that does not happen automatically, or you want to start up the server at a later date, simply run `npm start`. Boom! Now you can take a look at how web components work on the web.

## Analyzing a Web Component

Web Components, even in their most simple form have numerous files that provide functionality. Each of these files has a unique purpose that enables web components to function as the easy to use, reusable HTML elements.

First, lets look at the `index.html` of this project, the file that the browser loads when this project is opened.

```html
<body>
  <div id="demo"></div>

  <script type="module">
    import { html, render } from 'lit';
    import '../hello-world.js';

    const title = 'Hello World!';
    // takes elements with id=demo and replaces with hello-world element
    render(
      html`
        <hello-world .title=${title}>
          some light-dom
        </hello-world>
      `,
      document.querySelector('#demo')
    );
  </script>
</body>
</html>
```

In this file, there is a div with id=demo, that is then replaced by the `<hello-world>` element when the script is loaded. The `<hello-world>` element is hydrated with content referenced in `hello-world.js`, which is imported to the script.

```javascript
// imports the HelloWorld class from the source files
import { HelloWorld } from './src/HelloWorld.js';

// defines the "<hello-world>" HTML tag from the HelloWorld class in the imported module
window.customElements.define('hello-world', HelloWorld);
```

`hello-world.js` defines the `<hello-world>` HTML tag with the HelloWorld Web Component.

The meat of the element is found in `./src/HelloWorld.js`. This component defines the functions and properties of the HelloWorld web component, represented as a class that extends the base HelloWorld class. For example, one of the functions called `__increment()` increments the counter property of the HelloWorld object every time a button in the component is pressed.

Many of the other files that come with the base "hello-world" web component serve important purposes also. I've annotated many of the files found in this project and [uploaded them to a GitHub repository that can be found here.](https://github.com/mayormaier/edtechjoker-lab1/)
