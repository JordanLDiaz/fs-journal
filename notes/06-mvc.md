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


  Wed., November 9th, 2022
  GregsList

  Steps for setup
  1. reference "figma" - Craigslist, to decide what we want in our model. 
    - open up models and make Car.js, 
      export class Car{
        constructor(data)       // data in place of all the properties we want, see reference for all data types we placed here. 
    }

  2. now we need appState.js to store our actual data. 
  update the appstate event emitter w/Car instead of value, this helps with our intellisense! also add car [new Car]
  ****When typing Car notice the intellisense that pops up and hit TAB to actually link to models/car.js, then add one instance of car with all the properties. 

  3. Build out template for model in html. Can get rid of app class (do same in app.js).
  - view bootstrap cards for options for car cards. add to template, replacing the info w/stuff from appState car Array. Changed href to button instead. Copy this template, comment out and move it to car model under constructor array.

  4. Use getter function for the template above. 
    get CardTemplate() {
      return `
      HTML GOES HERE 
      `
    }

  **Don't worry about changing this yet. Will get there.

  5. Now make CarController. add export class CarsController() {}
  Always start with console.log after this! 
  
  6. But first in app.js, add carsController = new CarsController() and invoke so it links, 

  7. now we want a private function in controller so user doesn't have control over it. User can only call things inside carsController class. 
  - add  _drawCars() in controller. be sure to tab on all intellisense so it links the appstate. 
  - view drawCars function w/template, etc in reference. 
  - add car id in html (this later became listings so we could use it for multiple pages)
  - be sure to call function in carsController class/constructor
    _drawCars() (just asking it to draw cars on page load.)
  - should now be able to draw saturn to page.

7. still? add new car to appstate array. this will post the same info as saturn because we haven't updated the html in our model. Update with string interpolation ${this.make}, etc. We use this.make, etc because that's how we labeled in Cars model array. Replace one interpolation at a time and test! this... refers to the instance of the class

8. delete button - in model-->cardTemplate, add button for delete with i class. Reload page to view. Now we want an alert to pop up to make sure they want to delete. We want to do this by adding onclick to model w/ app.carsController.removeCar()  (will need to go to controller and add delete function, first console to make sure it works.)
- added title tag to html button in model.
- after consolelogging, now create rest of function. w/pop alert (there is a pop.js utils reference, but also can go to sweet alert site which has nicer looking window popups). Add sweet alert cdn to head!!!
- ADD Pop.toast('')      // don't forget intellisense!
- pop utility has confirm function. depending on which button they click, it returns boolean value. 
- async await - see note, we'll learn more about this next week. 

- When you need to change data in appstate, that's when you should create service file. add carsService file, then export. add removeCar() method in carsService class, and console to make sure they're communicating. Once we've done that, can move on. 

- To be sure we're deleting correct instance, we can add a this.id to our model using generateId (method that we can reference in utils)
  this.id = generateId()     //don't need data. on this one because it's just running a function. Don't forget to add string interpolation to our template for this.id.
  - add this to button in model, AND add carId as param in controller function removeCar, but then need to update that in service file too. Othewise it won't work because service didn't expect to be passed that. 

