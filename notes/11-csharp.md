# MVC
Monday, Jan 16th, 2023

* Benefits 
  - error messages are more specific/better error handling

Dotnet  6.0

Command prompt - 
  - dotnet new console -o catSharp (fileName)
  - cd into file, then dotnet new to make a new project
  - can dotnet console to see what files are there.
  - code . into file, then dotnet run when wanting to see output

C#
  - everything is a class
  - ; must be added to end of line to show that line of code is done
  - don't use var, instead use variable name that is specific to type of var (int, string, char, bool, double, float, long, decimal,  etc)
  - strings can only use ""

  - dictionary<> (these are our objects now)
    - inside carrots asks for Tkey, Tvalue (e.g. int, string)
      ex: weird = new Dictionary<int, string>() {
        weird.Add(0, "sup");
        weird.Add(1, "hello");
      };
    ** see reference for more examples
  - Console.WriteLine(normal["greeting"]);
    Console.WriteLine(normal["farewell"]);
    ** dotnet run in cp and will get the greeting and farewell 

  - array
    int[] numbers = {2, 3, 5, 6};
    string[] string s = {"hello", "arrays", "are not", "fun"};
    ** won't use arrays really, can't do much with them. different from previous arrays we've used because they cannot be changed. 

  - list - use this instead of arrays, can add to and remove from
    List<int> = new List<int>() {12, 120};
    list.Add(11);
    list.Remove(11);

  public class Cat 
  {
    public string name;
    private int age;   // no one else will be able to access this except that one cat because it's set to private
    public string color;
  }

  * have to generate constructor for this (right click cat?), spits out public at w/schema

  List<Cat> catHotel = new List<Cat>(); (had to move above class cat)
  catHotel.Add(new Cat("Maxwell", 2, "yellow"))
  catHotel.Add(new Cat("Jeremiah", 6, "grey"))
  catHotel.Add(new Cat("Randall", 11, "black"))

  string choice = console.ReadLine();
  Cat picked = catHotel.Find(c => c.name == choice);
  Console.WriteLine(picked.name + " " + picked.color + " " + picked.age)

  * in c# you have to define if something is private or public, so update class items to have public or private
    - if you don't specify to public, it autos to private

  * added public string getGeneralAge()   // adding string denotes the type of return
    {
      if(age > 6) 
      {
        return "old";
      } else 
      {
        return "young";
      }
    }

    * dot run enter Maxwell should return the string from the Console.WriteLine


New proj.
- bcw create --> dotNet-auth --> proj name
- npm i equivalent = dotnet restore
- don't worry about obj, bin files
- html lives in wwwroot
* now have controller, service, and repositories (extra layer) 
* models - similar to schema, but also not... looks like when we defined cat (above)
  - can specify public/private here (can specify get and set as well)


1. created Cats controller 
  - in public class CatsController : ControllerBase  // C# uses : instead of extends for inheritance 
  {
    public ActionResults<List<Cat>> Get() {
      try {
        List<Cat> cats = catsService.Get();
        return Ok(cats)
      }
      catch (Exception e)
      { 
        return BadRequest(e.Message);
      }
    }

  }

2. add Cat.cs model
  public class Cat 
  {
    public int Id {get; set;}
    public string Name {get; set;}
    public int Age { get; set;}
    public string Colr {get; set;}
  }

3. catsController --> need to "import", but here we do using catHotel.Models  (this brings in the namespace catHotel)
  * C# also lets you add these to global.cs if you want it imported everywhere
  - updated try catch above

