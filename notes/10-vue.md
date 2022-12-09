# MVC

Monday, December 5th, 2022

*Just remember that this week is largely considered the 2nd hardest week of the course...it will be ok!
* Videos are newer, but there have also been a lot of changes in vue this year. 


Vue Video Notes - 
MPA - multi page application
* have our client call to server for initial request, it will hit website/url, and it sends back updated html doc. 
* When user makes another request, the process repeats and sends back an entirely new html doc. 
- benefits: strong SEO, more security, better memory. 
SPA - single page application
* more modern, doesn't require the page reloads.
* sends initial request and returns initial html,  but everyother request after that is just to send message to server to get new data. It changes what needs to be changed without needing to reload. 
* this helps things move along more quickly due to faster load times.
- benefits: decoupled - can change out backend and database after initial load; more mobile friendly due to lighter server load; caching and offline functionality.

Components and Pages
- CBA = concept that takes small abstracted pieces of UI and break them into their individual sections, which all put together makes a larger page. 
- Components can have components. 

Component Parts
  1.Components have a template that holds all html for a component. We can inject directly into html using double curlies. 
  - we can also bind data to an element attribute (class, style, etc). 
  - can hook into events of element by using @ or v-on, etc (think of our onClicks from js)
  2. Script is next part of component
    - contains all js for our component
    - each must have default export object that includes setup() and components {}
      - Setup must return object (has state and any event hooks, returned object contains all available methods/data for template).
      - components - nested must be imported and added to this object to be used in html.
  3. Style is final part
    - contains all css for component
    - can add lang="" to changes styling... be sure to include all needed compilers to do this.
    - can include scoped attribute which ensures applied css only applies to that component.

    Props and Emit
    Props - data passed from parent to child.
    - parent data passes as bound attribute, which is captured by child in props property. <child :data="propData">    (props:['data'])
    - can be used directly in template or in setup if passed as first argument. setup(props){props.data}

    ** vue automaticaly detects {{}} in template and interprets it as js. This is data binding. Binds data from script into template.
    ** in script, added setup() w/ a return { name: "Mark"}, in html added {{name}}   // app showed Mark

    V-on:
    - want to bind an action (the click of a button) --> added button to template. We use to add an onclick here, but in vue we do v-on:click"count" OR @click="count". count is method we want to run when button is clicked.
    - in return in script, we can create method called count() {
      console.log('clicked');
    }  // check app, should now see clicked in console.log

    Non-reactive
    - in template added value element <p> {{value}}</p>, then in script return, added value: 0, and update console to print this.value and add this.value++ before the console. should now see increasing clicked count, but page isn't updating with it yet because it's not reactive.

    Reactives and Computeds
    - Reactive objects utilize es2015 proxy to establish ability to "watch" changes 
    - reactive() is to watch full objects, whereas ref() is used to watch primitive values
    - computed takes in a function and returns immutable reactive ref object.
    - computed is able to watch reactive objects, when a changed function is re-ran and the updated value is returned. 

    ** Eventually as our app gets bigger, we will refer to these as State and AppState, which are both reactive objects used to manage data. 
      - state = reactive object on a single component.
      - appstate = reactive component shared throughout entire application. 
      - both can have computeds pointed at them. 

      - in app, added to setup() a const state = reactive{} (should get prompt to import reactive from vue here)
      - in reactive{
        value: 0
      }

      then in return, get rid of value: 0 and put state, and update this.value++ to state.value++
      - finally in template, change value to state.value
      - page should now update count. 
      - the reactive element notifies the template that the page may need to be redrawn. 

    Binding in application
    - updated style w/color
    - added to state.value p tag in template, the v-bind OR :class="{red: state.value > 9}"
    // in app, should see count turn red if above 9
    - also showed :disabled="state.value > 9" and a reset button w/state.value=0 click

  Conditional Rendering
  - in app vue, the team data in template started to get too large, so this is a good time to turn it into its own component. 
  - created Team.vue in components, added basic template for template, script, and style
    - took from app.vue the team info and put it in div in template for Team.vue that has class=team
    - next removed class=team from app.vue, grabbed from script the add/remove functions, moved to Team.vue w/ setup() and return, methods go in return. 
    - finally grabbed scss that's specific to that team and moved it over too.
  - there will be a lot broken right now so don't worry, we need to give it directions on app.vue to get child (Team.vue) to inject into parent(app.vue).
    - 1st import Team from './components/Team.vue'
    - now we need to make that available, so in setup we add 
    components: {
      Team
    }
    - then we add <team /> to app.vue template where the old template was. 
    - still throwing errors because it doesn't know what team is in lower component of Team.vue. 
      - to get teams data to populate into script, use props. 
      - in app.vue update <team /> to <team :team="team" />    // the green team is the prompt here...he changed it to say teamProp so it's more clear here.
      -then updated v-for to teamData, key to teamData, and then after prop say teamData.
    - still throwing errors, one more step
      - in team.vue, above setup(), need to add...
          props: ['teamProp']
      - finally update all team in template to teamProp
      - in setup(props)       //put props in as param, and changed team.score to props.teamProp.score

  ** props are data passed from parent component to child, they get passed as attributes on component tag.


  All Spice
  * check out scss vid for tips on updating footer for webpage...
  * if you get stuck on a bug and can't figure it out, double check that all your imports are THERE and working! ESPECIALLY THE USEROUTE!!

  * Watch set up vid for first couple steps...forgot about notes.
  min 7:00 -->
  5?. added recipes = [] to appstate.
  2. recipeServices --> skeleton w/const res = await api.get('/recipes')
  appstate.recipes = res.data
  3. HomePage --> add {{recipes}} to template w/class="home"  //just says to dump all recipes to page, no styling/html
  - homePage --> "life-cycle hook" lives inside setUp() --> 
  onMounted(() => {      //this loads the content when component hits the page. 
  recipe
  })




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


