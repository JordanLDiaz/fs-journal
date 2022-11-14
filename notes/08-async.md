# MVC

Monday, November 14th - HP Trading Cards Lecture

Asynchronous code - helps to use multiple platforms to run code together asynchronously.
- Because we are working with things outside of our control (eg waiting to get functions/things from other places), we don't have control over that so we need to have asynchronous functionality. (idk...he explained better)
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
