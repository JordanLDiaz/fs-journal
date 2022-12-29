# Deploying Applications

**1.** What is the package.json file used for?
<!-- enter you answer in the space below -->
```
The package.json file keeps record of important metadata regarding the project and defines functional attributes that npm uses to install dependencies, run scripts, and identify the entry point to the package.
``` 
**2.** At what level of your project do you need package.json when deploying your application? Why?
<!-- enter you answer in the space below -->
```
The package.json file should be placed at the top level of the project so that any necessary modules are able to be imported wherever they are needed in the application. 
```
**3.** What command will ensure that your Vue code is compiled properly for deployment?
<!-- enter you answer in the space below -->
```
We run the npm i command to install the needed node modules.
```
**4.** _______ are used to provide your application with specific data based on it's environment. For example: connections strings, private keys or port. Fill in the blank.
<!-- enter you answer in the space below -->
```
Env files
```
**5.** What are the two ways to view the logs from your Heroku app.
<!-- enter you answer in the space below -->
```
You can view logs via the dashboard, logging add-on, or in your log drain. 
```
**6.** How do you update an app already deployed on Heroku?
<!-- enter you answer in the space below -->
```
Clone the repository from Github, make changes and commit them to Github, login to your Heroku account, set remote for the project, and then push it to the Heroku master to deploy the updates. 
```
**7.** Why is branching important to version control?
<!-- enter you answer in the space below -->
```
Branching is important to version control because it helps to prevent merge conflicts and encourages developers to code small to prevent any major changes that could negatively impact the app. 
```
**8.** When should code review happen?
<!-- enter you answer in the space below -->
```
Code review should happen after successful tests are run and before the code is pushed to the main branch.
```
**9.** What is the term used to define combining two branches?
<!-- enter you answer in the space below -->
```
The term used to define combining two branches is git merge. 
```