Tuesday, December 6th, 2022
vueFlix

1. bcw create vue --> vueStarter - vueFlix
  - view tmdb api, found url for popular movies, updated url w/api key. 
  - use this api to help us decide what data we want to include in our model, the naming conventions, and data types we want to include. 
2. made movie.js model w/skeleton including the data we want to include. 
  - be sure to match how it's written in api, or provide an || that matches.
3. axiosSErvice --> added export for movieApi including baseURL, timeout, and params (api_key, sort_by)
4. made MoviesService w/ skeleton. 
  - add async for getMovies (w/logger NOT CONSOLE)
5. When do we want to trigger this? Maybe on homepage?
  - open up HomePageVue, cleared out default stuff in template and added own. 
  - also cleared out default for style
  - script --> added onMounted() and an async for getMovies OUTSIDE of the onMounted so that we can still use trycatch, then the onMounted calls getMovies()
  - notice lambda in getMounted, this allows us to do more than one thing in onMounted. If we had just one thing, we could take out the lambda and it would still work. 
  ** spin up server, open local host page,  wasn't loading, so went back to vscode, stopped server, and ran npm i in terminal. once done, respin server, and it should now load. 
    - got error that onMounted was not defined, so added import link for onMounted in homepage
    - also got error because of missing part of url, updated this in moviesService. checked out network and found movies in results array
6. MoviesService --> updated  logger w/res.data and appstate.movies = res.data.results.map....
* respin, check vue in dev tools and see movies array (w/objects). 

7. homepage --> script --> return --> added movies: computed(() => AppState.movies), then added {{movies}} to template.
* respin, check site, should now see movie data(icky format)
- updated template for homePage to include all data we want to include. 
* checked page and have hard coded cat image w/info below messy movie data.
- added v-for="m in movies"    to the div for the movie element // m is value, movies is collection
- then updated the hard code with {{m.title}} and so on
- should now see each card updated w/specific data from api.
- updated img src with :src and appended url with + m.poster // this wasn't the nicest way to do it, so in model, updated the this.poster and this.backDrop in model.
- now in img src we can take out the url and just leave m.poster
- got rid of {{movies}} in template to clear our that messy data.

