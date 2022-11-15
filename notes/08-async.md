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
3. model -> added cars = []
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
  const res = await axios.delete('url')   // added + id at end of url to show that there's a car at that location to delete
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
const res = await axios.put('url')    //again added + id,carData to end of url 
console.log('[EDIT CAR]', res.data);         //now updates on refresh only so add. to service..
let index = appState.cars.findIndex(c => c.id == id)
appstate.cars.splice(index, 1, new Car(res.data))                 // we start at the index, splice out old and replace w/new
appState.emit('cars')                       //be sure to trigger listener here

23. added some pops to controller editCar, createCar, remove car (if(await pop.confirm('Are you sure...))