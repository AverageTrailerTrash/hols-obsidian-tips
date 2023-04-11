# Understanding Obsidian's Technologies

The first step to making an Obsidian plugin is understanding what kind of technology stack Obsidian is using, and what that means for best practices in plugin development.

>[!note] Some of this might sound like jibberish, and that's fine. It'll make more sense once you get in the weeds.

## The Basics

Obsidian's desktop version is an **Electron**^[[Electron's Official Site](https://www.electronjs.org/)] app. Electron is a local-first browser framework, which itself is created with **NodeJS**^[[NodeJS's Official Site](https://nodejs.org/en)] and **Chromium**^[[Chromium's Official Site](https://www.chromium.org/chromium-projects/)]. Chromium is an open-source browser & the same framework Chrome is built over. NodeJS provides an engine and various tools for using the **JavaScript**^[[JavaScript's Official Site](http://www.ecmascript.org/)] language to build local applications. 

(Before Node, JavaScript was primarily used in web pages, not local applications. It allowed web developers to provide dynamic content instead of just static text and images, without requiring constant page refreshes.)

Basically, this means that Obsidian is **a single-page web app that runs offline on the user's own device**, instead of on a website. It has its own browser built-in to display its content. Complex stuff in the background is handled with JavaScript via NodeJS, while the layout and styling are handled by web languages like **HTML**^[[HTML's Official Site](https://html.spec.whatwg.org/multipage/)] and **CSS**^[[CSS's Official Site](https://www.w3.org/Style/CSS/Overview.en.html)].

>[!warning] Obsidian's mobile version uses other technologies -- I'm not entirely sure which. But JavaScript that doesn't directly rely on Node modules (like `fs`) will run fine on it, as will HTML and CSS.

You'll be able to specify if your plugin is exclusively for desktop users.

## TypeScript vs JavaScript

Although Obsidian ultimately runs with JavaScript, it wasn't actually developed with JavaScript, and generally doesn't encourage using JavaScript in plugin development.

Instead, **TypeScript**^[[TypeScript's Official Site](https://www.typescriptlang.org/)] is used.

TypeScript is a "superset" of JavaScript. Meaning JavaScript is valid in TypeScript. But TypeScript provides lots of helpful optional features to make your JavaScript less prone to errors.

For example, imagine that you have an "addNumbers" function that requires an array of numbers. In JavaScript, you would have to add lots of manual if/then try/throw checks before running the function to make sure the parameters passed to it are actually numbers instead of letters or a boolean etc. 

With TypeScript, you can just clarify that the parameter must be a "number[]" type:

```ts
function addNumbers(givenNumbers:number[]): number{
	let subtotal:number = 0;
	for (const currentNumber of givenNumbers) {
		subtotal += currentNumber;
	}
	return subtotal;
}
```

Then, whenever you use that function elsewhere in your codebase, you get a big red squiggly error if you try to pass it anything that might not be a number. 

This makes it much, much easier to avoid subtle type errors that may otherwise not be found until some major bug pops up months later and are a nightmare to track down.

**But how are we able to use JavaScript technologies like Node or Electron with this language?** 

A TypeScript project, when complete or ready for testing, is compiled into JavaScript. 

So the final code being pushed to the end user is always JavaScript.

That means you can technically skip the TypeScript altogether and just write your plugin in JavaScript. 

However, nearly all of the documentation and community content surrounding plugin development assumes that you are using TypeScript, and trying to translate all of that to another language -- especially given the extra features TypeScript provides that don't have easy 1:1 equivalents in JavaScript -- can be a real pain.

So I recommend working in TypeScript when developing Obsidian plugins. (And when doing anything JS-related. Turns out, it's just a great language.)

TypeScript can be a little tricky at first if you aren't familiar with OOP principles like contracts/interfaces and objects/classes. But once you figure it out, it's really a breeze to use. It's free from both the vagueness of JavaScript and the strictness of Java.

>[!warning] It's important to use a modern IDE when working with TypeScript, as it's a new language that calls for lots of intellisense. I highly recommend using **VSCode**.

## To Node, or Not To Node

Just because Obsidian uses Node, that doesn't mean you have to.

There's no technical reason why you'd need to use Node to make an Obsidian project -- you could make your own TypeScript compiler, and make every functionality you need from scratch. You could make this an absolutely massive insurmountable project. Sure.

However, you will probably want to use Node and various NPM packages, for your sanity.

Obsidian used it for a reason, and you will too.

## Node Package Manager

With that in mind, most plugin development is done through **NPM**^[[NPM's Official Site](https://www.npmjs.com/)], i.e. the **Node Package Manager**. This service comes with your NodeJS installation, meaning installing NodeJS lets you use both `node` and `npm` commands in the terminal, and it has a significant impact on how Node projects operate.

>[!note] Neither NodeJS nor NPM has a user interface. Everything is done through the terminal with text. *See: [[Understanding the Terminal]]*

NPM is essentially a code distributor that makes it very easy to share chunks of code with other developers. It boasts a massive collection of libraries and frameworks that anyone can use in their JavaScript projects. Some of these libraries are itty bitty and just a few lines of code, while others are massive.

Many NPM packages depend on other NPM packages, and these dependencies will be installed automatically when you add them to your project.

>[!warning] You need to check the licenses of the packages you're using as dependencies -- including *their* dependencies, and their dependencies' dependencies... -- to ensure they are compatible with the license you want to release your plugin under. *See: [[Copyleft Licensing]]*

Some dependencies are not built into your final project at all, and therefore may not affect the licensing of your final product. *See: [[Types of Node Dependencies]]*

>[!warning] If any of your build dependencies use Node's own modules (like `fs`) or use anything OS-dependenent, your plugin will not run correctly on mobile.

One interesting feature of NPM is that you'll need a `package.json` file in your project to keep track of what dependencies you're using, your project settings, and any custom scripts you want to be able to quickly run from the terminal. This is referenced by lots of other packages you may use.

The `package.json` also has lots of options that are specifically for publishing your project to NPM, like the package name and tags. These can be ignored, as you generally **should not** publish a plugin to NPM, just use NPM to access helpful tools.

>[!note] Obsidian requires that you have a similar file, `manifest.json`, which is where you'll put information Obsidian itself uses to publish your plugin on their in-app marketplace. The information there is unrelated to the `package.json`.

## Github

So where *should* you publish your plugin? Obsidian manages its community contributions through **GitHub**^[[Github's Official Site](https://github.com/)], which is both a social site for sharing source code and program releases with the public, as well as a tool for developers and businesses to privately manage changes to their source code. 

>[!note] Github is based on **Git**, a popular and highly intelligent version-control tool that lets you track small changes throughout a codebase.

Although **Obsidian itself is not open-source**, there is an expectation that plugins will publish their source code, so that it can be reviewed by Obsidian's developers (for best practices and malignancy) before being added to the in-app marketplace.

Since most developers will be using Github for their projects already, it makes sense for plugins to be managed through the site.

>[!note] Obsidian plugins can be side-loaded / manually installed without being on GitHub and without having their source code published in an accessible form, in case you would like to make something for your private or commercial use.

When your plugin is ready to share with others, you need to clone the [Obsidian Releases repository](https://github.com/obsidianmd/obsidian-releases), add your plugin information to it (following the rules and template they provide), and make a descriptive pull request with the appropriate flair, template, and such. [More information (at the official site).](<https://publish.obsidian.md/hub/04+-+Guides%2C+Workflows%2C+%26+Courses/Guides/How+to+add+your+plugin+to+the+community+plugin+list>)

>[!note] A pull request is when you ask the mainainer of a Github repository (the codebase itself) to integrate your changes into the main codebase. In this case, you're asking to change the code of the in-app plugin marketplace to include your plugin.

The plugin information they request (below) would match data in your `manifest.json`.

```json
{ 
  "id": "",
  "name": "", 
  "author": "", 
  "description": "",
  "repo": "<github username>/<repository>", 
  "branch": "master" 
}
```

The pull request description has its own template that is more involved, which you'll be prompted to use when you go to make the PR.

## Markdown

#unfleshed 

## YAML

#unfleshed