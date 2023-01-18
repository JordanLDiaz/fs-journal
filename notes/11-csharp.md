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