# AngularJS Full-Stack generator [![Build Status](https://secure.travis-ci.org/DaftMonk/generator-angular-fullstack.svg?branch=2.0.0)](http://travis-ci.org/DaftMonk/generator-angular-fullstack)

> Yeoman generator for creating MEAN stack applications, using MongoDB, Express, AngularJS, and Node - lets you quickly set up a project following best practices.

## Usage

Install `generator-angular-fullstack`:
```
npm install -g generator-angular-fullstack
```

Make a new directory, and `cd` into it:
```
mkdir my-new-project && cd $_
```

Run `yo angular-fullstack`, optionally passing an app name:
```
yo angular-fullstack [app-name]
```

Run `grunt` for building, `grunt serve` for preview, and `grunt serve:dist` for a preview of the built app.

## Supported Configurations

**Client**

* Scripts: `JavaScript`, `CoffeeScript`
* Markup:  `HTML`, `Jade`
* Stylesheets: `CSS`, `Sass`, `Less`,
* Angular Routers: `ngRoute`, `ui-router`

**Server**

* Database: `None`, `MongoDB`
* Authentication boilerplate: `Yes`, `No` 
* oAuth integrations: `Facebook` `Twitter` `Google` 
* Socket.io integration: `Yes`, `No`

## Project Structure

Overview

    ├── client
    │   ├── app                 - All of our app specific components go in here
    │   ├── assets              - Custom assets: fonts, images, etc… 
    │   ├── components          - Our reusable components, non-specific to to our app
    │ 
    ├── e2e                     - Our protractor end to end tests
    │
    └── server
        ├── api                 - Our apps server api
        ├── auth                - For handling authentication with different auth strategies
        ├── components          - Our reusable or app-wide components
        ├── config              - Where we do the bulk of our apps configuration
        │   └── local.env.js    - Keep our environment variables out of source control
        │   └── environment     - Configuration specific to the node environment
        └── views               - Server rendered views

An example client component in `client/app`

    main
    ├── main.js                 - Routes
    ├── main.controller.js      - Controller for our main route
    ├── main.controller.spec.js - Test
    ├── main.html               - View
    └── main.less               - Styles

An example server component in `server/api`

    thing
    ├── index.js                - Routes
    ├── thing.controller.js     - Controller for our `thing` endpoint
    ├── thing.model.js          - Database model
    ├── thing.socket.js         - Register socket events
    └── thing.spec.js           - Test
    
## Injection

A grunt task looks for new files in your `client/app` and `client/components` folder and automatically injects them in the appropriate places based on an injection block. 

* `less` files into `client/app.less`
* `scss` files into `client/app.scss`
* `css` files into `client/index.html`
* `js` files into `client/index.html`
* `coffeescript` temp `js` files into `client/index.html`

## Generators

Available generators:

