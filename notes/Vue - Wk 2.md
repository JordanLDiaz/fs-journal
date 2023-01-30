Monday, December 12th, 2022
To Do:
* Review node vids

Full-Stack: Post-It
one-to-many: album to creator; pictures in album; 
many-to-many: collaborators can be on many albums; many collaborators can be on single album.

New - Postman requests can have tests on them that helps check that code is good. 
- think of this as guide for how server should be built, follow the order that's in postman.

Post-It
1. create FULLSTACK-VUE. cd into post-it, then type code postIt.code-workspace
  * This is two projects brought together into one workspace, not two projects truly together in the same file. 
  * debug now has a start server and client (server: 3000; client: 8080).
2. Before starting server up, fill out .env for server and env.js for client. 
* Be sure to update connection string w/name of project in .env!
3. Start client and start server
* server play and debug terminal both have drop downs for both client and server, so make sure you're looking at the right ones.
* once they're spun up, go to 8080 for client
* login and check network, if all token, userinfo, and account come back with 200 ok, it means everything is connecting correctly and you should be good to go. 
* if token and userinfo is bad, its client prob; if account its server prob (OR YOU FORGOT TO ADD S TO HTTPLOCALHOST IN ENV.JS!)

4. Looked at diagrams for relationships w/virtuals -
* picture is in one album
* account can be tied to many albums, each album has many accounts tied to it (m:m)

