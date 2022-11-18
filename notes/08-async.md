# MVC

Monday, November 14th - HP Trading Cards Lecture

Asynchronous code - helps to use multiple platforms to run code together asynchronously.
- Because we are working with things outside of our control (eg waiting to get functions/data from other places), we don't have control over that so we need to have asynchronous functionality. (idk...he explained better)
- typically we think of code running from top to bottom, line by line. Async still goes from top to bottom, but allows us to pause on certain lines of code and wait for something to happen. 
  - think of pop.confirm - we had to wait for user to make a decision, so that paused our code. without async await, it immediately removed our code without letting the user confirm. This is why we use async! (small level example)
  - allows us to pause code from continuing until it has something to return
- promise - api can promise that the info will come back, we can hook our async/awaits to these promise lines. it will either be returned or rejected for some reason, but want to give it something else to do if it is rejected.

Opening up API (application programming interface - aka the barista in coffe example)
- use routes to request data (like a menu, what can we request from api?)
- looking at harry potter api, if we click it, it gives us json data.
- look at network data in dev tools, it sent a request, and shows the info that came back. 

** JSON viewer extension - should add to browser **  added!
API - allows us to use data other people have created

How do we get this data into our application?
1. MAde new controller (CardsController), made new service (CardsService), made Card.js model.
2. Don't start with data model first this time since we did not create it.
3. open app.js, add 
cardsController = new Card()
4. then added iport and export in controller and service. 
5. controller --> added constructor w/log in class CardsController
- in constructor, add async getCards() {
  console.log('getting cards')
}
6. appstate --> cards = []    // start w/empty array since data doesn't exist on my computer. 
update import type w/card instead of value.
7. Check application to make sure it loads. See network tab and you should see that it added all of the data
- switch to fetch/XHR - it disappears because it only logs specific fetch requests which we havenbt made yet.

8. controller --> in getCArds(), add
await cardsService.getCards()   (we want controller to wait)
add console to see it happen

