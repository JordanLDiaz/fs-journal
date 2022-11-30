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