# MVC

Monday, December 5th, 2022

*Just remember that this week is largely considered the 2nd hardest week of the course...it will be ok!
* Videos are newer, but there have also been a lot of changes in vue this year. 


Vue Video Notes - 


Vue Lecture Notes - 
- Creating vueMiner - bcw create w/vue starter
- No longer using html unless changing title or favicon, everything will now be injected (think our templates w/string interpolation)
- front end has package.json and dependencies like back end did last week. No longer have to add links to html, what we need is built in (dependencies).
- most of what we'll do is in src file, notice a lot of similarities, but no controller. 
  - new = pages and components, this is where we'll be writing our vue components. 
  - We'll still write vanilla js in services and such. front-end services will work pretty much the same, same with appstate, model,
  - components are basically taking place of controller. 

Vue components - 3 parts
1. template (html)
  - like the getTemplate, but not linked with a class. 
2. script (js)
3. style (css)

To start up webpage - debug console like w/node
- vite - compiles our files into chunks that can be read by browser. 
- in terminal ran npm i 
- now when we save, the webpage should update automatically. Only what was changed will be redrawn, not the entire page. This makes the process less heavy/consuming. 

Started Project
- homePage --> added h1 for vue points to template
  - in script, see first rule of vue. for template to access data in the script, it must be in the return. 
    - added {{vuePoints}} to template in h1 that was added. 
  - next added v-on:click="", which is the old version, so now we'll do @click="vuePoints++"
  - we can check webpage but it won't be updated yet onclick. 
    - in script, changed vuePoints: 0 to vuePoints: ref(0)      //this should be able to be imported into script. 
    - now the 0 and vuePoints is reactive. 
    - the appstate imported reactive
    - now we can click our vue logo on webpage and the count updates. 
    - BUT since vuepoints is important for more than just here, we want to move it to our appState. How do we get it from appstate back to homepage?
      - commented out old vuePoints (see comment), and added vuePoints : computed(() => AppState.vuePoints)  // this is pointing at a specific reactive component
    - added function to script (under setup)
    function mineVuePoints
    - then created pointsService.js (w/regular skeleton) that includes function
    - in homepage linked pointsService in function, and also had to add mineVuePoints to return. 
    - note @click="mineVuePoints" w/o ()
    - you can also put the function in the return, but this isn't best practice if the function is async. 
      - also if its outside return its considered private, inside is considered public. imagine setup like the js file itself, and the return is like the class, in regards to private/public functions. 

  - appState --> added upgrades{} for auto and click
  - homePage --> now to draw to page, added section for upgrades
    - to col-6 for upgrades, added to div   v-for="item in upgrades"
    - updated {{item.name}} into h4 template below the v-for, also updated button and div.
    - on buy button, added @click="buyUpgrade"
      - then added buyUpgrade to return w/logger
    - bring in upgrades to return w/ upgrades: computer(() => appState.upgrades)

  -pointsService --> added buyUpgrade function
  * dont forget to add buyUpgrade in homepage return. didn't have to tell it to draw, it just did it. 
  - updated pointsService mineVuePoints() w/ let clicks = and clicks.forEach
  - also added upgrade.price to buyUpgrade()

-homepage --> added :class="{bg-primary...." (see note about :)
  - also added :disabled to button so that user can't select if they don't have enough points. 

Aboutpage - moved template here, now you can click the about button on page and it also has the game.
- created Shop.vue so that the aboutpage has the shop
  - added to return --> upgrades, took template from homePage and moved it to shop
  - added vuePoints to return

pointsSErvice --> added startPointInterval() in class, also added let _interval = null outside of class.
- added logger to this function and an else with logger

homepage --> under setup(), added onMounted() (when this component gets loaded to the dom, it has been mounted)
- moved onMounted to aboutpage


SETTING UP VUE PLAYGROUND
- forked from github, then git clone in command prompt
- in vscode, open terminal and run npm i
- then spin server and go to localhost:8080, should now see playground.

VUE PLAYGROUND NOTES
Expressions
- Vue parses html and looks for double curlies.
- when it finds {{}} it says, "Pause HTML, evaluate the javascript, restart HTML"

Bindings 
- How do we get the data from js code to page?

One Way Binding...
  - data goes from component to html.
  - we bring in properties by referencing them from components state object w/double curlies.
-State
- Inside our setup method, we create a reactive object that is called the state. 
  - the state holds all info unique to that page, allowing us to manage/maintain the state of the component in use.
  - this is wehre we safely store data that will be rendered to that page. 
  - reactive constructor for this object creates watchers for all properties allowing binding to work. 

Two-way binding...
  - goes from html input tag, to js, and back to another location in html.
  -use the v-model to tie input in html to js. Uses curlies to render value in html. 

Class Binding
  - used to supply values for an attribute within an element.
  - use : to show whatever comes next should not be evaluated as static value, but as a reference to an object.

** with vue we don't need to call update or draw functions anywhere because the view (html) is already bound to property of the object, that way when js changes, so does the view. 

Looping
- can easily repeat snippets of html for each index of an array/each property in object. 
-v-for directive allows you to automaticall loop over array and utilize item in current iteration to build/populate html. 

Computed Properties
- helps limit putting too much logic in template, also gives ability to access same computed value in several places in template. 

Conditional Rendering
- allows us to draw blocks of code to screen if condition is true. 
- do this by attaching v-if to element, then a direct boolean or expression that evaluates to one. 
-v-if, v-else, and v-else-if directives (similar to js)
- v-show directive
  - element will always be rendered on page, but condition of v-show will toggle the css properties. 

Events 
- Listening to events - having template and js on same page, its easier to see element we want to target, supply event, and call method to trigger event. 
- Events w/inline actions - @click
- Events calling a method - added value to @click is name of method (outside of state object because it isnt data)

