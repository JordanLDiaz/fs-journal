# MVC
Week 2
Monday, November 7th
Classes with ES6

MVC -
* Grouping/encapsulating code that works/fits together with like functionality

'View' - what the user sees. 
  - Includes HTML, DOM, index.html
  - HTML representation of data in the app

'Controller' - first layer of application
  - Takes in actions from user
  - Draws data to DOM (where it lands)
  - controller is the only thing you give user access to (other than screen updates w/html)

'Service' - layer where business logic happens
  - Anything that modifies data = business logic

'AppState' - single source of truth 
  - Where all the apps data will be stored
  - where you keep all your variables ('data store')

'Model' - 
  - blueprint of what data in the appstate will look like

**BCW serve instead of mkdir and touch in cp**
- This auto creates all the files/templates you need. 

- Organize files in VSCode by the order in which MVC works, eg: index --> valuesController --> valuesService --> appstate --> value
- Any of the javascript files that are in templates, DONT CHANGE, these are just to reference

Combat Tracker Notes
- Start at the model - the blueprint that tells us what things will even look like.
- Start w/ appstate.js (ONLY ONE PER PROJECT)
  - Within this, created character.js and put:
  - export class Character {

      constructor(name, tyoe, maxHealth, currentHealth)
        this.name = name
        this.type = type
        this.maxHealth = maxHealth
        this.currentHealth = currentHealth
  }

  - added constructor inside export class - these are the things that make the character a character
  - class character itself is a function - just defining what it will be, not actually creating it yet. 
  - this = special word that talks about itself, takes info that we will later provide and attach it to this character. 

  back in app.js, created spaot to save our data:
  - classes are made through 'new' keyword
  - when you added new Character below, it automatically added an import tag at top of appstate.js. if it doesn't, something isn't linked/typed correctly. DONT TYPE THESE YOURSELF! 
  - parentheses at end of new Character() contains the info you want to add, and tells it to do the function

    characters = [
      new Character('George', 'Dwarf Druid', 24, 24),
      new Character('Strahd', 'Vampire', 240, 240),
    ]

    -Added new file = charactersController.js, added export class (see below)
    - added drawCharacters method, but so far don't have characters here, so how do I bring it here? Import appState.js by adding it to front of characters.forEach. this will add import line at top of file automatically. 


    export class CharactersController {

      drawCharacters() {
        //TODO i need the characters
        let template = ''
        appState.characters.forEach( c=> template += `

        `
        )
        document.getElementById('app').innerText = template       
      }

    }

  app.js is 'doorway to our application', needs to be linked 
  Added: 
    class App {
      charactersController = new CharactersController()    //notice lowercase changes to capitalized. Capitalized is just the blueprint, lowercase is the actual objec that has been built out
    }

  - missed a couple steps (confused), but...
   then added character formatting w/ string interpolation in charactersController.js (within the for each)

  