5. Starting w/ create album in SERVER
  - make Album model w/skeleton: look at album in diagram to see data types
  in new Schema: ({title: {type: String, required: true}), etc. 
  * added in category, even though wasn't on model/diagram. how to tie into specific number of set values --> use enum: {'animals', 'games', 'crocs', 'pugs'}    ** says it has to be one of these values in order to be valid. 
  * creatorId: {type: Schema.Types.ObjectId, required: true, ref: 'Account'}   * need ref to reference where we're getting the objectId( what is it tied to? an account)
  * don't forget timestamps!
6. make album controller and service w/skeleton. 
  - don't forget baseController... if doesn't import find it in utils.
  - constructor w/super('api/albums')

7. now let's define our routes
  -controller --> .post('', this.create) in super/this.router, then make create(req, res, next) outside of constructor. 
  - pass in req.body in the await line
  - return res.send(album)
  - declare create() to service:
    const album = await dbContext.Albums.create(body)
    return album

* don't forget to declares albums to dbContext (w/mongoose)
* start server
* go to postman and send request. Got error and it sent request to account because it wants you to be logged in to make sure you can do so when logged in.
   - also bearer token is expired, need to get new token. Check network tab to get new one and put it in postman. SAVE!!!
   - resend request w/new bearer token, but now error w/creatorId required. 
   * DON'T MESS AROUND W/BODY OF ANY POSTMAN REQUESTS INSIDE POSTIT OR IT WILL BREAK!
   - so how do we check this? 
    - controller --> in create try/catch add req.body.creatorId = req.userInfo.id
    - also added middleware to this.router (.use...)
    ** don't forget to respin on server side!
  - postman --> resend request. checkout test results
    - passed one test, failed two. Note name of test is returned album has proper inputed data (but says it failed). 
    - we don't have creator so...
      * albumb model --> AlbumSchema.virtual('creator', {
        localField: creatorId,
        ref: 'Account',
        foreignField: '_id',
        justOne: true
      }
      )
      ** this ads virtual, but now we need to add album.populate('creator')   to service
      * respin and resend. should now see some data, but got one fail. Says we're missing something data needs
      - model --> added archived: {type: Boolean, required: true, default: false},
      * when we go delete album, it's not really gone, just stored somewhere else. So the archived is boolean that does this. default set to false so it's not behaving like deleted on default. boolean switches to true when someone actively deletes. 
    * respin and resend. now all tests pass!
    * now we know our data model and schema model are correct and we're good to proceed.

8. Move on to get All albums, hit send and got immediate error, didn't even get to test session. haven't done anything yet sooo...obvi not working
* albumsControll er --? added .get in super
    -make getAll(req, res, next) function, declare to service.
    - service --> use .find
    * respin and resend. 
    * got to string error...had to mess with tests but DONT DO THIS...just go ask for help if it happens.
  - got back 2 passes, 1 fail
  - albumSErvice --> add .populate to getAll. now last test passed.

9. get one album by id 
  - in request it says albumId error, but don't worry about it
  - controller --> add .get for this.getOne
  - add getOne function w/.getOne(req.params.id), declare to service (use findById) and pass findById(id).populate('creator')
  - service --> if (!album) throw new Bad Request...
  * respin and resend
  - should see data and test passed.

10. postman - archie album (delete)
- controller - add archive() 
   const mesage = await albumsSerivce.Archive(req.params.id, req.user)    passing in id we have access to and the user that's allowed to archive.
   -service -> notice params of id, id1....change t0 albumId, userId (DON'T REVERSE THESE OR IT WON'T FIND)
   - const album = await this.getOne(albumId)   // reuse the getOne test here... if there is one by this id we can jump into next line. since last test passed for getOne, we know it already works.
   - then add if (album.creatorId != userId) throw new Forbidden...
   - even though this is delete function, we can do what we want after this so mick put...
   album.archived = !album.archived  // this just flips the bool/archived status. if we had just set it to true, we would never be able to flip it back. we're not truly deleting here even though it's delete function.
   - return 'archived ${album.title}'
   ** respin and send, check test results. failed archived test because we didn't save. add...
    - await album.save() to archive. Don't forget to save it or the db won't know we archived!
    - respin and resend. MAKE SURE TO RESPIN THE SERVER...NOT CLIENT, easy to mix up cuz it changes to client automatically alot. 
  * Albums are good to go now!

Moving on to pictures....
11.  Create pic model w/skeleton -- check out diagram w/what we need in model
  include imgUrl, albumId (type-Schema.Types.ObjectId, ref: 'Album'),creator (same as albumId), 
  - timestamps
  -virtual:
    PictureSchema.virtual('creator', {
      localField: creatorId
      ref: 'Account',
      foreignField: '_id',
      justOne: true
    })
    * add to dbContext
12. Make picturesController and service w/skeleton
  -Controller --> constructor, super, this.router, and whatever is in postman first. in this case: 
    - .use   //auth MUST BE DONE BEFORE PUT REQUEST 
    - .post
    - make create function w/ req.body.creatorId = req.userInfo.id
    const picture = await picturesService.create(req.body)
    return, then declare to service. 
  -service --> const picture and await picture.populate('creator'), then return picture (don't forget to await the populate)
  * respin and send, got data,, passed tests.

  13. get pictures in album - note this is going to album (see this in postman url... albums/albumId/pictures... it's the picture in the ALBUM so we go to album controller)
  * best to make many small network requests than one large one, if we do requests on requests on requests, it slows server and user may refresh,causing the issue to worsen. 
    - albumsController --> not actually asking album itself for pictures.
      - add .get('/:id/pictures', this.getPicturesInAlbum)
      - make async getPicturesInAlbum()  w/ const pictures = await picturesService.getAll()
      return res.send(pictures) 
    - declare getAll to picturesService (don't forget to populate creator using find on this function in service)
    * respin and send/ got 3 pass, 1 fail (returned array only contains pictures for 1 album). This gave us back every single picture, so how do we only get all pics from that album?
    * pass albumId in function, also had to update getPictures in album controller w/req.params.id
    * respin and resend, test passes. 
    * service --> what if we just pass in query? 
    * added .get to picturesController and getAll()
    * went to localhost:3000 and manipulated url w/ different queries to show that we can get 
    * albumsController --> updated getpictureinalbum w/ getAll(albumId: req.params.id)
    ( make sure in pictures service we have query in .find of getAll function)
    * query that's being passed in is really banana word, another way could be filter since that's really what we're doing here with our query. (but in controller it has to be req.query; just in service we could really change it up...)

14. Closed server stuff, opened up client --> AlbumsService w/skeleton
    - add getAll() w/const res and logger
    - homepage --> script --> call async function getAlbums in setup. 
      - add onMounted below functions w/getAll
      * should now see albums in console.
  

Tuesday, December 13th, 2022

Jerms was made collab on Post-It, cloned down, used Mick's env info.
* Don't forget to run npm i on both server and client. 
* If you forget to go in workspace, click the bottom workspace button in left explorer and it will give you a popup for workspace.
* had to cd into both server and client folder in terminal to run npm i. 
* don't forget to add .env, won't be included when cloned down.
* spin up server, then client. Go to localhost:8080, login, check network to make sure everything is 200 ok.
  - also see albums from yesterday

1. client homepage and albumsSerivce --> Mick already wrote some code here
  - discussed needing models...can if you want, but it's our backend so don't need to. 
  - updated appstate w/ albums=[]; don't need imprt cuz we don't have model in client, but that also means we won't get intellisense. So if you want intellisense you can add model.
  - album service --> updated w/appstate.albums = res.data (no map cuz no model!)
  - checked vue in dev tools to make sure albums are there
2. homepage --> album data dump, added {{albums}} to template; add computed to return. 
  - got rid of homepage styling, checked app for data dump, good to go.
  - updated template w/how we want page to look w/container-fluid and container, added v-for to col-12 in container, checked with Test in div, saw tests on page.
  - changed test to {{ album.title}}, checked page, saw all tests change to title of albums
  - got rid of album.title and built out template (don't forget to check console for exact names of properties)
  - keep checking page as you add things to see how it looks, then continue formatting/styling.
  * checkout the elevation-6 class in styling w/box-shadow
  - appvue -- updated main class w/bg-dark
3. Since we're reusing this in other places, made component for it (albumCard.vue), cut everything below v-for and moved it to template of albumCard.
- bring in component w/ <AlbumCard />
- albumCard --> brought in computed for album to show that it will just repeat one album multiple times (see below)
- we're expecting an object, but albums is an array, so we need to bring in props
* Got rid of computed, added props for album (type: object), now need to pass down data to become the prop. so in template add :album='a' in <albumCard />
- check page, styling is gone, so move styling over to this component. should now look how it did before. 

4. Now looking at categories - want to set up buttons
  - homepage --> added row in container for them. design buttons
  - when we click on them, want to only show one to the page. how to?
    - since we have data locally, we can pull off from front end
    - homepage --> added const filterBy = ref('')      //starting out as empty string. want to filter appstate to show only ones that are category that was clicked.
    - filter does NOT destroy the array, returns new array w/o destroying old.
    - to animals button added @click="filterBy = 'animals'"
      - checked vue in dev tools to see if it worked. didn't see filterBy, so need to add to return, should now see in homepage, clicked animals button and refreshed vue tools, saw 'animals' in vue tools (homepage)
      - updated albums computed  w/ if...check out (if filterBy.value == ''), return appstate.albums.
      - also need else statement w/ return 
    - repeat above for each button (just need to add @ click because our filterBy handles the rest.)
  - now we want a button to show everything again.
    - added all with @click="filterBy  = ''"              //empty string sets it back to everything
  ** remember, did not have to do network request for any of this because we have the data locally.
  
5. How to get MyAlbums - every album I'm collaborator on (many to many)
 - need to make sure back-end supports this though, it doesn't yet, so will do later.

6. AlbumCard --> added router link to take to dif page
  - don't have page to route yet, so add one (AlbumPage)
  - added test h1
  - need to set path in router.js (notice path supplies album and the id)
  - albumPage --> router link, ad :to="{name: 'Album', params: {albumId: album.id}"    //notice the album.id matches album that we have in our computed. needs to match.
  - checked webpage and clicked album, took us to albumPage w/test h1 we put in. 
  - also if you hovered over each album should see localhost message pop up bottom left w/ path...notice id should change for each album you hover over.
  - we know we have params for router link because we supplied params in the router.js path for that page. 
7. AlbumPage --> added to setup getAlbumById() 
  - also needed to bring in route. add const route = useRoute() inside setup,
  - then can pass route.params.albumId   // must be albumId because that's what we named it in router
  - add onMounted under function. w/ getAlbumById()
  - declare to service w/const res, logger
  - checkpage, should see logger message and res in console.

8. Create picture controller and service w/skeleton      //only want to see pics when we are on albumPage
  - service --> made getPicturesByAlbumId(albumId)  w/const res, and router    look closely at .get route
  - albumPAge --> added getPicturesByAlbumId() 
     awaited picturesService (make sure imports)
     - pass route.params.albumId
     - add to onMounted
    - check console, should see message w/albumId, check that album id for each pic matches. should be same since they're in same album.


Back-end
* now we'll add server side for AlbumMember (n:m)
*  MAke sure you commit and pull down changes, then make sure you're in server now, not client

9. create Collaborator model/schema w/skeleton
  - albumId has type: Schema.Types.ObjectId and ref since it has objectId
  - same for accountId
  - timestamps
  - then added virtuals for CollaboraterSchema for both albumId and accountId
10. make collabsController and service w/skeleton
  - controller --> notice 'api/collaborators' because that's what it is in postman.
  - added .post method AND .use for middleware
  - make create() w/ let collab = AND req.body.accountId = res.userInfo.id and return res.send(collab)
  - declare to service, don't forget to add collabs to dbcontext.
  - service --> const collab and return
  - respin and send post request for api/collaborators
  - tests came back as passed, alls good.
11. album model --> added collabCount: {type: Numer, required: true, default: 0}
12. collabservice --> continue create() 
  - add const album = await albumService.getOne(body.albumId)    
  - add album.collabCount++ 
  await album.save
  return collab
  - whoopsies, collabCount should actually be memberCount, update that in album model and in service
  - respin and resend getAlbum request, look at last album created, ... got a little tricky so if I get here and get stuck go ask for help.
    - just wanted to make sure client side shows memberCount in console
  
13. Move on to next test
  - albumController --> add .get for :id/collaborators
  - make getCollaborators() w/ let collabs = await collabsService.getMembersByAlbumId(req.params.id)    //just id here because that's what's in super
  - declare to service, pass albumId here
    - include let collabs = w/find({albumId}).populate('account')    //account here because it already knows the what (album) but needs to know who (account)
    return collabs
  - respin and send get request; see data now (array w/collaborator object, inside object is account that tells who it was)
  - check tests, passed 3, failed 1. says account is not there. in test, account is called profile (account typically refers to mine, profile refers to account)
    - need to change collaborator model virtual to profile instead of account, and change that to params that were passed in service. 
    * respin and resend. should pass now

14. next test = get albums i'm collab on  (account means this should always be only me as collab...this is the my albums on figma which we haven't put in front end yet..this is why we didn't do yet)
  - Account Controller --> add .get('/collaborators', this.getMyAlbums)   // don't want accountId here because it should only ever be YOUR account.
  - add getMyAlbums() w/ const collabs = AND return
  - declare to service w/ getMyCollabs(accountId)
  const collabs = await dbContext.Collabs.find({accountId}).populate('album')    // now we know who(account)....so we need to populate the what (album)... this is opposite of the getMembersByAlbumId
  - respin and send request, see data, check test results, all passed.

15. collabsController --> add .delete in router and remove function
  - const message = AND pass req.params.userId, req.userInfo.id          // we need to see WHO is mkaing request AND what they'retrying to delete
  - declare to service, add remove(collabId, userId)
    - add const collab w/findById(collabId)      //looking for actual id of item itself
    - add if (!collab) throw new bad request('no collab at id:' ${collabId}')
    - add if(collab.accountId.toString() != userId) throw new Forbidden...          // have toString because accountId is an object so have to change it to string first.
    * this checks two different relationships to make sure they match...if there is no collab, do something, if there is collab, but it's not yours...do something different.
    - add collab.remove()
    return 'collab between ${collab.album.title} and ${collab.profile.name} has ended'    // not necessary, just fun, but had to add .poulate (profile album) in const collab
    - respin and send delete request. tests passed. also sent a few delete requests and our fun sentence above came back for multiple relationships ending.
    - should probs update our appstate when we remove something...also want number of collabs to go down
      - after await collab.remove(), add 
      const album = await albumsSErvice.getOne(collab.albumId)
      album.memberCount -= 1;
      await album.save()

After lunch....Back to front end
* updated some styling
* don't forget to pull down changes (bottom left button)
* respin server and check console to see updated data on albums
- updated our album count w/ {{album.memberCount}}  in albumCard

16. Building out form for reusbale modal that will open when we click new album and new post
  - grabbed modal from bootstrap - live demo
  - make ModalComponent.vue, place modal here
  - now need button on homePage for add album
    - go to navbar (where button will be), got rid of about button, added btn for add album; check page, should see button
    - now make sure to add data-bs-toggle and target to button or it won't open.
    - also need to inject modal into app.vue (under footer) because the button will be on ALLL pages. MAKE SURE IT IMPORTS.
    - now modal should open.
  - Now want to grab all modal content and swap it w/what we want (one for pic one for album)
    - made AlbumForm.vue, grabbed what we want from modal (below modal-content), place it in template, update info w/what we want. 
    -did <slot></slot>  so we can put dif component inside here
    - app.vue --> added end tag for modal component so we can inject <AlbumForm /> inside it.
    - now it will inject whatever is in slot to that component (nested components essentially).
    - little tricky so don't worry about or ask for help.
  - modalComponent --> need to add input fields (looked for floating labels...coool! put in body).
  - wrap everythng in form div, update button names/attributes
  - added url input, dropdown for category (selects), 
  - need to add const editable = ref({}) to setup, then add editable to return
  - update template w/ v-models
    v-model="editable.title", etc. on select, only v-model="editable.category"
  - add @click.prevent="createAlbum" to form tag.
  - add createAlbum under editable in return. 
    - test w/logger of editable.value, create an album on page and check data that comes back.
    - can also check vue tools to see how data changes as info is inputed. 
  - now that test worked, update createAlbum w/ await, passing in editable.value (don't forget value!)
  - declare in albumForm, passing in body, w/ const res 
  - test again by making new album, checkout network tab. 
  - want to clear form out, go to albumForm --> in create album add editable.value = {}     under await
  - also added Modal.getOrCreateInstance('#exampleModal').hide()           // this will hide the modal after you submit new album
  - in service --> add appstate.albums.push(res.data) - adds to end of album array. 

17. After we create the album, what if we want it to auto take us to that album?
  - albumform --> added const router = useRouter().... then in createAlbum added 
  router.push{name: 'Album', params: {albumId: }}
  - in albumsSErvice --> added appstate.albums
  - albumForm --> added const album = to the await line in createAlbum
  - then updated params above w/ albumId: album.id
  - sent practice and it took us to album
  - need to save to appstate now, but update appstate with activeAlbum: null
  - then in albumsSErvice, add appstate line (no mapping cuz it's single object)
  - check vue tools to make sure it worked. it's there! 
  - how do we get it's info to the page? 
    - albumPAge --> add computed to return for album:  activealbum
    - update template w/ {{album.title}}   w/ v-if="album" // since it starts as null, we need v-if
    - once we see that works update templat w/how we want page to look. 
    - include v-if="album"
      - update w/album coverImg, title, creator.name (nested obj), 
    - need t o store pics to appstate, update appstate w/ pictures: [], then add appstate line to pictureSErvice
    - album page --> need to bring in pictures in return (computed), then add v-for to p in pictures line in template.
    - refresh and should see pics if there are any in album.
    - added a href w/ _blank to line so that when pic is clicked it opens it to new page (this is what tyler showed me yesterday to open links from checkpoint to new page).

18. Now let's get our collaborators so we can display on album page.
  - albumPAge --> update template w/ new row/col-12 w/in col-4 for side of page.
    - added div for {{album.memberCount}}   and div for Collaborators heading, added heart icon
    - will come back to writing btn functionality later
    - now need collaborators to load to page
      - albumpage --> made getCollabsByAlbumId()
      - made collabsService w/skeleton- finished getCollabs() ... make sure service imports. 
      - update params w/ route.params.albumId
      - declare to collabsService
      - pass albumId
      - const res, check url path to make sure it matches what we named it; log the res
      - back in albumPAge, that function is grayed out because we didn't put in return. 
      - checkout page and see if any collab data.
      - update appstate w/ collabs: [] so it can be save
      - update service w/ appstate.collabs = res.data so it will save to appstate.
      - in albumPage add computed for collabs to return. 
      - update template to where we want collabs to show w/data dump {{collabs}}, check page, should see data dump
      - update the line before {{collabs}} w/ v-for, then change {{collabs}} with what we actually want (img tag for collabs.)
      - check page, should now see. 

19. Now on homepage we want to get My Albums section so we can see albums we're collab on
  - go find to do in homepage we left for ourselves earlier.
  - add getMyCollabAlbums ... send off to collabs service since we're getting collabs back.
  - collabSErvice --> declare above function w/ const res and api.get('account/collaborators'), log the res. 
  - check console to make sure we're getting data.

  ERROR
  - not logged in when we refresh page, so we get error.
  - fix... go to authSErvice -->  find anchor note. what do we want user to be able to do once logged in? 
    collabsService.getMyCollabAlbums()       //technically can get rid of getMyCollabAlbums in onMounted now. can also take out entire function from albumPAge.
  - updated service w/try catch and appstate line
  - added myCollabs: [] to appstate and changed appstate line to myCollabs instead of just collabs. now when we refresh, the collabs don't disappear from array (before it was over written so it disappeared on refresh).

  - went to postman and sent post request to api/collaborators, sent albumId w/ raw json, and refresh page, can now see that we're collab on album. 

20. now that we're collaborator on album, let's get that album drawn to our page in the My Album section.
  - make template 
  - we already have component that supports getting album to page... so let's reuse that! How though?
  - added myCollabs to return in homePAge, then v-for in template (need to make new div for it), 
    - v-for="c in myCollabs"
    - inject <AlbumCard /> in that div, but need props so add :album="c"
    - this gave error because we need :album="c.album"     //album is nested in c!


Checkpoint Notes
  - Tower - 
    ** be sure to spin up both client and server in order to get tokens!
    - update auth token in postman (top level folder --> variables)
      - need 2 auth tokens to run these tests. 
      - find in network, sign in --> grab one bearer token, place in auth1 (normal way), save
      - for 2nd one: right click codeworks logo and open new browswer in private, log in w/different account, now grab THIS other bearer token and copy into auth2, save.
    - 2 bearer tokens is for invalid requests
      - this makes sure we're throwing errors appropriately when other users are trying to be malicious w/use of your app.
    - when handling tests, go in order of files in postman.
      - if get and put requests are all passing, can technically start working on front end (can write our most of this)
    - if we click logout, will have to get new bearer token. 
    * want to pass test on user account logged in before posting (in postman)

    - comment = one to many
    -ticket = many to many

    - don't worry about login bar on left- just focus on navbar like with theNetwork.
    - stretch goal on figma for account page (myEvents that I've created, not required to pass)
    * for design requirements - code 1 and 2, can miss only 1 in each box to still pass that requirement. 
    - text-contrast - add shadow, make sure good contrast. Can use lighthouse in dev tools to see accessibility score. can also use element tool to hover over elements and see contrast score. 


Wednesdsay, December 14th, 2022

Adding back-end - access control
  - what are users allowed to do? 
    - when album is archived, we don't  want to lose the pics and collabs.

1. albumSErvice --> get one function... change out findById w/findOne({_id: id, archived: false})      // quick and dirty way to handle soft deletes.
  - Another way to handle - throw in another check:
    if(album.archived) {
      throw new forbidden('Can't modify an archived album')       // BUT this breaks the page so...
    }     
  - Better way....we want to modify albums info in pics service (create). place check above const pic
    const album = albumsService.getOne(body.albumId)
    if (album.archived) throw new forbidden('cant modify archived album)

    ** we need above check in collabsService create function too. copy/paste, but will need to get rid of other const album
    ** do again in collabsService remove()     //good notes in ref here, big function!
      - this one needs to be changed more cuz we don't have album. needs to go after the if(collab.accountId.toString), update .getOne(collab.albumId)
    - in postman, ran delete archive test, success. checked by sending post request for that archived album, got response that says can't modify archived album.

2. collabsService --> add await collab.populate('profile album') in create() above return collab.
  - did album cuz that is a virtual that needs to be populated when a collaborator is added. 

Front-end
  * shouldn't be able to collab multiple times (like not being able to buy multiple tickets for same event).
  * want to hide buttons that don't need access to.

1. if not logged in, shouldn't see add album button/modal
  - navbar --> update that button w/ v-if="account.id"   //have to add id because it's empty object
  - add computed for account  (make sure it imports)
  - homepage --> want to remove collabs if not logged in
    - add v-if="account.id && myCollabs.length > 0"        
    - add computed for account
2. on albumpage --> add ability to become collab.
  - on collab button, add @click="becomeCollab"
  - need albumId and accountId to become collab, accountID should be handled on backend,
  - add async createCollab w/trycatch in return (after computeds)
    await collabsService.createCollab({albumId: route.params.albumId})         // remember this is in return because we want user to have control over, otherwise would go outside of return.
  - collabsSErvice --> add createColl(body) {
    const res = await api.Post('api/collaborators', body)   // sending up body to collaborators
    logger
  }
    - check network to see that it was sent, on refresh should see new collab added to list
3. don't want to see add collab button on album page if not logged in.
  - add computed for it on albumpage, add v-if for createCollab button in template. 
  - back in collabsService, add appstate line.  
    - where do we want to save it? collabs or mycollabs? 
    - tried just collabs, worked for albumPage, but not fully.
    - need to add to myCollabs in appstate too so that it renders on homepage tooo (see note about this).

4. when we became collab, memberCount didn't go up. 
  - collabsService --> update createCollab w/ 
    appstate.activeAlbum.memberCount++               // this updates the memberCount on the activeAlbum and saves it to appstate
5. once we're collab on album, want collab button to disappear.
  - need to see if array of collabs has me in it... did something like this on artTerminal proj.
  - albumPAge --> add to return:
  includesMe: computed(() => AppState.collabs.find(c = c.accountId == appState.account.id))         // use find, this checks to see if you can find me in collabs for this album
  - now we can update our v-if to includes && !foundMe
  - if we want to return true or false, can add !! in front of appstate in computed ....if not worried, just leave off !! and check vue for object vs. undefined
6. want to uncollab on album.
  - copy createcollab button and add v-else-if="account.id", update @click for uncollab.
  - add removeCollab function on albumPAge   - want id from object, so got rid of !!, updated removeCollab button w/foundMe.id, pass collabId as param for removeCollab function
    await collabsService.removeCollab(collabId)
  - declare to service -->
    pass collabId here too 
  - in albumPAge, update function w/
    if(await Pop.confirm() {
      await collabsService....
    })
    - update service w/ appstate line
    appstate.collabs = appstate.collabs.filter(c => c.id !== collabId)     // do again w/myCollabs
    appstate.myCollabs = appstate.myCollabs.filter(c => c.id !== collabId)
    appstate.activeAlbum.memberCount--             // makes sure the count goes down.

  7. now need to be able to add pictures to album
    - update albumPage w/ button for add pic, include v-if="account.id && foundMe"    //makes sure we are collab on this album.
    - want to bring in modal component we have to pop up 
    -   add 
    <ModalComponent > 
      <PictureForm /> 
    <ModalComponent />
    - made pictureForm component  for this. update w/info we want
      - add const editable = ref({}), return editable
      - add v-models to form
      - make sure you have toggle and target for modal 
    * checked page, and add picture modal worked, but the new album button has switched to the pic modal instead of album because they have same id...will open the one it sees first in html (does pic one because album modal is below footer)
      - how do we give each modal unique id's? 
        - got rid of id for album modal in albumPAge, and added id="albumModal" directly on homePage <ModalComponent id="albumModal"
        - albumPage --> add pictureModal id to modalComponent here. 
        - be sure to update id on pictureModal buttons. 
  8. need to finish createpic()
    -picForm add createPicture() to return. pass editable.value!
    - bring in const route = useRoute() to return
    - add editable.value.albumId = route.params.albumId
    - editable.value = {}
    - declare createPicture() to service
  
  9. how do we tell if album is archived? 
  - checked archived album and got error, don't want to see collab button if archived.
  - albumPage --> added btn for disabled album w/ v-if="album.archived"   above other v statements...want to check if archived FIRST. 
  * changed lots of stuff on this button...just look at ref.