4. create catsService
  - catsController --> add private readonly CatsService catsService; // this says catsController will have this
  - add public CatsController
  - catsService -> add 
  public class CatsService 
  {
    private readonly CatsRepository _repo;   // says catsService will have repo

    public CatsService(CatsRepository repo)  // actually defines ??
    {
      _repo = repo;
    }
    internal List<Cat> Get()
    {
      List<Cat> cats = _repo.Get();
      return cats
    }
  }
  - create CatsRepository, then update service
    - add private List<Cat> catDb;
    right click CatsRepository and create constructor, in public CatsRepository... add this.catDb = catDb;  // removed this later because it caused probs, but this will just be for today since we have fake db
    catDb.Add(new Cat(1, "Maxwell", 2, "yellow-green"));   etc...

    internal List<Cat> Get()
    {
      return catDb;
    }
  - catsController --> add decorators above respective methods
    [ApiController]
    [Route("api/cats")]
    [HttpGet]

  5. spin up server, note it's not 8080 or 3000
    - it opens up new browser, its not happy, so click advanced and continue anyways. 
    - dot net sets up https (SSL), but we're just in dev mode so we don't need to worry about it. 
    - postman --> got SSL error - go to settings and turn SSL off. 
    - got catsService error, have to add to startup.cs
      services.AddScoped<CatsRepository>();
      services.AddScoped<CatsService>();    // creates instance of both of these things for anything that will use these files. 
    - send get request in postman and should now see our db data we created in repository

  * repo layer basically takes over what mongoose use to do for us. we will write our own queries (we define what find does, findbyid, etc)
  
  6. controller --> added [HttpPost]
  public ActionResult<Cat> Create() {
    try {
      Cat cat = _catsService.Create()
      return Ok(cat)
    }
    catch (Exception e) 
    {
      return BadRequest(e.Message)
    }
  }

  - on a create we need a body, so in Create([FromBody] Cat catData)
  and add catData to .Create in try
  - declare method to service, puts in the internal, replace error message w/... 
    Cat newCat = _repo.Create(catData);
    return newCat;

    * declare .Create to repo
  - in internal, add 
  catData.Id = catDb.Count + 1;
  catDb.Add(catData);
  return catData;

  7. postman --> post request
    - fill in body w/data to create cat and send
    - resent get request, no new cat :(
        * updated startup services to addSingleton, then resent get request, new cat added
  
  8. controller --> delete function
    [HttpDelete("{id}")]
    public ActionResult<string> Remove(int id) {
      try
      {
        string message = _catsService.Remove()
        return Ok(message)
      } 
      catch
      {
        return BadRequest(e.message);
      }
    }

    ** need id to know which one we're deleting, typically pass through, how do we do here?   "{id}" in decorator
    - declare Remove to service:
    add:
    string message = _repo.Remove(id);
    return message;

    - declare .Remove to repo
    repo --> 
    Cat catToRemove = catDb.Find(c => c.Id == id)
    bool result = catDb.Remove(catToRemove)
    if(result) {
      return $"{catToRemove} was removed from db";
    }
    return "no cat to delete";

    - send delete request, get message, send get request, its gone. 

  ** Can reuse method names and it will know which one to run based on params passed. 
  ** in put request, can pass in 2 params, shouldn't matter what order, c# can figure out.

Wednesday, Jan 18th, 2023
Post-It C#

1. Open dotnet vue file and make new
  - will need to get auth tokens (dummy one too).

2. Setup - create model, controller, service, repo, table
  - create albumb model first (prop tab will give you public ...)
    * can add required to properties but had to update the global cs with something
  - since we are using auth, also need to add creator id (just string now, not objectid like in mongoose)
  - repo - added private readonly IDbConnection _db;
  - service - added private readonly AlbumsRepository _repo;
  - controller - added private readonly AlbumsService _albumsService
  - also make sure to generate constructor on each of these pages
  - startup - add transients for albums (repo must go before service)

3. Create table in dbSetup for album
- pancakes --> new query to establish connection
- creatorId - want to tie this to account id, need to make sure the type is same (VARCHAR)
  - after creatorId, added FOREIGN KEY (creatorId) REFERENCES accounts (id)
  * this is what ties the creator id to id of account table. This will also prevent user from making album if no accountId
  * this also allows us to hook into event, add ON DELETE CASCADE, this will delete album when account is deleted automatically. 
- execute -- check table in pancakes
- spin server, do postman test
- dbSetup --> insert into albums, check table

4. controller --> get request, declare to service, then repo, etc.
  - run get request on postman
  - dbsetup --> new SELECT where we will JOIN accounts ON accounts.id = creatorId;
    - this will create a table where the account info is linked to the album info
  - repo - update select w/ JOIN accounts ON al.creatorId = ac.id; also updated * to al.*, ac.*. updated db.Query to <Album, Account, Album>(sql, (album, account) =>
  {
    album.Creator = account;
    return album;  // think of this like populating
  }).ToList();
  - had to add public Account Creator to album model
  - save, respin, send  (got error so added the populate to repo), then respun and got creator attached to album data
  * dbsetup is place to test out sql before doing it in repo, helps clarify if bug, is it sql or c# bug?

5. Added new entry to table w/ archived album, so want to stop it from being shown
  - services --> in get(), filtered
  List<Album> filtered = album.FindAll(a => a.archived == false); return filtered
  - controller --> bring in new service
  private readonly Auth0Provider _auth0provider;
  then in try for get() add 
  Account userInfo = await _auth0provider.GetUserInfoAsync<Account>(HttpContext);
  (go get account info, make request to another server, so await, will return account data type, gets token from http context)
  - had to wrap ActionResult in Task<> because get was mad
  - update get w/ userinfo.id
  - service --> update get w/ string userId
    - updated == false to || a.creatorId == userId

6. Controller - Post request
  - put [Authorize] after [HttpPost] to affect capabilities when logged in (only put this after requests you want user to be authorized to do)
  - follow request all the way through service and repo
  - controller --> add album.Creator = userInfo; because we need creatorId at top level. without this wont see creator info in postman request.

7. Get One
- should be able to look at if logged in or not
- controller --> try: Account userInfo = await _auth0Provider.GetUserInfoAsync<Account>(HttpContext);
Album album = _...
- repo - select looks like get, but add where al.id = @id;
* FirstOrDefault only returns one
- service --> update for access control
  if(album.Archived == true && album.CreatorId != userId)
  {
    throw new Exception("You don't own that")
  }
  * respin and send request, should see ones I own
  * if you do no auth, should see you dont own that message

8. Delete
- service
{
  Album original = GetOne( id, userId);
  if (original.creatorId != userId)
  {
    throw new Exception("Not your album.")
  }
  original.Archived = !original.Archived;
  _repo.Update(original);    // added update to repo so this was possible
  return $"{original.Title} has been archived"
}

Thursday, Jan 19th, 2023
Continuring Post-it

* He updated proj back end with pictures controller, service, repo, model

1. albumController -> adding get for picturebyalbumId
  - [HttpGet("{id}/pictures")]
  public async Task<ActionResult<List<Picture>>> GetPictures(int id)
  {
    try 
    {
      Account userInfo = await _auth0provider.GetUserInfoAsync<Account>(HttpContext);
      List<Picture> pictures = _picturesService .GetPicturesByAlbum(id);            // have to update private readonly w/ PicturesService here since we're bringing in a new service
      return (Ok) pictures
    }
    catch (Exception e)
    {
      Return BadRequest(e.message)
    }
  }

  picturesService --> internal List<Picture> GetPicturesByAlbum(int albumId)
  {
    List<Picture> pictures = _repo.GetPicturesByAlbum(albumId)
    return pictures;
  }

  repo -> internal List<Picture> GetPicturesByAlbum(int albumId)
  {
    string sql = @"
    SELECT
    p.*,   // grabbing all from picture
    a.*   // and grabbing all from account
    FROM pictures p
    JOIN accounts a ON p.creatorId = a.id
    WHERE albumId = @albumId;
    ";
    List<Picture> pictures = _db.Query<Picture, Account, Picture>(sql, (picture, account) => 
    {
      picture.creator = account;
      return picture;
    }, new {albumId}).ToList();
    return pictures;
  }

2. picturesSErvice --> in getpicturesbyalbum add albumsService in readonly
  Album album = _albumsService.GetOne(albumId, userId) // in controller, also add userInfo?.Id to getuserinfoasync in try, also update internal list w/ string userId
  * get one in albumservice already handles error handling, so don't need to add to pictureService
  * respin, send post requests for multiple entries, do get request

3. Create AlbumMember model (same as collaborator, but named differently because   . albumMember has many to many relationship)
  - must have id, albumId, accountId
  - dbsetup -> CREATE TABLE for albumMembers
    * don't forget foreign key 
    Foreign key (albumId) REFERENCES albums (id) ON DELETE CASCADE,
    Foreign key (accountId) REFERENCES accounts (id) ON DELETE CASCADE,
  -  new insert
    INSERT INTO 'albumMembers'
    ('albumId', 'accountId')
    VALUES 
    (5, 'id')
  * execute and view table in pancakes

  new SELECT
  am.*,
  ac.name
  ab.title
  FROM albumMembers am
  JOIN accounts ac On am. 'accountId' = ac.id
  JOIN albums ab ON am.'albumId' = ab.id;

4. albumController --> add 
[HttpGet("{id}/collaborators")]
public async Task<ActionResult<List<Collaborators>>> GetCollaborators(int id)             // because we have collaborators now, make new model by extending account model (includes AlbumMemberId, as well as all other info from account model. it's just EXTENDING it)
  try --> 
  Account userInfo line (same as httpPost)
  List<Collaborator> collabs = _collaboratorsService.GetCollaborators(id, userInfo.id)                //create collabsService and repo, update controller w/ collabsService readonly  
    - collabsSErvice --> add readonly for albumsSErvice
    - GetCollaborators -> 
    {
      Album album = _albumsSErvice.GetOne(albumId, userId);

    };

    - collabRepo -->
    {
      string sql = @"
      SELECT
      *
      ac.*,
      am.*
      FROM albumMembers am           // getting account info, but selecting from albumMembers table because we don't want to see every account that exists
      JOIN ac ON ac.id = am.accountId
      WHERE am.albumId = @album.id
      ";
  return = _db.Query<AlbumMember, AlbumMember, Collaborator>(sql, (collab, albumMember) => (collab.albumMemberId = albumMember.id;
  return collab;) )
    } new {albumId}).ToList();

    * don't forget transients for new tables (collaborators)
    * made new table for albumMembers, but not collaborators, just join albumMembers and account which is our collaborators. Because we extended the account model to include albumMember stuff, we needed a new model to represent that, which allowed us to make a new albummember table that joined info from account. 

