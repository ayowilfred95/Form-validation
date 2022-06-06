# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can't go back!**

If you aren't satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you're on your own.

You don't have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn't feel obligated to use this feature. However we understand that this tool wouldn't be useful if you couldn't customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)


In this tutorial, you’ll learn how to polyfill node core modules in webpack version 5 and above using the react-app-rewired package, installing the required dependencies, and overriding the default webpack configuration.

What causes the Polyfill node core module error?
Until the latest update to webpack version ___, webpack < 5 used to include NodeJS polyfills by default. Because the current version of webpack no longer includes NodeJS polyfills by default, it is causing issues for developers that use create-react-app with webpack > 5 to build applications 

4 easy steps to fix polyfill node core modules in webpack 5
The main issue with create-react-app and the polyfill error is that create-react-app, by default, hides the webpack config file inside the node-modules, and by doing so, generates the file at build time leaving developers unable to modify it.

Luckily there is a package, react-app-rewired, that allows developers to easily edit the webpack config file and fix the polyfill node core module error.

1. Install react-app-rewired
First, install the reach-app-rewired package with your preferred package manager.

Install react-app-rewired package with yarn: yarn add --dev react-app-rewired
Install react-app-rewired package with npm: npm install --save-dev react-app-rewired

2. Install missing dependencies
Next, install these missing dependencies:

crypto-browserify
stream-browserify
assert
stream-http
https-browserify
os-browserify
url
Install missing dependencies with yarn:

yarn add process crypto-browserify stream-browserify assert stream-http https-browserify os-browserify url buffer

Install missing dependencies with npm:        npm install --save-dev crypto-browserify stream-browserify assert stream-http https-browserify os-browserify url buffer process

3. Override the create-react-app webpack config file
In the root folder of your project, create a new file called config-overrides.js, and add the following code to it:

3. Override the create-react-app webpack config file
In the root folder of your project, create a new file called config-overrides.js, and add the following code to it:


 
const webpack = require('webpack'); 
module.exports = function override(config) { 
		const fallback = config.resolve.fallback || {}; 
		Object.assign(fallback, { 
    	"crypto": require.resolve("crypto-browserify"), 
      "stream": require.resolve("stream-browserify"), 
      "assert": require.resolve("assert"), 
      "http": require.resolve("stream-http"), 
      "https": require.resolve("https-browserify"), 
      "os": require.resolve("os-browserify"), 
      "url": require.resolve("url") 
      }) 
   config.resolve.fallback = fallback; 
   config.plugins = (config.plugins || []).concat([ 
   	new webpack.ProvidePlugin({ 
    	process: 'process/browser', 
      Buffer: ['buffer', 'Buffer'] 
    }) 
   ]) 
   return config; }


This config-overrides.js code snippet is telling webpack how to resolve the missing dependencies that are needed to support web3 libraries and wallet providers in the browser.

4. Override package.json to include the webpack configuration
Within the package.json file, replace react-scripts with react-app-rewired scripts for the three following scripts fields to update the webpack configuration:

start
build
test
Here’s what the package.json file looks like before replacing the react-scripts:

 
"scripts": { 
	"start": "react-scripts start", 
  "build": "react-scripts build", 
  "test": "react-scripts test", 
  "eject": "react-scripts eject" 
 },

Here’s the package.json file after replacing the react-scripts with react-app-rewired scripts:


 
"scripts": { 
	"start": "react-app-rewired start", 
  "build": "react-app-rewired build", 
  "test": "react-app-rewired test", 
  "eject": "react-scripts eject" 
 },
That’s it!

Now, the polyfill node core module error should be fixed, missing NodeJS polyfills should be included in your app, and your app should work 