- delete w/filter:
  - in service: add to removeCar() --> let filteredArray = appState.cars.filter(c => c.id != cardId)
  ** this gives new array of cars where all of cars id's do NOT match id I passed down. 
  - appState.cars = filteredArray
  console.log(''...)
  - at this point it disappears from console, but not page. We need to add appState.on('cars', _drawCars)    -- this guys job is to listen for something to change, and then communicate the function to occur (drawCars) to the appState.

  9. Create a car --> html hard code in main above car template
  - bootstrap for forms (we used floating labels in forms, see adjustments in reference for changes, especially the id's!).
  - made reference form for all properties we want to include. 
  - add onsubmit="app.carsController.createCar() to form
  - add submit button
  - console in controller w/ createCar() {console.log('creating car'
  )}. currently page refreshes auto, so add window.event.preventDefault to createCar() in controller. now it will stay in console. 
  - now to grab data out of form: 
    - let form = window.event.target   //'Target anything that has to do with the event that just happened', then console (form)
  - reference form handler --> get form data that will return us an object with that data (need to fully open this reference).
  - in controller --> let formData = getFormData(form), console again.
  - first time it created empty object, so we need to add name to each input field in form. don't need one for generate id because we have a function handling that for us. 
  - can change button TYPE to change it's function. buttons have type=submit auto'd, so if we don't want it to submit, change to type=button, and make sure it has correct function attached based on what you want it to do. Reset type exists!
  -img field ... type=url
  - after making form, add input required to each field so they can't submit empty form. 

  - controller --> need to send the data to service. In createCar(), add
  carsService.createCar(formData) (createCar will throw error, click, ctrl . and create function.)
  - service --> now has createCar(formData), so add
  let newCar = new Car(formData), then console.
  - after console works, need to store to appState
    appState.cars = [...appState.cars, newCar]     //spread operator = dumps out data to new array

10. Active details drawn to page
- view modals on bootstrap: good place to show details of the car. grab html and add new section with this in html. past underneath footer so its hidden until needed. 
- we want to attach model to 'see details' button. grab toggle and target, go to car.js, find see detail button, past toggle and traget there.
- add onclick="app.carsController.setActiveCard('${this.id}') to modal, 
  then in carscontroller --> create setActiveCar(carId) {
    carsService.setActiveCar(carId)
  }
  - services --> in setActiveCar....
  let foundCar = appState.cars.find(c => c.id == carId), then console.log
  - now need to setActive car in appState, copy new intellisnse(@type...) after cars[]
    activeCar = null   // notice, not an array!
  - service --> in setActive, add
    appState.activeCar = foundCar
  -- controller --> add new listener
    appState.on('activeCar', _drawActiveCar)

    create new function _drawActiveCar()
      console.log('draw active car')

  - now to actually add it to modal by creating new template in html first (hard code hidden in modal)
  - once happy w/form, copy and comment out so we can add to template in model. add to activecartemplate w/ return ``, add id to modal-content (id="details"), 
  - in controller, update drawActiveCar function with setHTML...then update string interpolation.

  11. Want to add a date the entry was created. go to car.js --> add:
    this.createdAt = new Date()   (this is built-in js code, passing in no params will give today's date.)
  - update details template w/string interpolation.
  - added this.createdAt.toLocaleDateString() in interpolation.

  12. local storage - store utils has reference functions (saveState and loadState)
    - have to supply key (name of collection (e.g. cars), and..)
    - in service --> add saveState (this saves to local storage, but still need to load)
    - appState -> add cars = loadState('cars', [Car])
    - now we need to save again so it actually saves any changes if we delete.  (saveState('cars', appState.cars))

13. static method --> GetCarForm() 
- only exists on uninstantiated classes, added to car model, then in controller class, added Car.GetCarForm() (not instantiated)

Thursday, Nov 10th, 2022

Redacted - 
1. Created index, model, CasesController.js, CasesService.js, app.js, AppState.js
2. model --> Case.js
  export class Case {
    constructor(data) {
      this.id = generateId()              // in utilities, guarentees unique property
      this.report = data.report           //report will contain all the info
      this.clearance = data.clearance     // levels of secrecy
      this.agency = data.agency           // tells who recorded this
      this.date = data.date || new Date()               // time report was submitted OR backdated date, always put data.date first because it will read that first.
    }

    get ListTemplate() {
      return this.report       //made this right out of the gate to start getting cases on page. 
    }
  }

3. appState.js --> cases = [
  new case ({
    report: 'hudahahufa',
    clearance: 'secret',
    agency: 'emoji',
    date: 
  }),    // also added 2 more cases, this is just test data that will be replaced later
]

4. controller --> 
export class CasesController {
  constructor(){
    console.log('fjd')    // make sure this logs
  }
}


5. service --> class CasesService {

}
 export const casesService = new CasesService()

 6. app.js 

 7. controller --> add function _drawCases() {
    let cases = appState.cases
    let template = ''
    cases.forEach( c=> template += c.ListTemplate)
    set HTML('case-list', template)     // then add _drawCases() to constructor (step4), after making sure this draws to page, comment it out so you can build out actual template in index (this came AFTER step 9)
 }

 8. index --> got rid of app id, added section for case list, added id="case-list"
 9. added section for active case (col-9)
 10. added template in html for case-list, copied, commented out, placed in model in ListTEmplate. Replace data spots w/string interpolation.
 11. after listTEmplate, add:
  get ComputeTitle  (still w/in class)

12. index --> under case-list section, added new case form section. place clearance, agency, date here.
select element gave drop down menus w/options, in input line added id and name, select line has id and name as well. 
added required and min and max length for agency
added button w/iclass
- tested
on form add onsubmit to link to our form

13. controller --> 
createCase() {
  window.event.preventDefault()   // prevents page from reloading
  console.log()
  const form = window.event.target
  let caseData = getFormData(form)
  console.log()     // now should get info to log to console w/details
  casesService.createCase()        // sends to service
  form.reset
}

14. service --> 
add createCase(caseData) to class CasesService    w/const newCase = ... and appState.cases =...

15. controller --> add listener to cases (appState.on('cases...))
got an error for line 8 in appState even though error was in controller, missing caseData param

16. model --> added if statement for report in get ComputeTitle 

BREAK - he reformatted form with way for clearance to default to our options only w/selected. 

Active case steps
17. model --> listTEmplate(), added onclick to app.casesController.setActive('${this.is}')
18. controller --> created setActive(id) w/ console to make sure it worked, then...
casesService.setActive(id)   // link - to services, then add id to console log.
19. services --> const activeCase = appstate.cases.find(c=> c.id == id) 

20. appstate --> add activeState = null, then in service add appstate.activecase= activecase
21. controller --> create drawActiveCase function w/console, then in constructor add listener (appState.on after other listener)
22. index --> add to active case section, create template, comment out and add to activeCase in model. replace data spots w/string interpolation. 
23. controller -> in drawActiveCase add setHTML('active-case', activeCase.ActiveTemplate)

24. want to prevent being able to look at reports until password is entered. think clearance type. 
appstate --> added classifiedWords = {'codeworks', 'ufo', 'mole'....etc}
25. model --> create new getter 
  get ClassifiedTemplate     // same as active template, but change this.report to thi

  also add get ComputeClassifiedReport() {
    let reportArr = this.report.split(' ')      //this turns report into array of words
    let redactedArr = reportArr.map(word => {                 //array.prototype.map() creates copy of array with actions performed on it.
      if(appState.classifiedWords.includes(word)) {
        return '   '} else
        return word
      }

    })
  }

  26. model --> updated classifiedTemplate with onclick button to unlock case. 
  27. controller --> added async unlockCase()     w/ pop.prompt
  28. appstate --> added clearanceLevels to appstate class
  29. servie --> added unlockCase(input) in CasesService class

Checkpoint tips
  generate list of things first before active note
  - mock requirements = note on mock
  - color picker - look up bootstrap component (or pick from bootstrap colors)
  - undraw = placeholder content (stretch goal though)
  - musical note = mdi icon for color