5. Accountcontroller --> new get request for "albums" w/authorize
  - album model -- extend album model w/ MyAlbum   w/ albumMemberId
  - accountController -> GetAlbums  // don't forget to update constructor w/readonly
  {
    try
  {
    Account userInfo line
    List<MyAlbums> myAlbums...
  }
  }

  collabsservice --> function
  collabsrepo --> SELECT
  ab.*,
  am.*,
  cr.*
  FROM albumMembers am
  JOIN albums ab ON ab.id = am.albumId
  JOIN accounts cr ON ab.creatorId = cr.id
  WHERE am.accountId = @accountId; 
  ";
  List<MyAlbum> myAlbum = _db.Query<MyAlbum, AlbumMember, Account, MyAlbum>(sql, (ab, am, cr) => {
    ab.AlbumMemberId = am.Id,
    ab.Creator = cr;
    return ab;
  } new {accountId}).ToList();
  return myAlbums;

  ** showed simpler way to do GetCollaborators because we're not joining complex data (two objects), only wanting to add one new value. 
  * did get request for accounts, saw data, then appended albums to url to see albums attached to that account. see albumMemberId in output because we're getting that data joined to account? 

6. create new CollaboratorsController w/ post request (authorize)
  * remember task signals we'll have to await something because order of actions needs to occur in certain order
  - try: account userInfo  = ...
  albumMemberDAta.AccounId = userInfo.id;
  Collaborator collab = _collabsService.Create(albumMemberData);
  return Ok(collab);

  collabService --> repo


  Monday Jan 23rd, 2023
  InstaCult
  * came in late missed cult deets...

