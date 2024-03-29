# MVC

Monday, November 28th, 2022

Isle of Node
* Node is a runner, it takes the text and turns it into functions/processes and then actually runs it. 
* Server - code sitting and waiting for us, how to activate it? http requests (get request, post request, etc).
    - server sees the request and runs the code to grab the api.
* castle = express.js, 
*island = node.js
* knight = http request that's made.
* response (res), knight coming back out of castle. 
* castle is full of hallways (dogs, cats) - knight goes on quest to get one of these, but there's another hallway behind door, which is our catsController. in this hallway there are doors for different requests types (get, put, post, delete). For this analogy we they go to get door, then through portal to come back out castle.
* goblin that goes to get data from database monster is called mongoose
Mongoose - orm? - the guy who takes our request for info and sorts it out of database and brings it to us. 
* middleware - a checkpoint in the hallways the knight has to pass through (authorization at front of castle which can be api key, barer token?) (error handler - middleware on the way back out, in case data not found, he's given a status based on type of error) (helmet - in network there are headers, helmet is middleware that men in blacks the knight on the way out - makes you forget what they saw/the castle, that way they don't leave with info that's not for them.) (body-parser - axios turns things into string, so having a body-parser takes the string and turns it back into object).


Building first server
- created new file for burgerShack using node-server-auth0, has index.js - we won't add anything here, but can think of like our app.js
- in main.js we see express. express is a package brought in from npm (node package manager), same with vue. 
- dependencies are listed in package.json (can see express, helmet, mongoose, socket, etc here)
- linting - used to format code to a practice setup by certain users. you may see somethings change to match the rules created (e.g. single quotes to double, double equal to triple, etc.)
- gitignore - holds the files we don't want to push to github
-package.json = what you need to install, packagelock.json = ones you actually installed. 
- notice client and server files (client side (front-end) is everything we've been doing so far, we're not going to worry about for awhile). server side is our focus for now (back-end).

Server File -
valuesController - works similarly to front end one, still where our code kicks off, where our knight enters castle.
  - super = name on door (cats, dogs, etc)
  - get use and post in the this.router = doors inside the super door/hallway
  - the .use here is a piece of middleware, place it where you want checkpoint before it moves onto next step. 
  - in getAll async, it has req, res, next (req = requirements, res = response, next = the next thing you want it to do, sends knight back into hallway and keep moving to next step).

  - how to load webpage - new way!
  * was getting error, in main.js, need to comment out createServer (look for comment in pushed code), or comment out piece from startup.js.
  - there's another piece to comment out in main.js so it doesnt' try to load server.

Want to get random number back
- to url added /api/values to localhost3000 to be able to see data. 
- updated the values to best burger
- must respin server each time or you won't see any changes to api. make sure the route laid out in super matches the route in url, otherwise will get errors. 
* super is the constructor within th constructor - notice the extends baseController - this is inheritance, which means the ValuesController does what is in BaseController as well as what you put in ValuesController. BaseController is responsible for setting up first door in hallway, allows us to not have to redo this data each time. this.mount creates first door in hallway, uses whatever is based into super as the label for the door. 

Create our own controller
* created BurgersController.js
1. export class BurgersController extends BaseController w/constructor that has super('api/burgers), this.router, .get('', this.getAll)
2. added async function for getAll()...will always take in 3 params in same order
async getAll(request, response, next) {
  return response.send({message: 'here for burgs'})
}
saved, updated url, and respun to get the message
3. added try/catch and moved return to try. in catch, put next(error)

4. created BurgersService.js
class BurgersService {
getAll() {

}
}
export const burgersService = new BurgersService(); (notice it's written same way we're use to)

5. back in controller --> change try to const burgers = await burgersService.getAll()
return response.send(burgers)
6. dbContext --> commented out values and account and added ...
Burgers = [
  {name: 'shoe', description: '.....', price: 20},
  {name: 'the bob', description: '...', price: 25}
]
** this is our fake database for now, until we use a real one later...

7. services --> to getAll() add...
let burgers = await dbContext.Burgers
return burgers    (we'll see a lot of returns now that we're using servers a lot). this return will send result of what ever we asked for in controller.
respin, refresh, and should now see the array of data from what we put in dbContext.

8. controller --> updated this.router w/ .get('one', this.getOne) and added async for getOne() w/try catch
updated .get('/:name', this.getOne)  and put a return response.send(request.params) in the try. 
- respin, update url and should get name of burger

9. updated try to const burger = await burgersService.getOne(request.params.name)
return response.send(burger)
10. service --> added async getOne(burgerName) {
  const burger = await dbContext.Burgers.find(b => b.name == burgerName)
  return burger
}
respin, update url, nothing happened. View comment above if (!burger), respin and refresh, should now see error. 

11. added id's to dbContext.js array of burgers, updated controller to /:id instead of name. and request.params.id, also updated service param of burgerName to burgerId
respin, updateUrl WITH THE ACTUAL ID #, NOT JUST ID WORD!, refresh
- enter in each id and you should see all burgers listed in burger []

after lunch
13. controller =--> added .post('', create), then added...
async create(request, response, next) w/try catch w/ 
const newBurger = await burgersService.create(request.body)
return response.send(newBurger) 

14. service --> added async create(newBurger) {
  await dbContext.Burgers.push(newBurger)
  return newBurger
}

New tool - postman allows you to make network request from uri

Ways to debug w/server...debugger won't work, console.log works sometimes, but often gets eaten...
1. logger - (LOWERCASE L!!) from utils.logger
logger.log(newBurger) (went backto main.js and uncommented out startup line... look for note)
2. breakpoints - little red dot that shows up to left of number in file, click and it should make it a breakpoint, can check in vscode debug

15. service --> added newBurger.id = dbContext.Burgers.length + 1 (only doing this for now until we are using database)

16. delete function. 
added .delete('/:id', this.remove) to controller
then make async remove(req, res, next) w/try catch function in controller.
try has const message = await burgersService.remove(request.params.id)
return res.send(message)

17. service -->
add async remove(burgerId) {
  const burger = await this.getOne(burgerId) // use this.getOne instead of find burger like we did earlier...does the same thing, but now reuses code we already wrote that has error handler in it already. 
  let index = dbContext.Burgers.indexOf(burger)
  dbContext.Burgers.splice(index, 1)
  return `${burger.name} removed. she gone`
}

My turn to make Burger Shack...things to remember.
1. created file w/node, went to main.js and commented out dbconnect.connect(), also went to startup.js and commented out auth0provider.configure and domain/clientId/audience.
- this gets rid of error handling while we're not using an actual database. 
- when checking each id on api, be sure to enter the number of the actual id, not just word id.
- need help figuring out postman, nothing would show in my response. 



Tuesday, Nov 29th, 2022
- After creating auth0 and mongodb files, made gregslistAPI folder. 
1. made car. js model with export const CarSchema and object in new Schema with make, model, year, etc. Use value.js as example, 
  - note: can put different properties in here like required, default, max/min length, etc.
  -timestamp can be added here and then we don't have to worry about handling timestamps on our own.
  -virtuals - more complicated getters, can be accessed on the model but doesnt actually exist on the model. 
  - not worrying about creatorId today.

- now that we've created model/schema, we need to tell database we're going to access it. 

2. dbContext --> added Cars = mongoose.model('Car', CarSchema)   in the string can put what we want, but it's what will show on database. Look at values = for example. 
- this tells api to grab from that db collection

3. Make Controller and service with skeleton (view examples to help). controller --> added super, this.router, .get, and async getall w/try catch. 
service --> added getAll function w/const and return cars

4. opened postman here: collection --> get request w/localhost
]- wasn't working, but later realized needed to download desktop version, then it worked.

5. controller --> added .post in this.router and create function below getAll w/try catch, etc.
6. service - declared create method in service w/const newCar = await dbContext.Cars.create(carData)
return newCar

7. in postman, made new request for http://localhost:3000/api/cars by putting the schema with an actual cardata in it. To do this...
went to body and changed it to raw and json
- added object for car w/make, model, year, etc. Plugged in data for actual car. make sure it's a post request here, then send. should see all the data to the right, has some new info (timestamps, id, etc).
- then went back to get request and hit send, 

8. controller --> added .delete('/:id', this.remove) in this.router, added async remove w/try catch
service --> declared remove method to service. w/ const car = await dbContext.Cars.findByIdAndRemove(carId)
return 'car gone'
- did delete request in postman after respin and refresh, 
- didn't quite work so we updated the remove function in service and continued checking postman to delete.

9. controller -- added update method
service --> declared method here.
- went to postman, made put request with id appended to url, copied over schema from previous request, make sure its raw json, updated info and send. 
updated update method in service with const original and original.make, etc. 
updated AGAIN with ternaries:
 original.make = carData.make ? carData.make : original.make (basically if the user gave a carData.make, put that, otherwise, keep the original.make that was entered. )

 10. controller --> const query = req.query, updated carsService.getall(query)
 - allows us to append url in postman with ?model=Protege in order to search for specific car, or any with that model. It passes that query into our find in getAll()

 11. service --> appended a sort to end of query (whoops above step should be in service not controller...i think)
 - tried a few different things here, like price, updated at, make, etc. 
 - can do negative (-price) 


 Wednesday, November 30th, 2022
 PingPong
 Goals - authorization layer that forces users to login to use parts of api, access control, data relationships (is focus).

 1. Started w/classes. created model Class.js. set up skeleton w/ new schema
 cohort, teamName, graduated, 
 added timestamp w/virtuals (will use today, so don't forget this here!)
 2. made skeleton of controller and service, created dbContext and added Classes (this is reflected in MongoDb in how its saved)
controller --> added get to constructor, created getAll(), declared method to services and completed getAll(). Could add query here, but don't have to. Remember this allows us to append url w/specific search date (e.g. ?cohort=LateFall22&graduated=false).
* save, spin up, check postman
** new info on postman - left side bar with collections allows you to save requests so you don't have to make new ones all the time. 
** variables in each collection - go to main page for that collection, click variables, add local host url to initial variable, then in actual get request page, can do {{baseUrl}} with the rest of url and it puts the localhost there automatically.

3. don't want user to be able to create classes unless logged in and authorized user. this is wehre things get different....
MIDDLEWARE - use method
- added .use((req, res, next) => logger.log(req)  to constructor
- then added .post request to constructor (normal), created function in controller, but did throw new BadRequest in try
*save, spin, made create class for post in postman, added schema to raw json w/actual data
- postman waited forever because we never told the .use to return anything. need to add a next to the middleware. after log(req); next(). now it works! we did get console log in vscode
- added private function in controller for _middleWareDemo, which does same as above.
-added another .use(Auth0Provider.getAuthorizedUserInfo) see note. this mainly looks at incoming request/token, if there is token on that request, it goes to auth0.com and varifies person, if no token, kicks user out w/401, etc. 
- how do we get network request to have token? 
- example: catups, looked at network, if you look at header you'll see token.
- in env, copied auth stuff, went to client, found env.js, and updated the domain, audience, and clientId with the info from our env. this connects both front and back end to auth0.
- now if you go to localhost3000, you will see the auth0 settings and can login. now we see token in network, copied value of toke, went to postman and found auth tab, chose bearer toke, dropped the copied token in there. 
- better way is to go to top level of pongping and authorization and put token info there. can also go to variables, amke new one for token, and then back in authorization we can put in {{auth}} just like we did with baseUrl

4. added return to create with req.userInfo

5. in class.js --> added coachId, this creates our first data relationship. The class is now the child of the coach. 
- include ref that references the account schema in our dbcontext. This is a one-to-many relationship.

6. controller --> updated create() w/ req.body.coachId = req.userInfo.id. this creates a coach id for the user so they don't have to do that.
added const newClass and return res.send(newClass)
-declared create() to service with classData, const newClass, return. 
* respin, send post request and you'll now see coach id, which matches what we see in network. 

7. class.js --> introduce first virtual. added a special virtual: 
ClassSchema.virtual('coach', {
  localField: 'coachId',
  foreignField: "_id",
  ref: 'Account',
  justOne: true   (w/o just one, it will make the coach data an array...which we want sometimes, but not here)
})

8. service -> added .populate('coach') to getAll (virtual won't come back without this)
* respin, do get request, now should see coach data. 

9. added .delete to constructor, delete function (similar to what we've been doing but slight spin)
- in await classesService.remove(req.params.id, req.userInfo) // rule that only coach of team can delete it. 
- declare method to service w/ const classToRemov and throw new BadRequest, and another check:
if(userInfo != classToRemove.coachId.toString()) throw new Forbidden('not your class')            // if userInfo is not the coachId don't let them delete.
-if you do get past above line, we want to allow them to remove
await classToRemove.remove()
return `the ${classToRemove.teamName} was disbanded`
* respin, sent delete request for specific id. 

* showed example of trying to delete class from different coachId, got not your class error. 
* putting the error code first helps keep code clean and uses "fail first" method. throws in checks first and then continues. 
- if you have other checks you want to do, try to do single line checks with if statements so that it's more simple for the user. we could have combined the error checks above, but then we don't always know what the error is because it's too complicated. 

10. created new model --> Player.js, set up skeleton w/name, wins, losses. timestamp
- want to be able to tie player to specific class, so also added classId: {type: objectId}. put const objectId = Schema.Types.ObjectId above schema to define objectId
- updated dbcontext. 
- created players controller and service skeletons.

11. controlelr --> in router, have .get, .use, .post.
-.get(Auth0Provider.getAuthorizedUserInfo)
- create getAll method, create method
** in player.js added accountId w/ref and classId ref
controller --> declare getAll and create to service, complete these functions, kept simple for now. 
* respin, postman --> new post request w/name and classId, grabbed classId from get request, and sent. should now see player data.
- added afew more players here too

12. player.js --> added PlayerSchema.virtual('class', {
  localField: 'classId',
  ref: 'Class',
  foreignField: '_id'
  justOne: true
})
serivce --> added populate to getAll
*respin, send request to players, should now see french oslos in data
* back in service, updated .populate to include comment with 'teamName' so we only get the data we want without all the extras.

13. added await player.populate to create() in service

14. controller --> added update(), declared to service
- added badrequest and forbidden checks (make sure params match what we have in controller)
- what if we want player AND coach to be able to update info? how to get coaches info...add
const playerClass = await dbContext.Classes.findById(player.classId)
- then updated forbidden check with && playerClass.coachId.toString() !== userId
- also added:
player.wins = playerData.wins != undefined ? playerData.wins : playerWins (same w/player losses) see note for reminder about what this does/why we write this way. 
 await player.save()
 player.populate('class', populateFields)        //notice, populate Fields is something he abstracted out, but not necessary to do. 
 return player
 * respin, write new update player request

 15. making matches - dif type of relationship, match can have many players and many players can have many matches, so now we have many to many, whereas player could only have one class(team), so that was a dif relationship.
-match.js w/skeleton (homePlayerId, awayPlayerId, winnerId, )
- don't forget const ObjectId = Schema.Types.ObjectId above new Schema
- created controller and service w/skeletons for create (not getAll)
* respin, postman send post request, grabbed id from koko for home, miles for away, and koko for winner. then sent. 

16. Match.js --> added
MatchSchema .virtual for homePlayer and awayPlayer and winner

17. matches service --> added populate to create()
* respin, send post request. should now see home,away,winner.

18. want to be able to see players matches
players controller --> added .get('/:id/matches') to router, then added getPlayerMatches() under getAll(). 
- notice we connect to matchesService, not playersService here
- declare to service, specified playerId
- in .find() focused just on matches where they are winner for now. 
* respin, get request w/ :/id/matches

planet proj
1:1 (one planet can only be in one galaxy)
1-many (galaxy can have many planets )
many-many (species to colony) colony is like the match, ties species together and planet together. there can be many species on many planets. A species can colonize many planets and many species can colonize one planet. 

* focus on create and get to focus on practicing relationships


Thursday, December 1st, 2022
Auth0 with BirdBrain

Set-Up
1. created birdBrain project
2. env --> upddate auth properties, update name of project in connection string.
3. env.js in client - update domain audience and client id too.
4. once those are in, spin it up! go to localhost3000, should now be able to sign in and see auth settings filled in. 

Code time: Jerms doing the back-end, Sam doing the front-end
1. Bird.js for bird schema
  - showed enum for size
  -peeperId type: Schema.Types.ObjectId      (so we need const above schema)   also included ref: 'Account' so that it references the account for that data. 
  * good idea to add maxlength to properties to limit what user can do. don't want them adding super long string and then have to store that in our db. 
2. registed bird schema in db context
3. created birds controller w/skeleton, getAll()
  -set up to be able to query
4. created birds service w/skeleton, getAll()
5. respin, postman --> new get request for birds, should get empty array
6. controller --> added getBirdById(), declared to service   // even if you don't use this, it's a helpful ability to have 
7. controller --> added .post for createBird, declared to service
** respin, postman --> new post request w/raw JSON (this is the req.body)
- added schema for bird w/actual data. No peeperId here because we don't want user to be able to create one or use someone elses, this should just come from app.
- had to add a .use to controller before .post so that anything after the auth0 they have to be logged in first. 

8. localhost --> network, toke, --> copy token, post in postman in authorization, put the bearer token here, then sent request. 
- Got error that peeperId is needed, so went back to controller and added to createBird   (req.body.peeperId = req.userInfo.id)
-Without this, we can't proceed cuz it needs login info to continue. 
9. postman --> made new get request to test getting one bird by id, don't forget to append id to url

--NOW switching dev, so he committed his changes, slacked sam the github link--
-- after bringing down changes, don't forget to add env file at top level of proj (not in client, server, etc). Need to get env info from whoever started project.
- in terminal ran npm i to find the env file, then spun up.

10. client --> made client side model, for bird. 
11. created appstate with birds = [], updated value import
12. created birdsController and service with skeleton.
13. app.js --> register controller
14. localhost --> console, should see hello from controller
15. controller --> created getBirds(), declare to service.
16. axios service --> checked name of axios instance (server in this case)
17. birds service --> to getBirds( added const res = await server.get('api/birds)), then log the res, controller, be sure to invoke the function, check console on localhost
18. should now see birds in console, also check network tab.
19. now wthat we're getting birds, save to appstate, so in service add appstate.birds = res.data.map(b => new Bird(b)), console to check what appstate looks like. should see any birds added now.

20. index --> updated main with how we want things formatted. 
21. main scss --> added styling for peeper-img. when checking app, didn't update styling, so in terminal sent npm i bootstrap, then do npm run sass, once both are there should now have compiled and app should load correctly.
22. updated styling in index, once basics done, copied bird card, commented out, moved to birdModel, update w/string interpolation.
23. discussed putting in something to populate user pic, but that's something we'll need to handle with a virtual in the back end, so we're leaving it for now. 

24. controller --> draw(), for setHTML, need id, so went to index and added id="birds". add appstate.on in birdsController constructor.
- refresh app page and should now see any birds in collection from back end.

25. Img fluid class --> removed from card template, went to scss main and added .bird-img{
  width: 100%,      // tells it to fit 100% of parent container
  height: 45vh,     // don't want auto here, want to set same height for all so they are the same.
  object-fit: cover,
  }
  ** see note in scss to see how to get sass files to load. 

26. Now let's draw birds from the page!
- grabbed modal from bootstrap which will hold bird details
index --> put under footer, added bird-modal id, changed to modal-lg, want to add button that stays at bottom of page but scrolls with us.
- in main, made new row, added emoji button w/sticky bottom
- add data-bs-target, etc to make clickable. should now get modal pop up when clicked.

27. added select input dropdown to handle enum (got from w3bschools).
- checked server model for size options, and updated dropdown form to correspond. 
-also updated checkbox with canFly, took out location section. 

28. bird model --> created static GetBirdForm, in index added id=birdModalContent to modal-content line, then grabbed entire form from modal header down, commented out, moved to model. 

29. index --> added onclick to add bird button, added getBirdForm( ) to controller. 
- this method will handle drawing form to the modal, set the html there
- added on submit to form tag in static getBirdForm template
- controller --> added peepBird()
- tried to add new bird to page using modal, should see data added to modal.

30. declared peepBird() to service, tried adding new bird again. got error because canFly says on instead of true, updated controller --> peepBird() w/if (form.Data.canFly == 'on) {
  formData.canFly = true {
    else {
      formData.canFly = false
    }
  }
}      // hmmmmm idk what happened....don't do bool.

* don't forget appstate w/push and emit so it draws on submit, not refresh. 
* removed commented out modal from index and put in blank modal (modal-body)
* grab from modal header to footer, comment out, put in model as ActiveBirdTemplate.
* in cardTemplate, added data-bs-toggle=modal and databstarget=..., onclick="app.birdsController.setActive(id), added this.id to model

31. controller --> made setActive(birdId), declare to service.
32. updated appstate w/activeBird = null, updated type
33. controller --> made private function for drawActive(), updated constructor w/listener. 
34. updated active template in model w/string interpolation.

Switched back to Jerms
35. checked bottom left of vs code, see number of changes to commit, be sure to do that when switching coder.

36. Now we want to get details of peeper onto object we want to send back. Need to do virtual to do this.
In server - bird.js add BirdSchema.virtual...
- virtual won't exist on schema until we populate in service (see createBird, added await bird.populate... specify details you want from it, AND populate in getAllBirds (can tack .populate onto existing const birds))
* respin, postman, do get request, should now see birds with peeperId populated to object. 

37. Now want others to be able to click eyeballs to say they've also seen that bird. This creates many to many relationship.
-created Creep.js schema. need 2 id's to keep track of relationship (bird id (Schema.Types.ObjectId, and ref: 'Bird') and creeper id(Schema.Types.ObjectId, but ref is Account))
- also want to get info of who also SAW bird (creeper). added timestamp w/virtuals: true, then added:
CreepSchema.virtual('creep', {
  etc
})
38. created creepsController and Service w/skeleton, including .post to be able to create. (becomeCreep), also added .use before .post because user needs to be logged in to do this. 
- need to grab userId, so also add req.body.creepId = req.userInfo.id   // this sets the creepId to the userInfo id of whoever is logged in.
- declare to service
- don't forget to add creeps to dbContext
* respin, new Create Creep request, in body need to include birdId only, grab one from get bird request. this set the birdId but not the creepId because we didn't populate yet. Go populate in creepService becomeCreep.
* respin and send request again, now see lots of info for creep. What if we don't want all this info? Update the populate w/what we want. 

39. Want to be able to unpeep...
creepSErvice --> made findOneCreep(body), then updated  becomeCreep w/const foudnCreep.... w/if statement to say if it exists already, remove it.
- need to make removeCreep() first.... this was complicated 😕 decided not to worry about this.

40. Creep.js --> CreepSchema.index({creepId: 1, birdId: 1}, {unique: true})    // this says if using same bird id and creep id, it will throw error. This prevents user from "seeing" same bird multiple times. This is our fix to the earlier remove attempt. 
* respin, send get request once, should see the data, send request again and should now get error which shows our code worked and prevents user from seeing one bird more than once. 

41. BirdController --> added new .get('/:birdId/creeps', this.getCreepsByBirdId)    // url needs to be dif from other get request otherwise it won't know where to send it.
-create getCreepsByBirdId(), awaited creepsService since we're getting creeps 
declare to service and create.
* respin, do new get request for Creep, checked ones with creeps and without. without should be empty array.
- add populate to getCreepsByBirdId, should now see virtual in data in postman

42. bird model --> added BirdSchema.virtual for creepCount. update birdsSErvice wcreepCount in getallbirds and createbird (in the populate).
back in bird schema --> add count: true to virtual so it only counts how many creeps. 

Jerms finished back end, push to github and sam will push up her changes and pull down jerms changes. 

43. want to get user pic to change. on app, checked network birds to see updated info w/virtuals.
bird model --> updated model w/creeper, string interpolation for peeperCount and peeper.picture.
* refresh and user pic and creepcount should be updated. 

44. when you click modal for a bird, it becomes active, want to pull creepdata
in client, made creep model --> then made creepsController and Service.
- made getCreeps() 
* check getCreeps w/string interpolation in url in service. add listener to constructor, then refresh and check network/console. 

45. updated creep model with some flattened objects (e.g. this.name = data.name.creep)
back in bird model --> updated activeTemplate w/new div for creep deets (look for id=creeps)
- moved this to creep model (just line with img), string interpolated, 
- creepsController --> created drawCreeps(), added listener to constructor w/creepers NOT activeBird, activeBird caused lag. 

46. Want to be able to click eye button to show creeping. 
client bird.js --> add onclick to cardTemplate creepcount button. Calling to creepsController, not bird. 
creepsController --> added becomeCreep(), declare to service, 
- in await server, be sure to put birdId in {} so it's recognized as object, otherwise won't know it's a bird id. 
added const bird = appstate...
bird.creepCount++
appState.emit('birds')


HACKATHON Prep