9. service --> in cardsService class
getCards() {
  const response = await fetch('url string of api')
  log(response);       
}

 10. in console, app.cardController.getCards(), returned a promise pending and then the response w/data, tHEN the got the cards console. 
 check out response status () in console response

 11. service --> in getCards add     // check out the fetch method note here...note this was then commented out. 
 const data = await response.json()
 then console.log('thedata', data);
 )

 12. in index, add script for axios (should be intellisensed cuz I have it downloaded, but need to make sure to bring it in- its a package of someone elses code that makes getting more complicated fetch requests easier)
 . then in service --> 
 import axios from 'axios' (notice it says it doesnt exist but it does... we had errors in console here... turns out we don't need the import statement, so ignore the import above. )
 - then in getCards, add const response = await axios.get('api url')
 console.log('axios res', response);
 - this should return axios res w/ data containing a shit load of objects. 
 - changed console to response.data, which will give you just the data in an array cut up into sections.

 13. Now that we have the data, lets saveit. service -->
 add appstate.cards = response.data
 controller --> add private function
 function _drawCards() {
  let cards = appstate.cards
  let template = ''
  cards.forEach( c=> temaplte += c.name)
  setHTML('app', template)
 }

 then in constructor on controller -->
 this.getCards()
 appstate.on('cards', _drawCards)      // added listener here so that whenever it happens, draw it. This is key since it's async and we don't have control of it. 
- then we consoled it and got the names on the page (ugly, but its there)

14. controller --> commented out get line for a sec
15. index --> updated container-fluid w/ cards id, then updated id w/cards in controller
- built out this section in html w/image and title, plus whatever other elements we want to appear on card (view card template stub)
- copied template, commented out, added to card model w/ ...
export class Card {
  constructor(data){
    this.name = data.name
    this.house = data.house
    this.ancestry = data.ancestry
    this.image = data.image
  }
  get CardTemplate() {
    return `
    card template here    // add string interpolation now
    `
  }
}

- updated _drawCards c.name to c.cardTemplate
- we got undefined on page and added debugger to drawCards function to see what's wrong. We got pojo's instead of cards like we wanted. The response.data is just pojo's, so how do we convert this? 
- in service --> appState.cfards = response.data.map{cardDAta => new card} (didn't finish this line so look at reference)
- once updated to map, should now get all cards to load, thoughit is slow w/so many plus some don't have image.

- we don't want all of these characters, so went back to "menu" - api url

16. updated html w/section buttons (see reference)
- added 4 buttons to add each of our properties/values
- controller -> 
added async getHouseCards(house) {
  console.log('getting cards from', house0;)
  await cardsService.getHouseCards(house)    // declare method to link to service
}

then in index added onclick to each button (app.cardsController.getHouseCards('gryffindor')), repeat for each button. 

17. in service --> add getHouseCards(house) {
  const res = await axios.get('api url/' + house)
  console.log('house data', res.data)       //always log res data here 
}
// we got a 404 error, looked at network, said cannot get gryffindor. we went to wrong menu so need to update url w/correct path. 

18. played around with debugging here: controller -> in getHouseCards function, added try/catch (see reference)
- this stops code from running as soon as error occurs, jumps back to controller to throw error message as instructed.
- can also do pop.Error so user gets notification when error happens (but also need try/catch for pop to work here)

19. now that we updated url path, it should work now, auto organizes by house. 
20. updated class card constructor w/ this.alive = data.alive AND in cardTemplate added p tag with ${this.alive ? 'still kicking it' : 'kicked the goblet'}

reminder: axios is library that wraps around fetch, which goes to get the data, makes it easier to use. use axios instead of fetch (notice that it's commented out in controller(I think...wrong file?))

- how to access info from api can be hard, make sure url is accurate and that model is interpreting information correctly. 
** generate api url is helpful on trivia question. 
- notice the data is inside results   (res.data.results.map)   // the results is an extra layer we need to make sure we include or our path is wrong and won't work. 

AXIOS Notes - from precoursework
- This is primary way we will make XHRhttp requests throughout rest of course.
- uses promises with all requests
- use to help avoid repeating segments of code.

1. Bring axios into code by adding script tag w/JS tags in index


Tuesday, November 15th, 2022

* bcw-sandbox/herokuapp.com - lots of references ('endpoints') and schemas for various data types (schema is word for model we use when referring to our databases rather than our own data we have put directly into model)
* add /api/cars (or other section you want to access)
* sandbox allows us to create, edit, delete things. This is a shared endpoint though, so just be aware of that. We'll learn to create our own api's next week.

Gregslist Async - today's project
1. created controller, service, model with skeleton.
2. app js -> added carsController
3. appState -> added cars = []
  deleted all old value files once everything was set up, can do if feel comfy, but don't have to. Just better organization and fewer files. 

4. referred to car sandbox to view schema and its rules. 
5. model --> added constructor w/data in class Car (straight from last week's proj, except no generateid and mileage because not in sandbox)
  - make sure the way each property is written matches the sandbox to prevent easy errors.
  - created simple get ListTemplate() {
    return this.make + this.model
  }

  6. controller --> added to bottom of CarsController class
  async getCars() {
    try {
      await carsService.getCars()
    } catch (error) {
      Pop.error(error.message)
      console.log(error)    // this helps error persist in console because if you just do pop error, it will disappear after a few sec.
    }
  }

    - tested app.carsController.getCars() in console and got error
  
  7. service --> in getCars() in carsService add:
  const res = await axios.get('api url')     //don't forget script tag in index for axios!!
  console.log('[got cars]', res.data)      // always console.log res.data first when using axios, before you do anything else. Makes sure we know what res.data is  before we can do anything with it. 

  after consoling, we saw that it was the collection of data we want, but that won't always be the case so need to make extra sure it's what we want. 
  - next we want to save it; add
  appState.cars = res.data.map(c=> new Car(c))           // don't forget to double check path/layers here so you have correct path.

  - next we want to draw it.
  8. controller --> added drawCars function w/template, forEach, and setHTML()         // right now we're just dumping data to page ,d ont worry about how it looks.

  9. controller --> added listener to carsController class 
  appstate.on('cars', _drawCars)
  this.getCars()

  10. index --> pulled car template from last week's, changed id in appstate to listings instead of app. 
  11. model --> changed out our simple template to one from last week. updated string interpolation, made sure imgUrl capitalization matched. 
  thisID - we commented out because there is an id property in the api, so we no longer have to handle that, uncommented it, but made it say data.id instead of generate Id because WE are not the ones generating. 

  12. model --> added static getCarFormTemplate (copied from last week's)
  13. index --> added button for cars in header w/onclick
  14. controller --> added 
  function drawCarForm() {
    setHTML('listing-form', Car.GetCarFormTemplate())
  }
  then added _drawCarForm() to constructor   and refreshed       // on refresh, form loaded.

  15. controller --> added async createCar() {
    // see reference for all lines here
  }
    // checked errors, had 404 that was just a bad img link, just be aware of that for sandboxes
    // added new car to form and submitted, showed to console, we had imgUrl as just img, needed to update name in getCarFormTemplate, worked.

  16. controller --> in async createCar() added  try/catch w/await (see reference)
  17. service --> added const res = await axios.post('url', carData)        
  console.log('[POST CAR]', res.data )
  // got 400 error, went to network, checked out red error, in preview we saw database wanted hexcode for color instead of string, changed form to color type (gave colorpicker), this worked.
  - in post car in console it added the other properties like id, also added to page.

  18. service --> need to save to appstate
  appState.cars = [...appState.cars, new Car(res.data)]

19. Delete function
controller --> after async createCar, add
async removeCar(id) {
  trye {
    console.log('deleting', )
  } catch {
    pop.error(error.message)
    console.log()
  }
}

service --> add removeCar(id) {
  const res = await axios.delete('url/')   // added + id at end of url to show that there's a car at that location to delete. Don't forget the / at end of url or else the delete function won't work because the url and id aren't joined properly.
  console.log('[DELETE CAR]', res.data)
}
added Pop.toast(res.data, 'success')     // because console just said deleted value, so toast confirms to user. Actually gone on refresh

to get it to disappear w/o refresh, add to service
appState.cars = appState.cars.filter(c => c.id != id)        // keep everything that doesn't have that id

20. Adding edit button next to see details --> updated getListTemplate w/new button (look for mdi pencil)
- after checking button on page, added onclick to editCar
- added edit function between create and remove in controller w/async
- also added setActive to controller after we updated button to setActive, declared method to go to service, added activCar=null in appstate w/@type...notice .Car[null]
- then updated service setActive w/ let car = .....
controller --> added listener for activeCar in constructor, and then added drawCarForm private function
- added console log to activeCar

21. controller --> in draw car form, pass active car, then in model, added car to getCarFormTemplate.
- to onsubmit in above form, added ${car ? 'editCar()' : 'createCar()'}         // if theres a car, want line to end with appstate.car editCar, if there's not a car, let it end w/same but createCar

don't forget to prevent default for editCar
- added value input w/${car.make} to getCarFormTemplate (change each w/string interpolation)
- confusion here
- model -> added || '' to constructor data.id... etc (0 instead of '' for numbers)
- in getCarFormTemplate, added 
if (!car) {
  car = new Car...// see reference, empty bracket here signals there is no data to be passed in
}

22. controller --> 
await carsService.editCar(carData, id)

added const carData = getFormData...
then service --> in editCar, added:
const res = await axios.put('url')    //again added + id,carData to end of url, and again dont forget the /after the url so it connects to id correctly. 
console.log('[EDIT CAR]', res.data);         //now updates on refresh only so add. to service..
let index = appState.cars.findIndex(c => c.id == id)
appstate.cars.splice(index, 1, new Car(res.data))                 // we start at the index, splice out old and replace w/new
appState.emit('cars')                       //be sure to trigger listener here

23. added some pops to controller editCar, createCar, remove car (if(await pop.confirm('Are you sure...))

Reflection from Gegslist Async:
now that we have a few different pages, we don't want any of them to load as soon as the page loads. Instead we want the user to select which category they want. To do this, I had to move the this.getHouses and this.getCars from the constructor to the showHouses() and showCars() functions. I also had to move the _drawHouseForm and _drawCarForm from the constructor to the showHouses() and showCars(). Do the same with jobs!

Notes from jobs
1. added button for jobs in header first, then created model, controller, and service. 

Wednesday, Nov 16th, 2022

DnD Spellbook app notes:
* Will be talking to 2 different api's in one application. Pulling from one into another. 
* search through api for spells we like, then add to our app.
* With dense objects, we don't want to pull everything, api site gives us info on how to get more info on specific ones we want.
   - can use url and index to get more info (add index onto end of url to get specific info on one object)
   - the dnd api is full of objects with references to other parts of database. 
   - endpoint for bcw sandbox is dif than gregslists', must append our own name as user in the url (like bug one from video, no longer shared view).

Spellbook steps
1. created skeleton for controller, model, service, app.js, appstate
2. got rid of value controller, service
3. controller -> getDndSpells w/async and try/catch, pop error and a console.error
  - in try, add await dndSpellsService 
4. service -> updated getDndSpells() w/ 
const res = axios.get('URL')             //place dndapi here, but we're going to change up the axios here. axios allows you to create instances of the object we're utilizing so we can create whatever we want.
- above service class, added const dndApi = axios.create({
  baseURL: 'dndURL'            // then change the axios we placed in the service class, put dndApi.get instead, and move the url up to the axios.create() baseURL: 'URL'
})
* Can also place this reference to dndApi in separate service file. Just makes it cleaner
5. added console with res.data, did something in constructor
* Don't forget to add script tag for axios in index. 
6. service --> add
appstate 
7. spell.js --> added static ListTemplate, returned div w/spell.name interpolation.
- using static now instead of getter because they're not going to be spells yet. 
8. controller --> created function _drawspells w/ appstate, template, foreach, and setHTML.
* static = method that exists on class, don't need instance of class. If you want to use getter, you have to have an instance of a spell. in static, we dont have to have said new Spell anywhere yet in order to use. Doing this instead of map
9. controller --> added listener to spell class, should now be able to get data dump to page.
10. index --> added col-3 w/row (section api spells), updated id to api-spells.
11. model --> updated template w/html for spell name, then in controller updated setHTML to apispells

12. now we wnt to be able to click on any of these spells and draw the data for that spell from the api to page.
13. model --> added onclick to listTemplate html for getOneSpell('${spell.index}). NOTE - no this in a static list
14. controller --> added 
async getOneSpell(index)   w/try catch, await, errors, etc.
service --> add console in getOneSpell in class. should now be able to click spell on page and get its info in console.
15. service - updated getOneSpell w/const res = await dndApi.get(index) (the instance will take whatever is past into argument and upend it to url).
added res.data to console.log in service. if we check network, we should see updated url with that index at end.

16. since we want to save the data to sandbox, we need to loook at sandbox for formatting. once we know what we want, add to model constructor.
eg:
this.name = data.name etc.
17. appstate --> add activeSpell = null, updated import type
18. service --> need to save to appstate, add to getonespell:
appstate.activeSpell = new Spell{res.data}   // not mapping
console.log('new spell', )

* when we consoled, we had casting time and description undefined because the naming format was different between the 2 api's. To fix, we need to do both formatting types for these two, we'll come across these errors later. 

19. index --> added active spell section col-6 w/id="active-spell", and active template
* elevation-2 is built into custom scss as shadow class alternative. notice the col-12's and 4's with intention to stack or side to side for formatting. 
- copied, commented out, added to activetemplate in model. replaced data points w/string intepolation.
20. controller --> added draw active spell function w/ let spell, sethtml, added listener to constructor in controller.
21. now test, card appeared, click spel land it goes to card. we have some undefined items because of mismatched naming, so we updated the model constructor data.description to data.desc; data.damage ? data.damage.damage_type.name : '' (this is a ternary that evaluates if first thing is true or not, if it is, it accesses left side of color, if its false, it accesses right side of colon. you can chain ternary's here if you keep getting errors.)
   //not all spells have damage type name, so this says OR set to empty string. also updated casting time, materials, etc. test out different cards to make sure data is showing correctly. 
22. added sticky top to row in template so the card continued scrolling down as we searched the spells.

Now right side of page... these are the items we want to store in sandbox
23. created new controller and service for sandboxSpells. Since they are 2 dif endpoints, this is reccommended. 
made skeleton for these. 
24. app.js --> brought in sandbox controller
25. appstate --> added 
mySpells = [] and created new import type
26. controller --> created async getSpells w/try catch, poperror, console, await
27. service --> created new const sandBoxApi = axios.create w/baseURL
in getSpells, added const res = await w/console.log and async.
28. constructor --> added get spells constructor. 
29. service --> save to appstate w/
appstate.mySpells = res.data.map   //map here because we want to turn these into spells, taking from array this time
30. controller --> added drawMySpells() w/let spells =, template, foreach, set HTML
31. model --> added get MySpellTemplate() {}   w/ similar template to listtemplate, added listen to constructor in controller.
32. index -->added my spells section
- now check page, should see my spells on right, but can't select them to middle.

33. now changed myspelltemplate onclick to set and sandbox
controller --> created setOneSpell(id) function in class. added this.id to model constructor. 
34. service --> setOneSpell(id) w/let, appstate.activeSpell.
Now check page, should be able to select spell to center now. 
35. getting undefined again, so updated description and casting time || in model. 
updating damage was complicated... look at update. still didn't work, so dont worry about it. 
- with different naming conventions, prioritize the one you want on the left, and the other one on the right. it will check both still, but this way it checks the preferred one first. 

36. How do we actually get the spells to the spellbook?
model --> added button  to activeTemplate (add) w/onclick to app.sandboxSpellsController.addToSpellBook()
37. controller --> added 
async addToSpellBook(), declared method to service
38. service --> updated above spell in service w/let, const res w/post('', spell), and consolelog.
39. got error, checked out network tab, spells, the api sends desc back in array which is messing us up. What do do? go to model desc and add join('/n') to data.desc.
- now it should show in console and when we refresh it adds to our page.
40. service --> to addtospellbook add
appstate.mySpells = [...appState.mySpells, new Spell(res.data]
41. added spell-description to index as class and updated css styling just for better formatting of card, also added scrollable-y. this kept card same height but added scrollable to description only. 
42. should now be able to add spell to spellbook without refresh.

43. How do we delete? If we click spell in spellbook, want it to have remove button.
updated index w/removeSpell.... look closely at this
controller --> asyn w/removeSpellFromBook
****windows key and v opens up copy history!!!!
declare method to service for same function. For delete, we add the id to end of url
const spell = appstate...
const res = sandboxapi.delete(spell.id)
console, but don't forget to add async await or it won't wait for response to come back. 
after checking, added appstate.myspells = appstate.myspells.filter....

44. need to distinguish between spells that come from right vs.left
 model --> added getComputeButtons()
 if(this.id != undefined) {
  return `remove button`}
  else {return `add button `}
 }

  also added this.compute buttons to activetemplate
  * had to update this.id line in model because both api's include id's. 
  added appstate = null to try to reset form after its been deleted, but got error.
  updated controller drawActive spell function with if else that added PlaceholderTemplate, then added placeholderTemplate to model. returned h3 tag w/please select a spell from either list.
   
  45. in dndController --> added something so that on page load it returns the please select message. 

  46. Now we want to mark spells as prepared spells. 
  model --> myspellTemplate, added input type="checkbox" wrapped in another div. This will be what we click if it's determined. As of now if we refresh, it doesnt do anything. NEed to communcate to database that it is prepared.
  model --> next to checkbox added onchange= w/id because it's happening in side list. 
  controller --> added prepareSpell w/asyn await, try catch, error messages, etc. pass id in this one. 
  service --> declare prepareSpell here w/
  let selectedSpell = ...
  console.log, then test. Should see it preparing in console.
  service --> still in prepareSpell, now we want add
  selectedSpell.prepared = true           // this tells database that info has changed, add put to tell it to change to updated data. 
  const res = await sandBoxApi.put(id, selectedSpell)

  47. model --> find checkbox, add ${this.prepared ? 'checked' : ''}

  48. now to be able to uncheck a checkbox.
  service --> in preparespell, add
  if(selectedSpell.prepared == false....) else...
  did a longer way first, then showed shorter way to write it. this only works w/bools!

  49. index added w/spell-count to my spells section
  controller --> added to drawMySpells (see note on spell count)
  * this line cuts the list of spells down to only prepared ones, and finds the length of these. then added sethtml
  - added appstate.emit to trigger listener in service.  
  (added if else statement...look at controller function drawmyspells)


Thursday, November 17th, 2022
NASA Picture of the Day API
* For this api we need to supply api key, entered info and it gave us key and url w/key. click link and it will take us to our api. 
* showed example of adding more keys onto url (see documentation for formatting on the original api)

Lecture goal: build app that will allow us to query nasa api, render pic to page, and be able to update the query.
- want to render pic as background info and render the other details. Add data picker somewhere, ability to save pictures. 

Steps:
1. She started w/sending first get request. Made axios service first
  set up w/export = axios.create, baseurl (leave api key off for now).
  - added timeout: 8000, in case it takes awhile to come through. 
  - added object for params: {api_key: entered key here in string}            //supply name of params and the value (read docs to see formatting for these otherwise it won't work). now when we send a request it will hit the api and then tack the key onto the end of url

2. Created NasaController and NasaService, set up skeletons. 
Controller -->consoled constructor to make sure its hooked up.
hook up app.js (register controller)

3. controller --> add
async getApod   w/try catch, errors, await, declare getApod to open in service
service --> created getApod w/async await, const res, log the res
index --> link axios script
conroller --> this.getApod() in constructor. 
** strting w/service calls first helps us to see what data we're working with BEFORE we actually build our model out. 
- viewed network tab here to see how url has been updated

4. model --> NasaApod.js. built out constructor data after viewing data in console to see what we want. 
* left hand side w/this.date, is what we want it to be called on our end, just make sure right hand side matches the apod api.

5.created appstate --> for now we just want one, so just added apod = null, updated type import
service --> added appstate.apod = new NasaApod(res.data)       // creates new instance with res.data
then console the appstate.apod           //helps us check if it gives us what we want.

6. controller --> outside class, add "hidden" function for drawpicture w/ let apod, document.queryselector('body')style.backgroundImage = 'url(${apod.imgUrl})'   //this targets entire body w/ an inline style
in constructor --> add listener for draw
- check app, should now load background image. 

7. want to restyle background 
css --> styled body

8. Now we want to render rest of details to page. 
index --> hard code how we want this section to look like.
- added d-flex to container which helps the col-6 span the length of the container.
- once happy w/layout, added apodtemplate stub, copy, comment out, add to detailTemplate in model, updated w/string interpolation.
- don't forget id in row above apod temp in html
- updated draw function w/setHTML

9. want to add copyright to the page IF it has one. 
model --> updated w/this.copyright = data.copyright || 'NASA'
- index --> updated footer w/copyright icon and nasa
- add id to nasa p tag, copy, comment out, updated drawpicture() in controller w/setText (no html here)

10. model --> updated detailsTemplate w/apod-body and header
css --> target apod-body w/visibility amd opacity(this hides the html from user, but its still in dom, display none on the otherhand removes from dom)
- .apod-header:hover+.apod-body {
  visibiity and opacity // this makes the body appear when you hover over header
}
- updated apod-body w/transtion (3s = dramatic AF, love it)

11. adding date selector and favorite buttons
index--> updated header w/col-6
- date = input date
- when we select date we want it to change to apod from that date
controller --> add getApodByDate function in class w/try catch, async/await, console.
- add onclick to date input in html (app.nasaController.getApodByDate()), added space-date name and id
- onclick here just consoled everytime you click the datepicker, so instead let's change it to onchange because we want it to update onchange not just when clicked. should see picking date in console each time you change date now.

12. contoller --> update getApodbyDate -- want to target the event that happened.
add let dateInput = window.event.target
console.log(dataInput, 'comment')
* this gave us whole input in console, so add .value to dateInput so that I'm only getting the value of the change input. 
should console correctly now. 

<!-- 13.   file - preferences - settings - null check turn OFF to get rid of question marks. -->

14. Want to send date to nasa apod
controller --> added await nasaServie.getApodByDate.dateInput.value to getApodByDate function
service --> declare method, pass date in param, start w/console('comment', date)  --makes sure this is the correct data in console
- once we know we're getting data we're expecting, now we need to send request to api, but we need way to supply the date. To do so, we need to append that data to end of api url
service --> getapodbydate(), add:
async/await w/const res = await NasaApi.get('apod?date=' + date)
* had to go to axios service and removed apod, then in nasa service, had to pass 'apod' into NAsaApi.get('apod')
- log the res in getapodbydate.
- now add the res.data to appstate.apod = new NasaApod(res.data)
console log, should now update data on the page w/image and everything

15. Want to display date to actually show date. 
controller --> update getApod() by setting value for date picker, and want to set a "max date" to prevent user from selecting future date. 
added let datePicker = document.getElementById('space-date')
log datepicker, once you've seen it work, remove that log
- add let ISODate = new Date().toISOString(), then log this
- then add let date = ISODate.substring(0, 10), then log the date.  //the 0, 10 slices off extra values we don't want. 0 is where we start and 10 is how many characters we want. 
- next set datepicker value = date AND
- datePicker.max = date

16. Now we want to be able to favorites - off-canvas to show all the ones we've favorited.
- look at apod model in bcw sandbox, we want to make apods for jsam
- create new model (FavApod.js), make skeleton w/this.id   this.imgUrl   this.date     this.user   (make sure naming conventions match our sandbox)

** on break she updated some css styling

17. add new FavsController and FavsService, make skeleton, then register controller in app.js

18. constructor --> log to make sure it's linked
19. index --> add favs template hard code w/col-6 buttons (placed below footer since it will not show on page load)
- when we click notebook we want offcanvas to load with our list of favorites. went to bootstrap for how to
- copied code for offcanvas into index w/id=favs-offcanvas, make sure you have all the toggles and arias. then test app
- once it works, update the template for what we want it to look like.
  * added placeholder image, delete icon, date (see stub w/fav template), copy, comment out, add to FavApod model w/get FavoriteTemplate, update string interpolation.

20. now to assign favorites. we need to send post request to jsam apod
- find heart button in html, add onclick="app.favsController.createFav()"
controller --> create createFav() w/try catch, async/await, and console.log
- make sure buttons linked correctly on app.
- once you see it's linked, replace log with await, declare method to service.
service --> createFav()... need to update axiosService w/instance for sandboxApi w/baseURL, timeout
-favService --> await sandboxApi.post()                // we are trying to post to our sandbox so use post. updated () w/'api/jsam/apods', appstate.apod, then log the data.
- make sure to link appstate to favsservice. 

21. when we hit the fav button, we're just wanting to save it to a spot in memory somewhere. 
favscontroller --> create method to getFavs() w/async await, try catch, error.
   first console the favs (before the await), add this.getFavs() to constructor
   - after checking console, update console to await, declare method to service
   - service --> const res = await sandboxApi....
   - w/console should now see the data.
  
  appstate --> create area to store these favs = [], update import type
  service --> add appstate.favs = res.data.map(f => new FavApod(f)), then console the appstate.favs, 'appstate favs'
  - moved appstate.favs = res.data.map to getFavs function
  - service in createFavs --> add appstate.favs = [...appstate.favs, new FavApod(res.data)], then console. 

  22. now to get our favs to actuallly draw to offcanvas
  - controller --> create _drawFavs() w/let template, appstate.favs.forEach(f => template += f.FavoriteTemplate)
  index --> find offcanvas, add favorites id
  controller --> update setHTML for drawFavs, then add listener to constructor. should now show in offcanvas.

23. now that they're in offcanvas, we want to be able to click a favorite and it renders to page.
 - when we click image, want to call getApodByDate ()
 --> model fav template, add onclik to image tag, supply w/string interpolation this.date (AS STRING)
 - got error, checked out getApodByDate in controller, needed to update param w/date, also added:
  if(!date) {
    let dateInput...
  } else {
    await nasaService.getApodByDate(date)
  }

  24. When we update page w/a fav, now we want the date to update to that img's date in datepicker.
  controller --> made _setDate(date) w/let datepicker = document.getElementById
  updated getApodByDate w/  

  25. How to remove one from favs
  view favApod model vs other apod model, notice one has id, other doesnt. 
  - updated trashcan w/slectable and onclick in favorite template, supply the deleteFav w/string interpolation for this.id
  controller --> write deleteFav(favId), log to be sure it works. 
  - once we see it works, add try catch, async await. declare method to service.
  service --> const res = await sandboxApi.delete('api/jsam/apods/' + favId)  DONT FORGET / ON END OF URL!!!!
  - should now delete on refresh. 
  service --> appstate.favs = appstate.favs.filter(f => f.id != favId)     (OR let filtered array method...view commented out stuff here to see both options, they perform the same. one just aliases out the filtered array)

  26. what if we delete one we diddn't mean to?
  controller --> add pop.confirm to have user confirm

  27. extra fun stuff - added off-canvas class to offcanvas, added new css class w/ background-color, backdrop-filter (blur), 
  nasa controller --> added bootstrap.offcanvas to get offcanvas to disappear onclick of fav.
