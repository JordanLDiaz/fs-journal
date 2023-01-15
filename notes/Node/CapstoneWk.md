Tuesday, Jan 10th, 2023

1. In vscode, open file, but not workspace.
  - We'll take our .client and move into the other client. 
  - check node version (needs to be 16 or above)
  - search for console logs and replace with loggers, make sure they import

2. open vite.config, make sure outDir is '../nameofproj/client'
  - open console, cd into the file above
  - npm i first if haven't already, then npm run build
  - should now be able to open up server folder, then client, and see all of the front end code. 

3. Go to render.com, sign up, new --> web service --> connect
  - name, region, 
  - change build command
    - in vscode, find package.json, look at start and build
    - update the build and start commands on render

4. advanced settings --> find in our .env file(server), update that in render.
  - need to match our env file exactly, add all of the strings
  - then create web service

* call out- many times build will fail because of incorrect file pathing...note that's especially true for image file names because it won't compute spaces in file names. have to rename any files with spaces if this is the case.
** May also fail if you forget to push the code!!!!

5. If build is successful, should now be able to use url to view live site.

6. build looked successful, but couldn't log in. 
  - Auth may not recognize thie site, so go to auth0 and add this new url to the allowed urls (in applications)
  

Wednesdsay, Jan 11th, 2023
Postman Tests

Reference: learning.postman.com - introduction

1. Find proj to work on, ad file in postman, update tokens. 
  - send first get request to see data
  - go to test tab (this is just js)

2. example get test 
let res = pm.response.json()
console.log('test time', res)

pm.test('status came back 200', () => {
  pm.expect(pm.response.code).to.be.eql(200, '')
})

3. showed example for post request test
4. delete request test
  - added prereq w/ let postCar = {} // {} has url, method, body
  - also added header w/ authorization outside of above {}
  - ended w/ pm.sendRequest(postCar)  (see let car in example... heavily modified from this)
  ** This posts a car first so that it's able to be deleted in tests.
  ** header also needs content type!

  - now ready for actual test.


Thursday Jan 12th 2023

Sockets
- Castle analogy - now we have wizards that open up "portals" to send certain messages straight to where we need them to go.
* can go to socket.io to read docs and also has practice chat room build

Steps
1. spin up proj, 
  - looked at socketService in client
  - env.js also has useSockets set to false, so now we want to change to true, refresh browser
    - console --> now see socket connection and socket playback, etc.
    -network --> ws (websocket), refresh, see messages about authenticating
  - in server side, there is socketprovider and testhandler which helps make sure they're working.

2. testhandler --> .on
  - on homepage, added onmounted w/ socketService.emit('SOCKET_SERVICE', 'message')
  - refresh, viewed network messages to see the messages. helps check that socket is working.

3. server --> testhandler --> viewed testEvent
- client --> socketService - added new listener (.on('IS_TESTED', this.runTest))
  - added runTest function
  - updated server testhandler with message instead of payload.

4. opened up private browser and in server testhandler, updated to io. Respun and saw that both browsers got message at same time when only one refreshes.

5. client --> homepage, added lightbulb mdi
  - added toggleLight to return
  - server --> handler --> added lightHandler, copied code from testhandler over, deleted old .on, added new on for light_toggle and this.toggleLight
  - declared method
  
6. setup LightHandler in client (had to make folder for handlers)
* handlers are like services, just for sockets
  - homepage --> import this
  - add to return the lightHandler.toggleLight
  - lightHandler --> add toggleLight, bring in socket service, emit magic string (light_toggle)
  - respin, test lightbulb, checked network and clicked button. should see bool toggling true/false
  - appstate --> added lightState: false
  - client --> socketservice --> add .on('LIGHT_TOGGLED', this.updateLightState), add updateLightState(payload)
  - check vue tools - clicking button should see lightstate toggle true false
  - in privatebrowser, can also click other button and the other browsers lightstate still toggles.

7. homepage --> added lighthandler.getLightState(),
  - return --> lightState computed, updated template w/ class for lightState so that color changes when toggled
  - lightHandler client --> add getLightState()  
  - lighthandler server --> added .on for this.checkLight, add function w/ this.socket.emit
  - client socketservice --> add .on listener for this.updateLightState
  - respin both browsers, test light. both should turn on! also saves state on reload.
  

