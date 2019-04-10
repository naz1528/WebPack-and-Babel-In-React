# WebPack-and-Babel-In-React
WebPack and Babel In React
                                             Why do we need Babel?
                                             
Answer is easy —for latest ECMAScript syntax support (like ES5, ES6). React.js library itself insists you to make use of the latest JavaScript’s offerings for cleaner, less and more readable code. But unfortunately our browsers do not understands most of the syntax and this is where we need Babel’s help. It is responsible for converting the ES5 and ES6 code to browser understandable code, basically backward compatibility. Solves our problem (so you need to know ES5 and ES6 syntax for React.js app development,in case if you don’t). 
The Babal packages that we are gonna use are:

                               babel-core: Well as the name suggests the main engine of babel plugin for its dependents to work.
                               babel-preset-env: This is the ES5, ES6 supporting part
                               babel-preset-react: Babel can be used in any framework that needs latest JS syntax support, in our case its                                “React”, hence this preset.
                               babel-loader: Consider this as a bridge of communication between Webpack and Babel
                               
                               
                               Why do we need Webpack?
                               
 Well we don’t necessarily need webpack to work with React, other alternatives could be Browserify, Parsel, Brunch, etc, but honestly I don’t know how well they fit in with React.js. Webpack is most widely used and an accepted module bundler and task runner through out React.js community. You will find solutions to most of the problems related to it, its community is vibrant. Also its quite easy and minimal. So why not Webpack. We need its packages for following reasons:
 
                             webpack: The main webpack plugin as an engine for its dependents.
                    webpack-cli: To access some webpack commands through CLI like starting dev server, creating production build, etc.
                    webpack-dev-server: A minimal server for client-side development purpose only.
                    html-webpack-plugin: Will help in creating HTML templates for our application.
                    
                    Trust me you don’t need to memorize all those plugins, just go through it once and understand what they does. At the end of this article they all will be in your mind.
                    
                    
                    2.1— So lets start creating the app
Assuming you have Node.js installed in your system and npm commands are accessible though your terminal. 
Lets say our current directory is c:\

Create our application directory "app" and cd app into it. Now we are inside c:\app directory
npm init and finish the process. package.json should get created inside (current)app directory
Install our main (dependencies)React packages npm i -S react react-dom 
As you can see there are 2 packages react is obviously the main engine of the React.js framework and as we re going to create app for browsers, we are using react-dom. We would have used react-native if we were to build mobile apps (which is not our case).
Install Babel packages as dev dependency, explained above in point 1.1
npm i -D babel-core babel-loader babel-preset-env babel-preset-react
Babel will require a tiny piece of configuration, we will do it later
Install Webpack packages as dev dependencies, mentioned in point 1.2
npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin 
Webpack needs some small configuration, that we will do later.
In package.json the dependencies and devDependencies should look like this.

                                  {
                                   “name”: “client”,
                                   “version”: “1.0.0”,
                                   “description”: “”,
                                   “main”: “index.js”,
                                   “scripts”: {
                                   “start”: “”,
                                   “build”: “”
                                   },
                                   “author”: “Austin4Silvers”,
                                   “license”: “ISC”,
                                   “dependencies”: {
                                       “react”: “¹⁶.4.1”,
                                       “react-dom”: “¹⁶.4.1”
                                   },
                                   “devDependencies”: {
                                       “babel-core”: “⁶.26.3”,
                                       “babel-loader”: “⁷.1.5”,
                                       “babel-preset-env”: “¹.7.0”,
                                       “babel-preset-react”: “⁶.24.1”,
                                       “html-webpack-plugin”: “³.2.0”,
                                       “webpack”: “⁴.16.2”,
                                       “webpack-cli”: “³.1.0”,
                                       “webpack-dev-server”: “³.1.5”
                                   }
                                  }
And one big step is over! The Installation phase. Our next step is to create the configuration files which will act as a leash on Webpack and Babel, providing them the necessary information within which they should operate.

3.1— Configuring Babel
Considering you are in c:\app , create a file .babelrc here, note the name of the file should be exactly the same and the content of this file should be following:

{"presets":["env", "react"]}
This is the configuration file babel looks up for. Remember we had 2 babel presets installed in step: 4 of section: 2.1? Those presets should be mentioned here for Babel to know.