8. Currently we only get 20 results back, so how can we change that? 
- Check that we can do this...put entire link into api and add &page=2, now should get 20 new movies.
-HomePAge --> added new section for previous and next, check page, should now see buttons at bottom
- in script --> in return added changePage async and try/catch. await moviesService.getMovies(2)
- moviesSErvice --> added page = 1 to getMovies, and updated .get w/url to have + page
- added @click to previous button
- we really don't want to hardcode the 2 in there, so want appState to handle it. 
-movies service --> added appstate.page = res.data.page
- homepage --> return --> added currentPage: computed(() => appstate.page)
- updated @click wi/currentPAge - 1 and next page same but +1 
- change 2 to number 
- buttons should now work properly. 
- added :disabled="currentPage == 1" // this doesn't show the previous page button when on page 1.
- also disabled next button when currentPAge == 10 (just for now...)
- added {{ currentPage}} div 
- rather than using disabled, we could also use v-show or v-if (would handle it in button element like the disabled was)
  - using v-if and v-else disabled together (liked this best) ** lots of ways to do this though.

9. Movie data is large now, we should make it its own component. This helps chunk down our main homePage so it's not overlycomplicated. ENCAPSULATION
- make MovieCard.vue
-move from home page to movieCard, copy and comment out card, paste to movieCard, but get rid of v-for. 
- homepage --> added MovieCard v-for after where the template was. 
- page shows no movies with tons of errors because it doesn't recognize the m. m was used in template, but was not defined in instance. So how do we get m?
  - we need to use prop on MovieCard (:banana="...")
  - prop allows us to pass info from homepage to moviecard directly. 
  - once it's passed, moviecard needs to receive it. above setup add props: {banana: String},
    - since props is outside of setup brackets, its immediately accessible here. 
  - check webpage, should now see cards with emoji but no movie data.
  - now that this is working, lets change it to ...
    :movie="m"          AND in movieCard --> change props to :{movie: Movie}
  - check out vue dev tools, can see the data for each movie even though its not drawing to page. 
  - we need to update the m.title, etc. to movie.title in moviecard template because we changed the name from m to movie in the homepage (:movie="m" is where name change took place).
  - also removed m: computer() in return of moviecard.vue because it can't pull from appstate anymore (doesn't tell it which movie to grab)

  ** one piece of data (m) becomes a movie when it enters the movieCard

10. homepage --> added new section, but then immediately made new component for searchBar.vue
  - in homepage added <SearchBar />
  - in searchBar.vue --> added form action w/styling
  - searchBAr --> return added searchMovie() w/async, trycatch.
    - added searchTerm as query, but we'll update that later. 
  - added v-model="search.query" to input in template so that it ties together. 
  - then under setup() added 
    const search = reactive ({ 
      query: ""
    })
    - also added search, in return otherwise it won't work. 
    - moviesSErvice --> added async searchMovies(search) {
      const res = await movieApi.get('search/movie?query=lego')   and logger
    }
    -searchBAr --> add @submit.prevent="searchMovie" to search form   //note .prevent prevents default event action from happening. much simpler than previously!
    - in moviesSErvice --> updated the .get w/ {params: search}
    - check site to see if search changes
    - once we see it works, moviesService --> appState.movies = res.data.results.map(m => new Movie(m))  then save page
    - now search other terms and search should work. 

