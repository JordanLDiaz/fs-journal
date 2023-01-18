# MVC

Tuesday, Jan 17th, 2023

MySql
  - signed up for account on scalegrid.io
  - started up MySql table
  * MySQL tutorial - good reference and place to play around w/sql

  Sql: What is it?
    - db language, we'll use MySql
    - built on statements that can be ran, includes statement to create table.
    - creates a location where we will save the data/info
    - Mongo took care of this for us, but now we need to actually create it using Sql
    
    Table structure:
    name, data type, 
    CREATE TABLE syrups(
      id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
      name VARCHAR(20) NOT NULL COMMENT 'the name of the product',
      viscosity int NOT NULL DEFAULT 5, 
      color VARCHAR(15),
      flavor VARCHAR(20) NOT NULL DEFAULT 'Maple',
      canadian BOOLEAN NOT NULL DEFAULT true
    ); default charset utf8 COMMENT '';

    - hit execute, see table in another file...where did he go?
    - if you mess up, can get rid of table with DROP TABLE name of table.
    - cannot just add to a schema like we did w/ mongo
    - primary key = denotes that we need to order in table by the id

    Now that we have table, let's put info in it.
    - INSERT into syrups
    (name, viscosity, color, flavor, canadian)
    VALUES
    ("Grandma Sycamore's", 7, 'brown', 'maple', false);

    then execute, should see green message w/Affected rows: 1
    back in table, now have that data entered

    SELECT * FROM syrups
      - says select all of my columns from syrups, replace * with w/e column you want, can add multuple cols w/ , in between.
      - leave * if you want to see all cols
      - order you put cols in is order it will show in. can also put same col more than once if needed
      - name AS syrup_name (can put something like this if you want to update how col label is shown, doesn't actually change it for real, just how it's displayed)
    
    SELECT * FROM syrups WHERE viscosity < 8;  // will show only ones fitting requirements
      - can add multiple queries using AND to connect
        eg WHERE viscosity < 8 AND canadian = true;
    
    Want to order least to most viscose
      ORDER BY viscosity;

    Still need to connect sql to our projects, need to find connection string in scalegrid.io
      - put connection string in appsettings.Development file in project, update database name in carrots, as well as user and password

GregsList
1. setup file (dotnet auth), create car.cs, add public class Car w/schema
2. create new api CarsController, service, repo
3. Get Cars
  - controller --> httpget w/
  actionresult<List<Car>> Get() {}  w/try catch
  - update service w/ readonly, right click CarsController to declare constructor
  - generate get to repo, add private readonly CarsRepository _repo;
  - right click carsService to declare constructor
  - update internal List<Car> Get() w/ ...
  - repo --> private readonly IdbConnection _db;    (i says its interface of dbConnector...blueprint of a blueprint)
  - generate constructor
    - in get - has to have mysql, and run sql
    internal List<Car> Get()
    {
      string sql = @"
      SELECT 
      *
      FROM cars;
      ";              // don't save here or it will get rid of it
      List<Car> cars = _db.Query<Car>(sql).ToList();    // query will run sql statement we have above, ToList converts Ienumerable to list
      return cars;
    }

4. don't have table of cars yet, so do now. 
  - in dbSetup.sql add CREATE TABLE cars() w/what we put in cars.cs, included createdAt and updatedAt (from account)
  - don't forget default charset utf8mb4
  - add insert into cars(make, model, year, price, color, 'imgUrl', description)
  Values
  ('Toyota', 'Bugati', 2000, 12.99, '#000000', 'imgUrl' 'description');
  execute, continue adding more cars to table
  - go look at db, spin up server
  -tested on swagger, got error because of dependency injection, forgot to update startup, add transient or addscoped for service and repo

  * db connection string... 
  server=<YOUR Master Endpoint>;port=3306;database=<Database name>;user id=sgroot;password=<Password>
  - make sure this is updated and correct in order to work with swagger and postman
  - send first get request on postman

5. Post 
  - update CarsController w/ [HttpPost], actionresult, try catch
  - declare to service, make function
  - declare to repo, make function
    - in repo we add sql...
    internal Car Create(Car carData)
    string sql = @"
    INSERT INTO cars
    (make, model, year, price, imgUrl, description, color)
    VALUES
    (@make, @model, @year, @price, @imgUrl, @description, @color);     // carData will be passed alongside sql and will put it where @ is
    ";
    int id = _db.ExecuteScalar<int>(sql, carData);    //execute (run sql), scalar(expects result/single value, in this case int which is the id of thing we put in sql)
    carData.Id = id;
    return carData;    // we just get affected row: 1 over and over technically, so need to add to dbSetup this line...
    SELECT LAST_INSERT_ID();   (also add to repo after the @s under values... or only put here? idk)

  - make new post request, resend get request and should see new one. also can check sql table in vscode and should now get new car

6.Get One
  [HttpGet("{id}")]
  public ActionResult<Car> Get(int id)
  {
    try
    {
      Car car = _carsService.Get(id);
      return Ok(car);
    }
    catch (Exception e) 
    {
      return BadRequest(e.Message);
    }
  }
  * generate to service
  internal Car Get(int id)
  {
    Car car ....
  }
  * generate to repo w/ string sql = @"
  SELECT
  *
  FROM cars
  WHERE id = @id;
  ";
  Car car = _db.Query<Car>(sql, new {id})  // id gets passed in an object here so that it gets key value pair so that it knows the exact value it needs to use. also need to change it from iEnumerable to single car so add .FirstOrDefault(); immediately after {id}).
  - service --> update Get w/ 
  if(car == null) {        // cannot do if (!car) here, that's js truthy falsy, doesn't exist in sql
    throw new Exception("No car here.");
  }
  - make new get request in postman, appending id to url

7. Put 
  dbSetup--> 
  UPDATE  cars
  Set
    make = '🚤',
    model = 'speedboat', 
    year = ...... etc
  WHERE id = 7

8. Delete
  DELETE from cars;      // delete will delete all from cars so add...
  WHERE id = 6

9. Controller --> add httpDelete function, declare to service, etc etc.
{
  Car car = Get(id);
  int rows = _repo.Remove(id);
  return $"{car.Make} {car.Model} was removed"
}

repo --> 
{
  string sql = @"
  DELETE FROM cars
  WHERE id - @id;
  ";
  int rows = _db.Execute(sql, new {id});    // remember has to be id here so it knows which item in table based on its id
  return rows > 0;       //returns to the service a bool if it deleted 
}
* updated delete in service w/ bool deleted and if statement
* check w/postman test
* double check sql table to see its gone from there too.

10. Put
  - controller - add function w/actionResult, try catch (update)
  - generate to service
  {
    Car original = Get(id);
    original.make = carUpdate.make ?? original.make;        etc     // notice int variables are angry because ints cant be null in c#, so back in car.cs, null check those

    bool edited = _repo.Update(original);
    if(edited == false) {
      throw new Exception("Car was not edited");

    }
  - generate to repo
  {
    string sql = @"
    UPDATE cars
      SET
      make = @make,
      etc....
    WHERE id  = @id;
    ";
    int rows = _db.Execute(sql, original);
    return rows > 0;
  }


** to stop browser frrom opening new swagger each time you respin 
  vscode --> launch.json --> comment out serverReadyAction object
