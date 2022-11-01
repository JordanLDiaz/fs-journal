# JavaScript
Monday, October 31st, 2022

-JS file names - app.js and others, but app.js is commonly used. Depends on preference
- After setting up file, always check that css is properly loading.
- Be sure to link JS file using a script tag in bottom of body so that it loads last 
  -(as you add more and more JS, it will take longer to load, so this way you show content to page first, and THEN the js loads last)
  - <script src="app.js">
    check with console.log ("fdlsa") inside script tag
  </script>
  - Check console log with dev tools - console (MONITOR CONSTANTLY)

JS land 
  - No longer talking about content on page, so don't forget to make content strings when needed so things actually show

Data Types
- Primitive Types
  - String
    - let string = "string"
  -Numbers
    - let number = 2
    - let number2 = 54853
  Boolean (true or false)
    - let bool = true 
    - let bool = false
  Undefined
    - let dontKnow = undefined // unknown nothing (more unexpected or just variables that were never defined)
      Example: let variable1    (never defined variable)
  Null
    - let nothing = null // known nothing (confirmed nothing was there and I know that there is no value)
      - let variable2 = null ('I told you there was nothing there so why are you looking for something?')

  - Reference Types
    -Object (different from arrays, but still store info. objects store info by key, value pair. Objects don't contain a certain order, don't know which order it will go in. If order is important, don't use object)
    - link created between values
      - let obj = {} (curly brackets)
      - Example:
      let obj = {
        key: 'value'
        name: 'Mick'
        costume: 'Mega-Mind'
      }

      Dot-notation
        -obj.name // Mick
        -obj.costume // "Mega-Mind"


    - Array (items are accessed by position in array)
      - let arr = [] (array, square brackets, stores info by index/position/order, can contain strings, numbers, objects, etc.)
        - let numArr = [5, 10, 15]
        - let strArr = ['Howdy', 'Hey', 'Sup']
        - mixArr ['5', 10, ['sup]]    (not best practice!! AVOID)

    - Function data type
      - function doSomething () {
        console.log ('I did something')
      }
      do something()
      ** Don't forget to invoke function

      let funky = function () {
        console.log('this is a funny way to do it')
      }
      funky()

      let funcArr = [funky]

      Functions that return something (returns the value from function that was supplied to where function was called)
        -Example: function returnSomething() {
          return 'something'
        }

        function sum () {
          return 5 + 10
        }

How to link html buttons to js
  - Event handlers
    -Buttons
    - onclick="doSomething()"> press me </button>
    - onblur
    - onfocus (not a great one for buttons though)

    -Image
      - ondrag
      - ondragstart
      - ondragstop

Notes from tiny app
  - let secretCode = 'placed emojis here which is valid'
  - let yourCode = ''   (empty string)

  function ghost () {
    yourCode += 'ghostemoji'
    console.log(yourCode)
    drawCode()
  }
  (repeated 3 more times w/different emoji)
  - To actually run, make sure you invoke in your html and don't forget to console.log to make sure the output/data has changed as we expected it to
  - window.alert('correct')  <-- will alert user with pop-up box
  - function drawCode() {
    let codeElm = document.getElememtById('your-code)
    console.log(codeElm);
    codeElm.innerText = yourCode;
  } 

  debugger placed in curly brackets of function - allows you to pause code in action to see where bugs are 

emoji access = windows key .


Tuesday, November 1st Notes

Array Methods
- if you want to access info from array you need to go through the layers (like an onion)
Ex: catArr[2].livesLeft would return infinity (see breakroom example)


let catArr = [
  {
    name: 'Fido',
    age: 22,
    livesLeft: 0,
    color: 'calico tapioca',
    favoriteToys: ['mouse', 'hairball', 'laser']
  },
  {
    name: 'Yoshi',
    age: 3,
    livesLeft: 0,
    color: 'Grey',
  },
  {
    name: 'Morpheus',
    age: 8,
    livesLeft: Infinity,
    color: 'Brown striped',
  }
]

catArr[0].age   //22

for (let index = 0; index < catArr.length; index++) {
  console.log('looped', catArr[index])
}

// This would log data for all 3 cats

- Within for loop it gives const element = array[index]; 
    -What does this mean?
        - When it grabs array at index, it's grabbing out one element of the array
        - in this case it would be catArr[index] which is grabbing the objects (each cat) and its data. It's just assigning catArr = [index] as the element.
        - We should try to rename the 'element' to more closely match what the objects are. In this case it would be const cats. This is just an alias that makes code more clear
        - console.log('looped', cat.name) would spit out looped and the names of the cats

catArr.forEach((cat, i) => console.log(cat, i, 'for eached'))

console.log('found: ', catArr.find(cat => cat.name == 'Fido'))

Murder Mystery Notes

// Functions and Buttons
1. created animals array with objects representing each animal. 
2. updated html w/ container for lineup
3. in js, created function for drawAnimals and tested in console w/drawAnimals()
4. added animal lineup id to body
5. filtered animals by weapon type - claws
6. filtered for intellect but still returned same as claws. Need to make new function for each weapon type (filterClaws(), filterIntellect(), etc)
BREAK
1. Added more animals, then created filter functions for mammals, created buttons for mammal/not mammal
2. Created sort function for age, created buttonsfor old to young and young to old
3. created function to filter by diet (herbivores, carnivores). Diet was listed more specifically as meat/veggies, etc so we need to do extra here.
  Filter for herbivores (strictly)
  let herbivores = animals.filter(animal => (animal.diet.includes('vegetables') || animal.diet.includes('fruits')) && !(animal.diet.includes('meats') || animal.diet.includes('fishes') || animal.diet.includes('bugs')))
              // or could use && to say has both instead of || which is OR
  Filter for carnivores (strictly)
  let carnivores = animals.filter(animal => animal.diet.includes('meats') || animal.diet.includes('fishes') || animal.diet.includes('bugs')) && ! (animal.diet.includes('vegetables') || animal.diet.includes('fruits')
  Filter for omnivores (didn't mess with this but would be a good question to see how )
  let omnivores = animals.filter(animal => animal.diet.includes('meats') && animal.diet.includes('vegetables'))

4. created buttons for herbivores, carnivores, omnivores,
5. updated drawAnimals function so that it doesn't keep redrawing all animals over and over.
6. added drawSuspects function   (drawSuspects(suspects)) to target new arrays that are made when accessing the different properties

//Game Logic
1. created doCrime function - will grab the guilty property and change it to true. Added doCrime() to end of js after drawAnimals(). This is what randomizes the guilty animal. 
2. Added clues section in html and getClue () in js using find method
3. added swtich(randomClue) to getClue 

innertext vs. innerhtml   (this string is no longer text, it's html   if you switch to html. This allows you to put a p tag within the string which renders it as a p tag to the page)
- if you see an actual p tag on the page it means you didn't change it to innerhtml
- this formats the clues on the page much more nicely since it's creating new elements as clues are given. 

Using `` allows you to do string interpolation (sp?) which treats it like javascript and has to go search for the info in the object.
    e.g. clueElm.innerHTML += `<p>the murdered is ${guiltyanimal.age}<p>`

4. added accuse()

Takeaway - remember how to use .filter, sort, find, etc.