11. we were having difficulties with page buttons breaking... so made new changePage() in moviesSErvice. THIS NEXT CHUNK WAS CONFUSING AND SPECIFIC TO THIS INSTANCE W/MOVIE API
  - appstate --> just for handling pages on movie api...probably don't want to use for other apps unless it's similar issues...
    - endpoint : ''
  - moviesSErvice -> add appstate.endpoint = 'discover' to getMovies() AND appstate.endpoint = 'search' to searchMovies AND const res = await movieApi.get( `${endpoint}/moivie, {params: params})
  appstate.movies = res.data.results.map(m => new Movie(m))
  appstate.page - res.data.page

  -homepage --> updated await in try catch of changePAge()
  - appstate --> changed enpoint...
  - moviesService --> added const query and const endpoint

  - got rid of some other stuff....but added special sauce in moviesSErvice. 
  - moviesService -> in getMovies added appState.maxPAge = res.data.total_page, also put this in changePage.
  - homepage --> added currentPage and maxPage: computed(( => appstate.maxPage))
  - updated appstate w/maxpage: 10

11. updated disabled for currentPage so that it stops when you hit the max page for a search. 

12. Now want to view details of individual movies. 
  - created MovieDetailsPage.vue
  - created template
  - want to be able to navigate to new page when movie poster from homepage clicked. 
  - see router.js which defines routes for page to load depending on route taken. 
    - added our own path: '/movie', name: 'Movie Details', component: loadPage('MovieDetailsPage')   // loadPage tells app what to load
    - check app by appending movie to url, should see movie details page
    - navbar --> look at router link, should see pathway and where you will end up. 
    - also see one for about page, so let's go to MoviesService and add router link tags outside of main template, this affected our columns though, so instead better to put inside col-, added class selectable. Now when we click on movie, takes us to movie details page, but doesn't give us actual deets.
  - to get actual deets, go to router.js, and update path to have /:id  (can also do title, but depends on api). 
  - now update router link in movieCard template to include params w/id info. had to update movie.js w/id
  - now if we click movie, we'll see the url update w/id for that movie. still no deets cuz we havent made rest of template for that.

  -moviedetailspage --> to setup add onMounted w/getMovieById and function for getMoveById w/try catch, etc. Where do we get id? from url...how?
    - above onMounted add 
    const route = useRoute() //this gives us access to current router loc. in app. then logged route in onMounted function in logger, updated logger w/'route', route.params.id
  - oviesService --> add getMoviesById()
  - should now see details in console. lots more info here than we originally saw, 
  - appstate --> added activeMovie = null
  -service --> to getMovieById add appstate.activeMovie - new Movie...
    - also added return {
      movie: computer(() => AppState.activeMovie)
    }
  - then updated template to include movie.name, img, poster, etc, in {{}}. don't forget :for src
  - also added :style="`background-image: url(${movie.backdrop}`"
  - updated styling a bit. 


  Wednesday, December 7th, 2022
  GregsListVue w/auth

  * Switch out my env file later when it's time for lab

  1. run npm i in terminal, spin up server.
  2. Deleted About page, and added Cars, Houses, and Jobs page
  3. went to router.js, got rid of about, added 
  {
  path: '/cars'
  name: 'Cars',
  component: loadPAge("CarsPage")
  }
  and same for jobs and houses.
  * now if we update url w/specific page, should see what we put in template.
  * in navbar had to comment out router link for about page to stop errors.
  * added router links for cars, jobs, houses in navbar
  * check page and should see navbar links that take us to the right page.

  4. fill in our env.js file w/our info.
  * on webpage, login and check network to see token, userinfo, and account. 
  * token and userinfo come from auth0, account is your info in the database. 
  * if you don't see one of these, something wasn't linked right

  5. CarsPAge --> script --> added getCars() w/async, trycatch, etc. 
  - got to await and realized we needs carsservice, created w/skeleton and base for ALL functions w/throw new error
  - created getCars() w/ const res, logger,
    - the api should lead to baseURL, which is in our env file, so make sure that's correct. 
  - back to carsPAge, finished getCArs(), 
  - added onMounted above return because we want cars to load on initial load. 
  - checked webpage, click cars button, should see empty array or cars that are already there in console.

  6. want to save cars. go to appstate. add cars = []
  carsservice --> add appstate.cars = res.data
  carspage --> add to return cars: computed...
    - now should see messy data for cars on page (IF you have data in db already)
    - not adding a model here because it will just be added to our components. this is because we own the database, so we can just do it on backside. Remember he does have a backend server running, so that's why this works right now. 
  
  7. made carCard component
  carsPage --> added template details w/ <CarCard />
  - v-for on the col before the CarCard
  v-for="c in cars"
  - CarCard --> added card to template w/ details we want

  8. carspage --> updated CarCard w/ :car="c"
  - carcard --> added to default export: props: {car: {type: Object, required: true}}
  - update template w/ car.imgUrl, etc. 
  - updated styling w/car height and img{object-fit} // w/scss can nest classes for styling.

  9. Now let's create a car!
  - standard version - create car form component and put <CarForm /> in template
  - carForm --> added forms for all needed deets and button to submit.
  * form action acts like img-fluid in keeping form elements relatively responsive/fluid based on screen size. 

  10. need to connect form input to script 
  - carForm --> setup() add const editable = reactive({})
  * now in template updated all to have v-model="editable.make" etc (make sure they all match correctly. )
  * be sure to return editable
  * back in form, add @submit.prevent="createCar()"
  * add createCar() in return (after editable). be sure to pass it the editable (this is the banana word we linked to form earlier...editable could be whatever we want...carDAta, etc.)
  - service --> update createCar() w/post, logger, appstate.cars.push(res.data)  (newest stuff at bottom is push, newest at top is unshift...pick between these depending on how you want it to load).

  11. now add new car to webpage to make sure it works. 

  12. carform --> showed example using ref instead of reactive and why it didn't work. using ref accesses editable.value instead of just editable, causes issues. so use reactive here, unless you're trying to access the value.
    - reactive = meant for objects, red = meant for values. 

  13. hook up cancel button
    -carform --> add @click="editable = {}"

  14. some of cars have creator, let's get creator info here
  - carcard --> in template add img tag for user icon and span for name. 
  - got error because not all have creators, so added v-if to div tag. added styling to make prettier.

  15. carCard --> add delete button, styling
  - want to only show if you're cretor
     - to carCard --> template, added v-if to delete button. also added account: computer(()) to return. 
     * check site and see if button shows for creator.
  - make delete function: add @click to button
  - make delete() to return. how do we find car id? 
    - tried putting carId in, but said car is not defined. 
    - you can pass id from template or you can pull it from props. props exist outside of setup, so need to add to setup to get acces..
    aawit carsSErvice.removeCar(props.car.id)  // can remove the id from async removeCar(id) but only if you're using props. If you are passing from template, need it there still (as well as in the await).
  - carservice --> to remove add const res = await api.delete... logger, appstate w/splice
  - deleted car from page and it worked, but the above isn't best way (see comment).
  - updated appstate line w/ let index = appstate...   this way allows debugging to be easier. 

  16. Edit car
  * made carDetails page, register it with router by adding
  {
    path: '/cars/:id',
    name: 'CarDetails',
    component: loadPage('CarDetailsPage')
  }
  * yesterday we did router link, today we're going to do router push. This just changes WHEN the data is pushed.
    - carCard -> added to return under removeCar(), 
    goto() {

    } 
    to setup() added const router = useRouter(),
    updated the goto() w/router.push and logger
    then updated template w/ @click='goTo'

  - car details page --> each page needs to be responsible for its own data. how does it get the details?
    - add getCarById() to setup()    //where is the id though? there's no car on this page, so where do we get it from? the url...
    - need to add const route = useRoute() outside of return, then update the (route.params.id) in the getCarById()
    - carsservice --> update getCarById(id) 
    - appstate - add activeCar = null
    - then update carsservice w/ appstate line. 
    - didn't work because we need to fun getCarById in onMounted. should now see the details in console, 
    - added {{car}}  to template on cardetailspage, then added computed in script return for this. should now see messy data on page.
    - created template for how we want this to work (took out {{car}}). should now see deets on page.

  17. want to be able to edit if it's your post
    - add button to template in cardetailspage
    - added const editMode = ref(false) to setup, changed something in function, then in template added v-if="editMode..."
    - add @click to edit button
    - add v-if to top layer of container to check for car so it actually shows. 
    - added v-else to end of template w/ <CarForm />
    - added edit for save button after that.
    - added async editCar to script 
    - updated <CarForm /> to include :carData="car", then added props to export default
    
  18. changed create and save buttons to have v-if and v-else, updated @click to handleSubmit. 
  - carform --> moved createCar to outside of return, also added editCar() outside of return. 
  - in rturn, made handleSubmit() 
  - carservice --> finished editCar() 

  - car can be edited now but isn't going back to cardetails
    - use emit (black magic)
    - cardetailspage --> create our own event in template w/ @carEdited="(editMode = false)" in <CarForm.../>
    - in editCar function, put emit('carEdited')  // this tells parent object something has happened. this should flip it. 
    * generally data should be passed through props or appstate, so using emit isn't common/is probably harder way to do it. 

Review router links
- look at navbar.vue and router.js
* each router link points to route w/in page. the :to tells it to go to the path, the object tells which one. 
* router push is within setup, but still directs to router.


Thursday, December 8th, 2022
** WB practice - adventOfCode and edabit.com

Project - Art Terminal
1. updated env.js. 
  - since we're using project sandbox, we're going to grab auth from that. 
2. run npm i in terminal, then spin up 
3. go to localhost:8080, login to make sure it's working.
  - check network tab to make sure token, userinfo and account are all getting 200 oks
4. updated sandbox api w/api/projects to see what projects are already in there.
5. want to get the data that's there and save it in app state.
  - homepage --> want to see everyone's posts -- > add onMounted to setup()
  - add getProjects() (in setup, doesnt matter if above or below onMounted)
  - filled out try w/services, but don't have yet, so created w/skeleton, then finished getProjects function
  - note had to manually import projectService because it wouldn't load on its own.
  - add getProjects() to onMounted
  - --> service - create getProjects w/ const res and log the res.
  * check console - should see projects from sandbox as array. 
6. because we didn't write this api, it's good practice to make our own model for it
  - made Project.js in models w/skeleton. 
  - now need to store data --> appstate --> add projects = []
  - service --> add appstate line so we can now actually store.
  - did map here!
  - check view tools to make sure appstate loaded with the projects. Don't need to log appstate here because we have vue tools now!

7. Now let's log the data to the screen, doesn't need to be pretty.
  - home page --> template --> {{projects}}  AND need to add a computed to the return.
    ** this gives it an instance and draws from appstate (make sure computed and appstate imports here or it won't work! having lots of issues w/auto imports :(
    **Now check page, should see raw data dump to page.

8. Now let's figure out how we want to format data.
  -homePAge --> template --> added row and col-12. col-12 has v-for="p in projects"
  - got rid of {{projects}} and continued working on how we want to display data, but decided to make a new component right out of gate for projectCard.
  - projectCard --> template --> started but need props to get component.
    - script --> above setup props: {
      project: {
        type: Project,
        required: true,
      }
    }
  - homePage --> updated tempalte w/ <ProjectCard />
  - added testing div and checked page, got testing
  - homepage --> updated projectCard div
  - projectCard --> updated template w/{{project.title}}   //should now see titles dumped on page. 
  * generally when we v-for to inject data, we'll likely need a prop.

9. Now let's build out projectCard
  - style --> .bg-image {}   // check this out for styling of background image. 
  - want background image to change per project
  - in return added coverImg: computer(() => `url(${props.project.coverImg}`)    //had to manually import again, also added props inside setup() since we're using props here.
  - then in style, updated background-image: v-bind(coverImg)    // this is just a dif way of doing this... helps limit inline styling
  ** should now see all the dif cover imgs in cards on page. 

10. now want toget profile img on card
  - projectCard -> check console to find where creatorpic is stored. 
  - in template add img tag w/ :src="project.creator.picture"   // also added :alt="project.creator.name" and class="img-fluid creator-img"
  - added creator-img class to style to make prettier.

11. now we want to be able to click on creator pic and go to their page to see all their posts. we need profiole page to do this. 
- created ProfilePage.vue, updated template w/h1 for profile page
- projectCard --> add router link here to take us to profile page. 
  <router-link :to="{name: 'Profile'"              // when putting name, we didn't have one yet, so went to router and added path for '/profile/'', name, and component
  - now check page, when you click creator pic, should now take you to profile page.
  - what if we can store id of someone who was clicked on somewhere to the page
  - updated router.js path to include the profile id
  - checked page, got error because we're missing profile id,
  - projectCard --> in router-link, supply params: {profileId: project.creatorId}     
  - now we can use that id to make network request.
  - profiolePage --> added onMounted to setup()  //be sure this imports 
  - add function in setup() for getProfileByProfileId
  - create ProfilesSErvice   before finishing above function, then finish await in profilePage
  - we need id that's stored in params, so add to setup() const route = useRoute(), then in await add route.params. profileId
  -then add that function to onMounted
  - now add this function to service, first with just a logger to make sure we get a profileId in console that matches url.
  - back to service --> update function w/ const res = await api.get('api/profiles/' + profileId), then log the res. 
  - check console to make sure we're getting this data.

12. projectCard --> updated img tag w/ :title=" Go to ${project.creator.name}"   //should now see "Go to __'s profile page" when hovering over profile pic.
13. Now that we've gotten that data, let's store in appState.
  - add activeProfile = null
  - we don't have an activeProfile model, but we do have account model that matches pretty closely, update this model to include all data we want. 
  - updated accountService appstate line...not necessary but a personal pref? 
  - checked vue tools --> appstate shows null on some data, but that's good because we set || null in account model

14. profileService --> add appstate.activeProfile = new Account(res.data)        //not mapping here because we just got one back, not an array. map is array method
- checked vue tools and should see the data there.

15. profilepage --> need to talk to appstate w/computed. put computed in return  (make sure it imports), 
  profile: computed(()=> appstate.activeProfile)
  - updated template here w/ {{profile.name}}  to see if we get that on page, check webpage to be sure the name is showing on each profile page.
  - did a refresh and got error because activeProfile is null on load.
  - updated profile page w/ v-if"profile"    so that it loads on load.

16. profilePage --> add to script something.... missed this....go find! Or was it just setting up style for cover image? 
  - added to template inline style with :style="`background-image: url(${profile.coverImg})`"   //notice backticks here!
  - updated styling for cover-img.
  - add :src for profile.picture in img tag, h1, h2 for profile name and bio, updated style for profile-picture
  - did lots of styling here

17. updated url w/ api/project/?creatorId=844u3q89feeawnkef.ka
* this pulled up all projects for that creator
- profilepage --> added getProjectsByCreatorId() in setup()
  - be sure to pass route.params.profileId through function
- service --> add this function here, being sure to pass creatorId here. 
  const res = await api.get('api/projects', {params: {creatorId: creatorId}})   // this will append the url w/creator id provided. formats it as a query, like what we did in our url earlier.
  -and add a logger
  - didn't work because we didnt add function to onMounted.
  - check webpage, make sure the info we're getting matches who actually created. 
  - now we need to save to appstate, so in service add our appState.projects = res.data.map(p => new Project(p))    // again can map here because we're adding to array. Since we already have projects in appState, this is fine. 

18. when on profile page, how will we render data to page?
  - profilepage -> move vi-if="profile" to row w/profile.coverImg
  - add new row w/col-4, add v-for="p in projects" , but we haven't brought in projects yet, so add a projects: computed (() => appstate.projects) to script. 
  - added {{p.title}} to template, then check webpage. 
  - after checking, since this new data will be the same as the projectcard, which we already have, lets reuse.
  - bring in project card element to template, but also need to pass in the prop because it's expecting a param to be passed in order to render.
    - update the above w/ :project="p"
    ** check page, should now see it rendered to page. 
    - update styling. 
  - projectcard --> in setup() add const route = useRoute(), we'll use this route in our v-if, but we also have to add route, to return, now to router-link in template add v-if="route.name == 'Home'"    // this takes the profile image off of post in their own profile page. it was redundant so not needed.

19. how do we get to MY profile page? 
  - login.vue component --> in template, copied router-link and added new one. updated new one w/ Go to your Profile Page, plus update name: Profile
  ** checked webpage but got error because params not supplied. 
  - login --> add params: {profileId: account.id}    // we already brought in account in computed, so let's just reuse this here. 
  ** got erro and had to add v-if="account" to the router link in login, still didn't work. went to login again and update v-if w/ account.id
  ** on webpage, when you click the login button, should now see go to profile page option, and it should take you to users account page. 

  - in router, notice the account path has beforeEnter: authGuard, which prevents user from accessing their account w/o being logged in.
    - authGuard is built in package that's preinstalled for us.

20. Let's write a form for Account Page --> grabbed from bootstrap, put in template and checked out in application.
  - updated w/info we want, including submit button. make sure to put in <form>
  - once happy w/ look, add const editable = ref({})        //be sure it imports!
    - we want to take all account data and store it with the ref...
    - so add watchEffect() in setup, under const editable
      - similar to onMounted, but here we only want to run the code if he appstate.account.id is there   // notice it has editable.value because we're grabbing the value
    * check webpage, nothing changed, so add editable to the return
    - also v-model the editable.name in template. 
    * check page and name should now update. this edits what's in the appstate, which we don't necessarily want to do, so in accountpage, update editable.value w/ {...appstate.account}   // this stops user from changing their name in appstate, but still can in form.
    - now add v-model to rest of template for everything that's stored in appstate. 
    * check page, should now auto populate that data to form. 
  - add @submit.prevent to form. 
  - add editAccount() in return ... link to accountsSERvice. make sure to pass in editable.value
  - accountsService --> add editAccount(body)   // or formData...w/e you want.
    - put const res and logger 
    - update info in form. on submit should now see data pop up in console. 
    - the page didn't update though, so we need to add appstate.account = new Account(res.data) to function in service.   // could just do res.data here, but new Account(res.data) is better...
    - update form again and it should update w/o refresh!
    - checked out profile page too

21. now on homepage, want to add button on card to open model for the 
  - app.vue - COULD put modal here  (under footer) so we have access on each page, BUT better choice is to create modal component (find modal in bootstrap)
  - create modalComponent.vue, then in app.vue, add <ModalComponent />
  - projectCard --> above router-link, add button for See Images
  - add data-be-target and toggle, without won't work!
  * check page, click see images and modal should now work.

22. how to get all other images to load in modal? 
  - need to set activeProject: null in appState
  - projectCard -> find see image button, add @click="setActiveProjects", then updated return w/
  setActiveProject() {
    projectsService.setActiveProject(props.project)         //be sure service imports, alse need to pass something here so it knows which proj, so pass props.project, then log, check webpage, in console should see entire project passed after clicking see images
  }
  - update setActiveProject 

23. now let's bring in active project to modal
  - modalComponent --> add project: computed to return      // be sure appstate and computed import.
  - in template, update modal title with {{project.title}}
  - check webpage, should now see titles on modal. BUT getting error so we want to only render when we access appstate
    - modalComponent --> add v-if"project" to modal content
    - then update template modal-body w/ new container and col-12 w/ v-for="projects.projectImgs"
    - also need img tag w/ :src="p", checked modal, loaded but needed to update styling. 

24. want to show user how many imgs are in modal
  - projectCard --> to see images add {{project.....}}

25. want to see if someone's img array has my img
  - add to return containsMe: computed(() =>  props.project.projectImgs.includes(appstate.account.picture))
  - checkout vue tools --> use selector and click on dif project card, find includesMe to see true/false. 
  - now want to alter something on page to see if a post has my image in it... make button red? 
    - template --> add :class="`btn btn-outline-${includesMe ? 'danger' : 'light"
    - button should now change if post has your img
    - added i tag above see images w/ v-if="includesMe" so heart will show only if post includesMe.


    Blogger -- copy entire env.js from ref for ours since we're using sandbox. 
    Figma - first page = homepage, 2nd is their profile page, 
    - stretch goal is to make entire blog post page w/comments possible. 
   