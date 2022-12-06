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
