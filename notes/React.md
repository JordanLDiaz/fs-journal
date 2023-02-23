Friday, February 17th 2023

React Template Notes
  - app.js - main thing that loads when app is loaded (default has navbar, outlet. make sure to reimport if you get rid of and put back)
  -  outlet is like router view (like in vue where routes map their content).
  - app is not what loads, main.jsx is what loads and pulls in/loads router (big deviation from how vue does it)
  
  Router 
    - register main element (app)
    - children [] has paths 

    - pages, components, router links work similarly to vue
    - path not case sensitive
    - main.jsx loads sass 

  Homepage 
    - react uses jsx syntax, functional components instead of class components. 
    - render function is within return
    - use className, not class
    - no "", use {} for jsland
    - useState is a function, we set what we want it to return
      - uses a twople(sp?) which is an array where first item will always be the variable that holds the data you started with,
      ex
      const [count, setCount] = useState(0)
      //const count = ref(0)
      //const setCount = (val) => count.value = val

  Appstate
    - uses mobx library that has actions and makeAutoObservable
    - constructor has makeAutoObservable(this)
    
  Homepage
    - getHouses() calls to api, set const houses w/ template in return
    - reactivity - after sending get request, got data in network, but nothing to page because of reactivity
    - homepage updated export at top to function getHouses(), then updated something at bottom
    - now network is showing tons of requests to sandbox, so had to add useEffect hook event on homepage (think of onMounted)
    - move getHouses() inside useEffect 
    - every time houses changes, it will recall. 

  * on new component, use ! to choose type of template you want.
  99% of time, components don't need to be observers

  Styling
    - has style components that are imported to respective components
    