* All 
    - [angular-fullstack](#app) (aka [angular-fullstack:app](#app))
    - [angular-fullstack:openshift](#openshift)
    - [angular-fullstack:heroku](#heroku)
* Server Side
    - [angular-fullstack:endpoint](#endpoint)
* Client Side
    - [angular-fullstack:route](#route)
    - [angular-fullstack:controller](#controller)
    - [angular-fullstack:filter](#filter)
    - [angular-fullstack:directive](#directive)
    - [angular-fullstack:service](#service)
    - [angular-fullstack:provider](#service)
    - [angular-fullstack:factory](#service)
    - [angular-fullstack:decorator](#decorator)

### App
Sets up a new AngularJS + Express app, generating all the boilerplate you need to get started. 

Example:
```bash
yo angular-fullstack
```
###Openshift

Depending on your configuration, deploying to OpenShift can be done in just a few steps:

`yo angular-fullstack:openshift`

A live application URL will be available in the output.

> #### Additional Configuration 
> 
**Enabling web sockets**
If you're using socket.io, you will need to update the client to connect to the correct port for sockets to work. 
> 
In `/client/app/components/socket/socket.service` update the socket to connect to port 8000. (with `my-openshift-app` being the deployed name of your app):
>
    var ioSocket = io.connect('my-openshift-app.com:8000');
> 
**oAuth**
If you're using any oAuth strategies, you must set environment variables for your selected oAuth. For example, if we're using Facebook oAuth we would do this :
> 
    rhc set-env FACEBOOK_ID=id -a my-openshift-app
    rhc set-env FACEBOOK_SECRET=secret -a my-openshift-app
> 
After you've set the required environment variables, restart the server: 
> 
    rhc app-restart -a my-openshift-app
    
**Pushing Updates**

    grunt build
    
Commit and push the resulting build, located in your dist folder:

    cd dist && git add -A && git commit -m "describe your changes here"
    git push -f my-openshift-app master

### Heroku

Deploying to heroku only takes a few steps.

    yo angular-fullstack:heroku 

To work with your new heroku app using the command line, you will need to run any `heroku` commands from the `dist` folder.

If you're using mongoDB you will need to add a database to your app:

    heroku addons:add mongohq

Your app should now be live. To view it run `heroku open`

> #### Additional Configuration 
>
> 
**Enabling web sockets**
If you're using socket.io you will need to enable websockets on your app: 
>
    heroku labs:enable websockets
**oAuth**
If you're using any oAuth strategies, you must set environment variables for your selected oAuth. For example, if we're using Facebook oAuth we would do this :
> 
    heroku config:set FACEBOOK_ID=id
    heroku config:set FACEBOOK_SECRET=secret

**Pushing Updates**

    grunt build
    
Commit and push the resulting build, located in your dist folder:

    cd dist && git add -A && git commit -m "describe your changes here"
    git push -f heroku master


### Endpoint
Generates a new API endpoint.


Example:
```bash
yo angular-fullstack:endpoint message
[?] What will the url of your endpoint to be? /api/messages
```

Produces:
    
    server/api/message/index.js
    server/api/message/message.spec.js
    server/api/message/message.controller.js
    server/api/message/message.model.js  (optional)
    server/api/message/message.socket.js (optional)

### Route
Generates a new route.

Example:
```bash
yo angular-fullstack:route myroute
[?] Where would you like to create this route? client/app/
[?] What will the url of your route be? /myroute
```

Produces:
    
    client/app/myroute/myroute.js
    client/app/myroute/myroute.controller.js
    client/app/myroute/myroute.controller.spec.js
    client/app/myroute/myroute.html
    client/app/myroute/myroute.scss
   

### Controller
Generates a controller.

Example:
```bash
yo angular-fullstack:controller user
[?] Where would you like to create this controller? client/app/
```

Produces: 

    client/app/user/user.controller.create
    client/app/user/user.controller.spec.js

### Directive
Generates a directive.

Example:
```bash
yo angular-fullstack:directive myDirective
[?] Where would you like to create this directive? client/app/
[?] Does this directive need an external html file? Yes
```

Produces:

    client/app/myDirective/myDirective.directive.js
    client/app/myDirective/myDirective.directive.spec.js
    client/app/myDirective/myDirective.html
    client/app/myDirective/myDirective.scss

**Simple directive without an html file**

Example:
```bash
yo angular-fullstack:directive simple
[?] Where would you like to create this directive? client/app/
[?] Does this directive need an external html file? No
```

Produces:

    client/app/simple/simple.directive.js
    client/app/simple/simple.directive.spec.js

### Filter
Generates a filter.

Example:
```bash
yo angular-fullstack:filter myFilter
[?] Where would you like to create this filter? client/app/
```

Produces: 

    client/app/myFilter/myFilter.filter.js
    client/app/myFilter/myFilter.filter.spec.js

### Service
Generates an AngularJS service.

Example:
```bash
yo angular-fullstack:service myService
[?] Where would you like to create this service? client/app/
```

Produces:

    client/app/myService/myService.service.js
    client/app/myService/myService.service.spec.js


You can also do `yo angular:factory` and `yo angular:provider` for other types of services.

### Decorator
Generates an AngularJS service decorator.

Example:
```bash
yo angular-fullstack:decorator serviceName
[?] Where would you like to create this decorator? client/app/
```

Produces 

    client/app/serviceName/serviceName.decorator.js

## Bower Components

The following packages are always installed by the [app](#app) generator:

* angular
* angular-bootstrap
* angular-cookies
* angular-mocks
* angular-resource
* angular-sanitize
* angular-scenario
* bootstrap
* es5-shim
* font-awesome
* json3
* jquery
* lodash

These packages are installed optionally depending on your configuration:

* angular-route
* angular-ui-router
* angular-socket-io

All of these can be updated with `bower update` as new versions are released.

## Configuration
Yeoman generated projects can be further tweaked according to your needs by modifying project files appropriately.

A `.yo-rc` file is generated for helping you copy configuration across projects, and to allow you to keep track of your settings. You can change this as you see fit.

    {
      "generator-angular-fullstack": {
        "insertRoutes": true,
        "registerRoutesFile": "server/routes.js",
        "routesNeedle": "// Insert routes below",
        "insertSockets": true,
        "registerSocketsFile": "server/config/socketio.js",
        "socketsNeedle": "// Insert sockets below",
        "filters": {
          "js": true,
          "html": true,
          "sass": true,
          "uirouter": true,
          "socketio": true,
          "mongoose": true,
          "auth": true
        }
      },
      "generator-ng-component": {
        "routeDirectory": "client/app/",
        "directiveDirectory": "client/app/",
        "filterDirectory": "client/app/",
        "serviceDirectory": "client/app/",
        "basePath": "client",
        "filters": [
          "uirouter"
        ],
        "extensions": [
          "js",
          "html",
          "scss"
        ]
      }
    }
    
## Testing

Running `grunt test` will run the client and server unit tests with karma and mocha.

Use `grunt test:server` to only run server tests.

Use `grunt test:client` to only run client tests.

**Protractor tests**

To setup protractor e2e tests, you must first run 

`npm run update-webdriver`

Use `grunt test:e2e` to have protractor go through tests located in the `e2e` folder.

## Environment Variables

Keeping your app secrets and other sensitive information in source control isn't a good idea. To have grunt launch your app with specific environment variables, add them to the git ignored environment config file: `server/config/local.env.js`.

## Contribute

See the [contributing docs](https://github.com/DaftMonk/generator-angular-fullstack/blob/master/contributing.md)

When submitting an issue, please follow the [guidelines](https://github.com/yeoman/yeoman/blob/master/contributing.md#issue-submission). Especially important is to make sure Yeoman is up-to-date, and providing the command or commands that cause the issue.

When submitting a PR, make sure that the commit messages match the [AngularJS conventions](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/).

When submitting a bugfix, try to write a test that exposes the bug and fails before applying your fix. Submit the test alongside the fix.

When submitting a new feature, add tests that cover the feature.

See the `travis.yml` for configuration required to run tests.

## License

[BSD license](http://opensource.org/licenses/bsd-license.php)