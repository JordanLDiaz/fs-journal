# MVC
Week 2
Monday, November 7th
Classes with ES6

MVC -
* Grouping/encapsulating code that works/fits together with like functionality

'View' - what the user sees. 
  - Includes HTML, DOM, index.html
  - HTML representation of data in the app

'Controller' - first layer of application
  - Takes in actions from user
  - Draws data to DOM (where it lands)
  - controller is the only thing you give user access to (other than screen updates w/html)

'Service' - layer where business logic happens
  - Anything that modifies data = business logic

'AppState' - single source of truth 
  - Where all the apps data will be stored
  - where you keep all your variables ('data store')

'Model' - 
  - blueprint of what data in the appstate will look like

**BCW serve instead of mkdir and touch in cp**
- This auto creates all the files/templates you need. 

- Organize files in VSCode by the order in which MVC works, eg: index --> valuesController --> valuesService --> appstate --> value
- Any of the javascript files that are in templates, DONT CHANGE, these are just to reference

Combat Tracker Notes
- Start at the model - the blueprint that tells us what things will even look like.
- Start w/ appstate.js (ONLY ONE PER PROJECT)
  - Within this, created character.js and put:
  - export class Character {

      constructor(name, tyoe, maxHealth, currentHealth)
        this.name = name
        this.type = type
        this.maxHealth = maxHealth
        this.currentHealth = currentHealth
  }

  - added constructor inside export class - these are the things that make the character a character
  - class character itself is a function - just defining what it will be, not actually creating it yet. 
  - this = special word that talks about itself, takes info that we will later provide and attach it to this character. 

  back in app.js, created spaot to save our data:
  - classes are made through 'new' keyword
  - when you added new Character below, it automatically added an import tag at top of appstate.js. if it doesn't, something isn't linked/typed correctly. DONT TYPE THESE YOURSELF! 
  - parentheses at end of new Character() contains the info you want to add, and tells it to do the function

    characters = [
      new Character('George', 'Dwarf Druid', 24, 24),
      new Character('Strahd', 'Vampire', 240, 240),
    ]

    -Added new file = charactersController.js, added export class (see below)
    - added drawCharacters method, but so far don't have characters here, so how do I bring it here? Import appState.js by adding it to front of characters.forEach. this will add import line at top of file automatically. 


    export class CharactersController {

      drawCharacters() {
        //TODO i need the characters
        let template = ''
        appState.characters.forEach( c=> template += `

        `
        )
        document.getElementById('app').innerText = template       
      }

    }

  app.js is 'doorway to our application', needs to be linked 
  Added: 
    class App {
      charactersController = new CharactersController()    //notice lowercase changes to capitalized. Capitalized is just the blueprint, lowercase is the actual objec that has been built out
    }

  - missed a couple steps (confused), but...
   then added character formatting w/ string interpolation in charactersController.js (within the for each)

  
  Tuesday, Nov 8th
  Gachamon
  1. Start w/model --> create gachamon.js under models (THIS IS SINGULAR). We are starting w/singular  gachamon.
  2. in Gachamon.js, create:

  export class Gachamin {
    constructor(name, emoji, colorVariant, rarity) {
      this.name = name
      this.emoji = emoji
      this.colorVariant = colorVariant
      this.rarity = rarity
    }
  }

  3. Now that we have model of what it will look like, open up appState.js to store. Within class AppState (make sure its within curly brackets!) put: (below). notice it is now plural because we are creating multiple gachamons
  gachamons = [
    new Gachamon('Albert, '🐩', 1, 50)
    new Gachamon('Koko', '🦍', 1, 40)
    new Gachamon('Kim', '🦩', 1, 10)
  ]

  4. now that we have our array of Gachamons, create controller and service. These need to be plural because they are handling all of the gachamons. 
  Create GachamonsController.js AND GachamonsServices.js
  In Controller, put:
  export class GachamonsController {
    constructor() {
      console.log('gachamans controller loaded');
    }
  }

  In Services, put:
  class GachamansService {

  }
  export const gachamanService = new GachamansService()

  5. Open up app.js, which is the middle man between index amd controller
  in class App, put:
  gachamansController = new GachamansController()

  6. back in controller, add:
  drawGachamans() {
    let mans = appState.gachamans    //this is grabbing it from appState.js
    let template = ''     // creates empty array
    mans.forEach(m => template += m.name)     // testing just on its name first without putting in ``html yet in case we get bugs
  }

  7. in index, created place to grab data from (see his index).
  8. back in controller, under forEach add:
  setHTML('gachamans-list', template)

  consolelogged app.gachamansController.drawGachamans() and it returned the names to the page (not just in console). If this works, we know our draw is working and things are linked properly.
  - view the writer.js file under utils for reference for get and set HTML and text

  9. back in index, added mans list section and stubs mans template
    notes:
    - selectable = built in class for this template that creates hover affect on page (shows it is selectable)
    - title='Kim', makes it so when you hover over kim image, her name will pop up
    - popovers on bootstrap.com - checkout 
    - copied the html for kim, commented out, then put it in gachamon.js, still inside class Gachamon, add:
      get ListTemplate() {
        return `<h3 class='col-1 p-2 selectable' title='${this.name}'>${this.emoji}</h3>`
      }

10. back in controller, add this.drawGachamans() to Gachamans 
then changed template to ListTemplate so that its grabbing the html we put in ListTemplate()

11. If we click on emoji on page, we want it to show bigger image and more info. To do so:
  in gachamon.js, add onclick to list template
  onclick='app.gachaman

12. in controller, add:
  setActive(name) {
    gachamansService.setActive(name)
  }

  then in services, make class GachamansService {
    setActive(name){
      let mon = appState.gachamans.find(m => m.name == name)
      console.log('selected', mon);
    }
  }

  13. back in appState, after gachamans array, add:
  activeGachamon = null  ///this is the initial state, so there is no active Gachamon yet.

  14 . back in services, add appState.activeGachamon and console.logged

  15. controller - add
  drawActive() {
    let mon = appState.activeGachamon
    setHTML('active-gachamon', mon.emoji)
  }
  then in index, added section for active-gachamon
  then in controller, called this.drawActive() in setActive method
  then built out template under active gachamon for kim, added css styling for gachamon-detail
  
  16.copied this html and in gachamon.js, added:
  get ActiveTemplate() {
    return html here, replace details w/string interpolation
  }
  incontroller, changed drawActive to emojo to mon.ActiveTemplate

  17. index --> added 2 new sections for get mon 💰 button
  in controller, added addCoin and getGachamon functions. added onclick to those buttons in index (app.gachamonController.AddCoin() and onblur=app.gachamansController.getGachamon()). also added gach-btn class to style in css with:
    .gacha-btn:focus and no focus (see css)

  18. in controller, now add gachamansServe.addCoin() and other function
   then in service, add these as well
   in appState under null, add coins = 0
   in service, addCoin() {
    appState.coins++
   }

   getGachamon() {
    if(appState.coins > 0) {
      appState.coins --
      let mon = appState.gachamans[0]   /// replace later with random
      appState.activeGachamon = mon
    } else {
      console.warn('no coin')
    }
   }

   at this point, no gachman is being selected on page because we havent draw it yet.
   Draw in controller -- getGAchamon (this.drawActive())

   19. controller --> export class GachamansConroller, add
    appState.on('activeGachamon', this.drawActive)  // this gives instructions to run later, not now. 
    commented out draw from set active and get gachamon in controller.

  20. appState --> extra code that's already there needs to stay or it won't run. 
    next when we get Gachamon, we want to add to my collection so, add: 
    in serveices add: appState.activeGachamans.push(mon)
        console.log(appState.myGachamans)
   in appState --> myGachamans = []

   21. in index, added new row for my-gachman collection
   in controllers, add drawMyGachamas() {
    let mans = appState.myGachamans
    let template = ''
    mans.forEAch(m => template += m.listTeamplate
    setHTML('gachamans-list', template))
   }

   22 . services --> change appState.myGachamans = mon to.... [...appState.myGachamans, mon] (see below for details)
   this should trigger listener so that albert is actually draw to page/added to list. 

   ... = spread operator, when its placed infront of array, takes items from array and 'breaks them out".
   arr = [...arr,4] will add 4 inside array with original contens and the new item added to it. 


   Private functions vs. public methods
   - currently we have all draw functions in controller class, which we don't want because user doesnt need access here. move them to outside of controller class (still in controller page), but need to put function in front because they are no longer methods. 
   - place underscore infront to designate as private function (did this for methods in gachamansController too). controller can reach them, but user doesn't have access anymore because they are private. Not totally necessary, but is best practice. Makes it more organized. 

  23. appState --> changed out simple array to array of objects w/data constructors, in gachamon.js, changed constructor name to (data) and data.name, type.name, etc. 

  24. added ,sirt ti _drawGachamans in controller
  25. for speciality mons, add css class to target styling on them (color-1, 2, 3, etc)
    - filter: hue-rotate(20deg) invert(100%) - vary degree, this will alter the colorVariant
    * need to add color-${this.colorVariant} to getListTemplate and get ActiveTemplate()in gachamon.js
    - drop shadow = glow

    26. service --> add:
      function _rollGachamon() {
        let mons = appState.gachamans.sort{(a,b) => b.rarity - a.rarity}
        let randomNum = math.round(Math.random()*100)
        console.log('rolling', randomNum)
      }
      place function in getGachaman in controller.

27. save to localstorage:
  utils --> store.js -->saveState function
  add saveState('myGachamans', appState.myGachamans) in service
  - put this in trash function too.
  - when item is set to localstorage, it loses its class and becomes regular pojo, so when it's taken back out, it needs to get that class back. 
  -to do this in appstate --> under coins = 0 changes myGachamans = loadState('myGachamans', [Gachamon])   // this is where they are kept, and where they will be. 