3.2— Configuring Webpack
Considering you are in c:\app directory, create a file webpack.config.js , note the name of the file should be same, the contend will be the following:

                                                      01| const path = require(‘path’);
                                                      02| const HWP = require(‘html-webpack-plugin’);
                                                      03| module.exports = {
                                                      04|    entry: path.join(__dirname, ‘/src/index.js’),
                                                      05|    output: {
                                                      06|        filename: ‘build.js’,
                                                      07|        path: path.join(__dirname, ‘/dist’)},
                                                      08|    module:{
                                                      09|        rules:[{
                                                      10|           test: /\.js$/,
                                                      11|           exclude: /node_modules/,
                                                      12|           loader: ‘babel-loader’
                                                      13|        }]
                                                      14|    },
                                                      15|    plugins:[
                                                      16|        new HWP(
                                                      17|           {template: path.join(__dirname,‘/src/index.html’)}
                                                      18|        )
                                                      19|    ]
                                                      20| }
Lets go through the this webpack.config.js file.

Line #04 indicates the starting point of the application. In our case index.js resides inside c:\app\src directory, so create src directory inside app and create a new file index.js . Lets not put anything inside the file yet. By now you should have an empty file c:\app\src\index.js
Similarly create another file in the same directory c:\app\src and name it index.html , keep the file empty for now. This file will act as template file. Please remember React.js is best known for creating one page applications. So you see, this is the main template HTML file and the content inside this file is supposed to change from time to time whenever needed or whenever a user requests for change in view. To achieve this concept, we are using the package html-webpack-plugin which is imported in Line #02.
Line #05 to #07 In this section we have defined — in production what should be the name of the main file (build.js) and where will it be created (inside c:\app\dist) automatically.
Line #8 to #14 Within this section inside module we need to define the rules webpack should be bind to.
Rules in #10, #11 and #12 tells webpack to look for all .js files excluding the files inside the directory node_modules for the purpose of transpiling ES5, ES6 codes and use a loader called babel-loader for the purpose.
Line #15 to #19 — plugins — In case you are using any other plugins that webpack needs to know for the purpose of bundling or transpiling, you need to push the reference of the plugin within this array.
The 2nd and final piece of configuration is done! Now the only thing we are left with is displaying the “Hello world!”.

Remember in section 3.2 , point 1 we created two files index.html and index.js and kept them empty?

Lets start with adding content to c:\app\src\index.html file:

                                                        <!DOCTYPE html>
                                                        <html lang=”en”>
                                                        <head>
                                                         <meta charset=”UTF-8">
                                                             <meta name=”viewport” content=”width=device-width, initial-scale=1.0">
                                                             <meta http-equiv=”X-UA-Compatible” content=”ie=edge”>
                                                             <title>React app</title>
                                                        </head>
                                                        <body>
                                                              <div id=”root”></div>
                                                        </body>
                                                        </html>
Note: The only important part to notice here is the <div> with ID root . No matter how big your app is going to become, all that you’re gonna see in browser is going to reside inside this div.

Next lets add the React part of the app in the file c:\app\src\index.js

                                          01| import React from ‘react’;
                                          02| import ReactDOM from ‘react-dom’;
                                          03| const App = () => (
                                          04|   <div>
                                          05|      <h1>Hello world!!</h1>
                                          06|   </div>
                                          07| )
                                          08| ReactDOM.render(<App/>, document.getElementById(‘root’));
If you look up to the section 3.2 (configuring webpack), in the webpack.config.js we have informed webpack that this file index.js inside \src directory is the entry point of the app (line #4 of webpack.config.js)
and
Use index.html inside \src directory as the main template file.

The main part of the file is line #08. render() function is defined in package react-dom , which is responsible for rendering components in the browser. This line tells react-domto render the component <App/>in a DOM element with id="root"

Line#03 to #07 — const App- is a function that returns the “Hello world” within a div (FYI: This XML looking weird code inside JS function is called JSX syntax if its your first time with React.js) which gets rendered in <div id="root"></div> defined in c:\app\src\index.html.

Application is ready! Now all we have to do is fire up the webpack-dev-server to see our “Hello world”. For that lets open the file package.json from c:\app\package.json and update the index “scripts” in it:

“scripts”: {
   “start”: “webpack-dev-server — mode development — open — hot”,
   “build”: “webpack — mode production”
},
This says: If we trigger npm start , webpack-dev-server is going to fire up the application in mode=development, then display ( — open) it in your default browser automatically and keep watching for any changes made to the application ( — hot).
When npm run build is triggered, use webpack to create a production ready build in c:\app\dist directory. Done!

Here’s an example you refer to:
https://github.com/SiddharthaChowdhury/react-app-example
