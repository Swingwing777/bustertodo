# To Do!
### A Vue.js Experience
To Do! is a Vue.js-based task list application, using local storage for a simple client-side experience.

![Todo_App(450).png](https://sweepback.co.uk/supportfiles/Readme%20Support%20Media%20-%20for%20Sweepback/Todo_App(450).png)

### Tech:

To Do! uses the following technologies:

* [Vue.js] - A framework for building native apps using React.
* [Node.js] - A JavaScript runtime built on Chrome's V8 JavaScript engine.
* [npm] - Build Amazing Things.
* [Vue CLI] - An official CLI for quickly scaffolding ambitious Single Page Applications.
* [Vue Material] - Component library.
* [Vue Draggable] - Draggable Package.
* [Heroku] or [GitHUb] - Hosting.
* [Dillinger] - The Last Markdown Editor ever.
* And of course 'To Do!' itself is open source on [GitHub].
----
### Installation:

To Do! requires [Node.js LTS](https://nodejs.org/) to build.

Install the latest LTS version of Node.js, update npm, and then install the latest version of the Vue CLI:
```sh
$ nvm install lts/*
$ npm update -g
$ npm npm install -g @vue/cli
```

----

### Creation:
Create the new app by typing:
```sh
vue create <appName>
```
eg `vue create bustertodo`.  Some presets will be offered.  The defaults are suitable.

----

### Enhancement:
[Vue Material] is used to add the task cards:
```sh
$ npm install vue-material
```

[Vue Draggable] is used to add a reordering capability:
```sh
$ npm install vuedraggable
```

----
### Run App:
To test run the app at any stage during development, type:
```sh
$ npm run serve
```
Check the result via `localhost:8080`.

----

### Production Build:
To run a production build of the app (within app directory), type: 
```sh
npm run build
```
This builds a new "dist" directory that needs to be added to GitHub:

----
## Hosting:
This is as per developer's preference.  Both GithHub Pages and Heroku are suitable hosts.

##### For Github:
After build is complete, install the Heroku CLI if not already installed:
```sh
curl https://cli-assets.heroku.com/install.sh | sh
```
Then create a `static.json` file in the project's root directory:
```sh
{
  "root": "dist",
  "clean_urls": true,
  "routes": {
    "/**": "index.html"
  }
}
```
Add the new file to git:
```sh
$ git add static.json
$ git commit -m "add static configuration"
```
Then deploy to Heroku:
```sh
$ heroku login
$ heroku create
$ heroku buildpacks:add heroku/nodejs
$ heroku buildpacks:add https://github.com/heroku/heroku-buildpack-static
$ git push heroku master
```
QED!

----
### Todos

 - Add frontend local storage to allow a user to review their answers.

----
License 
----

##### DWhal
* Email via GitHub.
* [GitHub]
* [LinkedIn]
* [Twitter]



   [Dillinger]: <https://github.com/joemccann/dillinger>
   [Node.js]: <https://nodejs.org/en/>
   [npm]: <https://www.npmjs.com/>
   [Vue.js]: <https://v3.vuejs.org/>
   [Vue CLI]: <https://cli.vuejs.org/>
   [Vue Draggable]: <https://github.com/SortableJS/Vue.Draggable>
   [Vue Material]: <https://vuematerial.io/>
   [Heroku]: <https://bustertodo.herokuapp.com/>
   [GitHub]: <https://github.com/Swingwing777/bustertodo>
   [LinkedIn]: <linkedin.com/in/david-hales-3450305a>
   [Twitter]: <https://twitter.com/dwhal>