1. Models - Created cult.cs, cultMember.cs, RepoItem.cs
    * added models and updated account model.
  
2. Now make tables for models
  - connect to db (pancake)
  - create cult table (with foreign key leaderId that references accounts id), cult members (foreign keys for cultID that references cults, and accountId that references accounts id)
  - tested using insert for cults and cultMembers

3. Cult post route
  - make controller, service, repo
  - Repo - hook it up, w/ private, generate constructor, repeat w/service and controller.
  - startup - add transients
  - update app dev settings
  - controller -- write out post w/ authorize (pull in auth0provider in readonly and constructor)
    - cultaData.LeaderId = userInfo.Id;
    Cult cult = _cultsSErvice.Create(cultData);
    return Ok(cult);
  - service --> generate create from controller
    - Cult cult = _repo.Create(cultData);
    return cult;
  - repo --> sql w/ insert for name, description, leaderId
  - outside of sql -->
  int id = _db.ExecuteScalar<int>(sql, cultDAta);
  cultData.Id = id;   // fixes the id
  return cultData;

4. Cult Members (many to many)
  - create controller, service, repo; update private readonly and constructor (don't forget auth0)
  - ADD TRANSIENTS TO STARTUP!

5. cultMembers post
  - public async Task<ActionResult<Cultist> Create([FromBody] CultMember cultMemberData)
    - account model --> extended account model w/
      public class Cultist : Account
      public int CultMemberId {get; set;}
    - also added public class Profile : RepoItem<string> w/ string Name and string Picture
    - also updated Account to extend profile and Cultist to extend profile ... this prevents other users from seeing others' emails. does make the account harder to look at.
  - controller ->  try
    cultMemberData.AccountId = userInfo.Id;
    int id = _cultMemberSErvice.Create(cultMemberData);
    userInfo.CultMemberId = id;
    return userInfo;   (see now about why we constructed it this way)

  - service --> straightforward, generate to repo
  - repo --> 
  string sql = @"
  INSERT INTO cultMembers
  (cultId, accountId)
  VALUES
  (@cultId, @accountId);
  SELECT LAST_INSERT_ID();
  ";
  int id = _db.ExectureScalar<int>(sql, cultMemberData);
  return id;    // this id goes back up to service, then back to controller, and is assigned to be the userInfo.CultMemberId before the return in controller.

  - dbsetup --> ALTER TABLE cults
  ADD COLUMN memberCount INT NOT NULL DEFAULT 0;
  ** be careful altering tables, can easily get bad data if not careful. if early on in proj, just drop table and recreate

  - cultsService -->   ** wrote function that requires get one, so added get one function before completing update.
internal Cult GetOne(int id)
{
  Cult cult = _repo.GetOne(id);
  if (cult == null) {
    throw new exception($"No cult at id:{id}")
  }
  return cult;
}

  internal Cult Update(int id, Cult update)
  {
    Cult original = GetOne(id);
    update.Name = update.NAme ?? original.Name;
    etc...
    _repo.Update(update);
    return update;
  }
- repo - > write sql for getOne
  - getOne
  {
    sql = @"
    SELECT
    c.*,
    prof.*
    FROM cults c
    JOIN accounts prof ON prof.id = c.leaderId;
    WHERE c.id = @cultId;
    ";   //cult -- added leader to cult
  Cult cult = _db.Query<Cult, === Profile, Cult>(sql, (cult, prof) => {
  Cult.leader = prof;
  return cult;
  }, new {cultID}).FirstOrDefault();
  return cult;
  }

  - repo - write sql for update (internal bool)
    UPDATE cults SET 
    name = @name;   etc...
    WHERE id = @id;
    ";
    int rows = _db.Execute(sql, update);
    return rows > 0;

  -cultMembersSerivce --> 
    - add CultService to class and constructor
    - update create w/ Cult cult ....


6. start client and server, go to localhost808
- grab token for postman
- run create test
  * leader was null 
- ran test for join cult, worked, checked sql table in dbsetup, saw membercount for that cult increase to 1.

7. GetAll for cults
  cultController --> straightforward
  - declare to srvice --> straightforward
  - declare to repo
    sql = @"
    SELECT 
    cult.*,
    prof.*
    FROM cults cult
    JOIN accounts prof ON prof.id = cult.leaderId;
    ";
    List<Cult> cults = _db.Query<Cult, Profile, Cult>(sql, (cult, prof) => {
      cult.Leader = prof;
      return cult;
    }).ToList();
    return cults;
    * send postman get request, got good data

  - update sql w/ 
  LEFT JOIN cultMembers cm ON cult.id = cm.CultId
  ** dbsetup --> showed this select with and without LEFT join, without left didn't get new cult at all. 
    - also added COUNT(cm.id) AS calcMemberCount // will count all cult members that match
    - needed GROUP BY (cult.id);    // takes results of join and tells how to group the data together
  - updated sql in repo, note old regular one still there for ref.

  - update getOne
    - add WHERE cult.id = @cultId; to sql
  
  ** using the calculated membercount allows you to remove hardcoded member count from create in cultMemberService


Review of postIt
  -collab repo --> getMyAlbums  -- will be similar to favorites.
  - getCollabs -- more like who are the people who have favorited this recipe...but don't need for allspice
  - getmyalbums is like getmyfavorites. 

Tuesday, Jan  24th, 2023
InstaCult c# Day 2

1. added GetOne in CultsController
2. Get cult members by cultId
  controller --> getting back list of cult members <cultist> (cult member is the many to many, cult member is singular member of a cult, so not many to many).
    - sent to cultMemberService, so update in private and constructor
    - write function (updated id to cultId), pass to repo
  Repo --> 
    SELECT
    cm.*,
    a.*
    FROM cultMembers cm    // selecting that data from RELATIONSHIP TABLE
    JOIN accounts a ON cm.accountId = a.id  
    WHERE cm.cultId = @cultId;
    ";
    LIST<Cultist> cultists = _db.Query<CultMember, Cultist, Cultist>(sql, (cm, cultist) => {
      cultist.CultMemberId = cm.Id;
      return cultist;
    }, new {cultId}).ToList();
    return cultists;
          // we are ultimately returning a cultist, not a cult member because cult member is just the relationship, cultist is the actual individual member that we are trying to get data on. 
        <CultMember, Cultist, Cultist>   // matches order in select (a account is pulling the cultist data which is cultMemberId, then 3rd spot is Cultist because we want to get cultist data back. )
        - in sql, (cm, cultist)   passing in params that match <CultMember, Cultist>
        - set the cultMemberId on cultist = to cultMember Id, return singular cultist, then take that new cultId and add it to list of cultists.
        - if we don't map here, the cultMemberId would come back null because it doesn't exist on profile table.

    - Repo Item review
      - repItem has T id, created/updated at. we extend profile w/ repo item,, need to specify int vs string because T Id exists in multiple places, but isn't always the same data type.

3. Delete cult member
  - cmController --> straightforward delete
  - cmService --> pass in cmId, userId
    bool result = _repo.Remove(cmId);
    return "Someone has left the cult."
  -cmRepo --> straightforward
  - cmService --> updated w/ result == false check
  
  - only leader of cult should be able to delete many to many
    - Where is the leaderId? on cult model
    - get cult member, then use cult member to get the cult
    - cultmemberSErvice --> CultMember cultMember = _repo.GetOneCultMember(cultMemberId)   // declare to cmrepo
      - repo --> straightforward 
    - back to service --> add if w/ null check, then go get the cult
    cult cult = _cultsService.GetOne(cultMember.CultId);
    if (userId != cult.LeaderId) throw new Exception...


InstaCult Client

  1. cultsService --> getCults
    -homepage -- onMounted calls getCults, w/ async function below.
    - see cults in console, now update service w/appstate, add cults to appstate,
    - cults in computed, dump {{cults}} to page
  
  2. created cult card
    - another way to do components - change script to script setup, get rid of export, write like js
    - this is simpler, but takes away some control from devs, gets rid of private functions.
    - in script - 
    const props = define props({cult: {type: Object, required: true}})
    - update template w/styling and details
  
  3. make cultpage - add route to router
  - cultDetailsPAge - getCult() calling to cultsService w/getById, but don't forget const route = useRoute
  - getCultMembers - call to cultsService
  - add both () to onMounted
  - in service - add functions, appstate uses activeCult, so update appstate w/ activeCult = null, same w/cultists = []
    - update return w/computeds for both
  - template styling for data
  - add button for joining cult, add @click, then create function in return. 
    - create cultMemberService
      - joinCult().... need cultId (backend takes care of accountId). 
      - in try: let cultMember = {cultId: route.params.id}
      await cultMembersService.joinCult(cultMember)
    - service --> pass in cultMember, 
    - send to api/cultMembers, cultMember
    - could null check cult.name, etc, but instead add v-if for cult
    - click join btn, check payload to see what data we sent up (cultId: 2)
    - don't want user to see join btn if not logged in, add computed for account, then add v-if="account"
      - didn't work because account is {}, and activeCult = null, so need to do account.id
  
  4. remove cult member
    - cult details page - add button w/ @click="removeMember"
    - need to tell it which member, so need to pass single instance through (c.cultMemberId). This destroys relationship between cult and account, not the actual cult and account destroyed.
  
  5. hide remove button using v-if="account.id == cult.leaderId"


Wednesday, Jan 25th, 2023
Help Reviews

* Interfaces - blueprint of a blueprint (blueprint of a class on server side)
  - not something we need to do, but can if we want

1. Made Restaurant model w/schema
  - skeleton of controller, service, INTERFACE, then repo
  - do interface before repo, called it IRepository though
    - this will dictate what ALL of our repos will look like. 
      public interface IRepository
      {
        List<Restaurant> Get();
        Restaurant GetOne();
        Restaurant Create();
        bool Update();
        bool Delete();
      }
  - then make RestaurantRepo : IRepository<Restaurant>
    * this errored, but right clicked and implemented interface and it declared the methods within the repo (deleted this to updated what data we're passing)
  - updated Irep<T, Tid>, put passed stuff into getOne, Update, Delete
  - updated restaurantrepo w/ <Restaurant, int> 
  - then declared interface again and now it has the methods declared w/more specifics for whats passed. Dictates bare minimum of what there should be.

2. Now that we have skeleton of files, lets set up sqldb
  - created restaurant table (don't forget to spin up sql db)
  - did one insert, checked table

3. controller --> setup privates, constructor,
  get request - regular
  - service --> pass in userInfo
  regular but add...
  List<Restaurant> filtered = restaurants.FindAll(r => !r.shutdown || r.OwnerId == userId);
  // if restaurant is not shutdown or if you are the owner, return those restaurants
  - repo - straightforward (select * from restaurants)

4. getone -->
  - controller - regular
  - service --> regular
  ACCESS CONTROL CHECKS
  // no restaurant at that id
    if(restaurant == null) {throw new exception("not a valid id.")}
  // restaurant is shut down and you're not owner
  if(restaurant.Shutdown && restaurant.OwnerId != userId) {throw new exception...}
  - repo --> 
  SELECT
  *
  FROM restaurants
  WHERE id = @id;

5. shutdown restaurant (could be done through put or delete, so let's do put)
  -controller --> 
    updateData.OwnerId = userInfo.Id // attaches these together so you can't update others
    - updateData.Id = id;   // sets the data to same as the original id
  - service --> Restaurant original = GetOne(updateData.Id, updateData.OwnerId)   // getone has some access checks already, so grabbing it here does this for our put
    if(original.OwnerId != updateData.OwnerId) throw new exception // doesn't let you update if it's not your restaurant
    original.Name = updatedData.Name ?? original.Name;    //etc
    * did shutdown here and errored out, so went to model and did null check bool?
    * this caused error in get one so had to updated restaurant.Shutdown to be == false
  - repo -->
  UPDATE restaurants SET
  name = @name;   //etc
  WHERE id = @id;
  ''; 
  int rows = _db.Execute(sql, update);
  return rows > 0;   // this returns a bool to show if anything changed/happened.

6.Create report model, decided to extend off account
  - made profile class w/id, name, picture, added coverImg. account extends profile, has email only
  - added public Profile Creator to report model

7. build out controller, service, repo (here this extends IRepository<Report, int id>), then implement interface
  - setup table dbsql w/foreign keys for creatorId and restaurantId
  - add insert
  - profile has brandimg, but didn't put in sql table, so alter table

8. Post report
  - controller and service --> regular
  - repo -> regular
  - back in controller, added reportDAta.creator = userInfo;

9. Delete report
  - regular controller
  - got to service and decided we needed getOne so we can use in our delete (grabbing one)
  - in repo for get one, did join for practice, but not totally necessary because it'll just be for backend for delete
    * this complicates the Report report, so look closely.
    - service for get one - check if null
    - continue delete in service, after Report report = GetOne(id), add access controls to check for ownership of report
  - repo --> regular

10. From restaurants controller, want to add get so we can get reports for a restaurant
  - list report that takes in int id
  - try -> List<Report> report = _reportsService.GetReportsByRestaurant(id, userInfo.Id)        
     // add to private/constructor
  - reportsService --> changed int id to restaurantId, also pass userId
    Restaurant restaurant = _restaurantsService.GetOne(restaurantId, userId)                             
     //add restaurantsService to private/constructor so we can run the getOne to handle access control
    List<Report> reports = _repo.GetReports(restaurantId)
  - repo - > 
    select
    r.*,
    a.*
    from reports r
    join accounts a on r.creatorId = a.id
    where r.restaurantId = @restaurantId       // we want to get all reports from single restaurant

11. postman tests
  - had to update some access controls to make sure logic was correct after running postman tests and not getting the messages/data we expected.

Thursday Jan 26th, 2023
Help Reviews - Front End

  
