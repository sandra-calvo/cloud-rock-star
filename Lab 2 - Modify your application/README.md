# Lab 2 - Modify your application

In this lab you will create your first Node.js application using Cloud Foundry Public environments. 

  - [Need to know](#need-to-know)
  - [Application components](#application-components)
  - [Modify your code](#modify-your-code)
  - [Summary](#summary)
  

## Need to know

**npm**: Node Package Manager (npm) is the default package manager for the JavaScript runtime environment Node.js. 

**XXXXXXX**: 

## Application components

Your delivery pipeline is ready and now we will access the source code. This toolchain includes tools to develop and deploy the application. 

1. In the toolchain overview click on the Git box to access the code. 

**THINK**: This phase is for planning the application by creating bugs, tasks, or ideas by using the Issue Tracker, which is part of the Git repository. 

**CODE**: This phase is for the implementation of the application by providing a GIT repository as source code management system, and a Web IDE (Eclipse Orion) to edit your code online. In the repository, you can specify whether to clone a repository or start from scratch by selecting New in the repository type. 

**DELIVER**: This phase is for configuring the delivery pipeline. It allows you to specify automatic build, deployment, and testing of your code after a developer pushes new code to the Git repository. 

<img src="/images/toolchain-ready2.png" width="80%" height="80%">

If you closed the toolchain tab you can go to IBM Cloud dashboard -> Cloud Foundry Apps -> Your application -> View Toolchain.

<img src="/images/application-toolchain.png" width="100%" height="100%">

2. You have accessed your sample application source code. 

Usually the development would happen locally and when changes have been tested and approved it would be pushed to the cloud. In this lab all changes in the code will be done in the IBM Cloud to simplify the development phase. 

<img src="/images/nodejs-toolchain.png" width="100%" height="100%">

3. Now we will get familiar with the Node.js application components. 

- **public folder**: This folder contains the HTML code for your application's user interface (UI), the css style sheets and the images used in the UI. 

- **index.html**: 

```html
<!DOCTYPE html>
<html>

  <head>
    <title>NodeJS Starter Application</title>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="stylesheets/style.css">
  </head>

  <body>
    <table>
      <tr>
        <td style= "width:30%;">
          <img class = "newappIcon" src="images/newapp-icon.png">
        </td>
        <td>
          <h1 id="message">Hello World!</h1>
          <p class='description'></p> Thanks for creating a <span class = "blue">NodeJS Starter Application</span>.
        </td>
      </tr>
    </table>
  </body>

</html>
```

- **style.css.**: 

```css
/* style.css
 * This file provides css styles.
 */

body,html {
	background-color: #3b4b54; width : 100%;
	height: 100%;
	margin: 0 auto;
	font-family: "HelveticaNeue-Light", "Helvetica Neue Light", "Helvetica Neue", Helvetica, Arial, "Lucida Grande", sans-serif;
	color: #ffffff;
}

a {
	text-decoration: none;
	color: #00aed1;
}

a:hover {
	text-decoration: underline;
}

.newappIcon {
	padding-top: 10%;
	display: block;
	margin: 0 auto;
	padding-bottom: 2em;
	max-width:200px;
}

h1 {
	font-weight: bold;
	font-size: 2em;
}

.leftHalf {
	float: left;
	background-color: #26343f;
	width: 45%;
	height: 100%;
}

.rightHalf {
	float: right;
	width: 55%;
	background-color: #313f4a;
	height: 100%;
	overflow:auto;
}

.blue {
	color: #00aed1;
}


table {
	table-layout: fixed;
	width: 800px;
	margin: 0 auto;
	word-wrap: break-word;
	padding-top:10%;
}

th {
	border-bottom: 1px solid #000;
}

th, td {
	text-align: left;
	padding: 2px 20px;
}

.env-var {
	text-align: right;
	border-right: 1px solid #000;
	width: 30%;
}

pre {
	padding: 0;
	margin: 0;
}
```

- **.cfignore**: In this file, specify the files or file types you wish to exclude from upload. This is the most common way package authors prevent people from downloading extra files.

In this case we will ignore node_modules.

- **.project**:

- **LICENSE**: 

Feel free to view the License file in your application. 


- **README.md**: Contains information about other files in a directory or archive of computer software. A form of documentation, it is usually a simple plain text file called READ.ME, README.TXT, README.md.

In this case the README.md file cointains information on how to run this same application locally in our computer. 

```text
Node.js Hello World Sample
This application demonstrates a simple, reusable Node.js web application based on the Express framework.

Run the app locally

Install Node.js
cd into this project's root directory
Run npm install to install the app's dependencies
Run npm start to start the app
Access the running app in a browser at http://localhost:6001
```

- **app.js**: This is the entry JavaScript file for your application. 

```javascript
/*eslint-env node*/

//------------------------------------------------------------------------------
// node.js starter application for Bluemix
//------------------------------------------------------------------------------

// This application uses express as its web server
// for more info, see: http://expressjs.com
var express = require('express');

// cfenv provides access to your Cloud Foundry environment
// for more info, see: https://www.npmjs.com/package/cfenv
var cfenv = require('cfenv');

// create a new express server
var app = express();

// serve the files out of ./public as our main files
app.use(express.static(__dirname + '/public'));

// get the app environment from Cloud Foundry
var appEnv = cfenv.getAppEnv();

// start server on the specified port and binding host
app.listen(appEnv.port, '0.0.0.0', function() {
  // print a message when the server starts listening
  console.log("server starting on " + appEnv.url);
});
```
- **manifest.yml**: The manifest.yml includes basic information about your app, such as the name, how much memory to allocate for each instance and the route. It also contains information about the deployment of the application to IBM Cloud. 

```text
applications:
  - path: .
    name: cloud-rock-star
    environment_json: {}
    memory: 256M
    instances: 1
    disk_quota: 1024M
    services: []
```

- **package.json**: This file holds various metadata relevant to the project. This file is used to give information to npm (Node Package Manager) that allows it to identify the project as well as handle the project's dependencies. It can also contain other metadata such as a project description, the version of the project in a particular distribution, license information, even configuration data - all of which can be vital to both npm and to the end users of the package.

```javascript
{
	"name": "NodejsStarterApp",
	"version": "0.0.1",
	"private": true,
	"scripts": {
		"start": "node app.js"
	},
	"dependencies": {
		"express": "4.15.x",
		"cfenv": "1.0.x"
	},
	"repository": {},
	"engines": {
		"node": "10.*"
	} 
}
```

## Modify your code

4. It's time to modify your source code
- Go to the public folder and then open the index.html file. 
- Click 'Edit' to edit your file. 

<img src="/images/edit-index.png" width="100%" height="100%">

- In the line 19 change the text "Hello World!" to anything you want. 
- Then add a commit message at the end to record your changes in the source code. 
- When you are ready click 'Commit changes'. 

<img src="/images/change-text.png" width="100%" height="100%">

This will save your changes and start the delivery pipeline. 

5. Delivery pipeline running

If you checked the delivery pipeline before you could see it was not running before we modified our code. 

<img src="/images/pipeline-not-running-yet.png" width="70%" height="70%">

Once we commited the changes the build phase will start running. You can see your commit message is added to the build so last changes are recorded in the logs. 

<img src="/images/build-runnnig.png" width="100%" height="100%">

After few minutes your build and deploy phases will be ready and all changes will be visible in your application. 

<img src="/images/build-deploy-ready.png" width="100%" height="100%">

6. Visit your application to see the changes

<img src="/images/new-app.png" width="100%" height="100%">

**Congratulations, you modified your source code!** :+1:

## Summary

Lab 2 is complete! Good Job! 

In this lab you learned about the Node.js application components and modified the source code of the sample application. 

Now that you know how to modify your code, let's crete a cool application using open data. [Lab 3 - Add events API](link). 
