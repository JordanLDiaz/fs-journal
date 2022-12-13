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
* if token and userinfo is bad, its client prob; if account its server prob. 

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
  






