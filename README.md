# jsreport-express
[![Build Status](https://travis-ci.org/jsreport/jsreport-express.png?branch=master)](https://travis-ci.org/jsreport/jsreport-express)

> jsreport extension adding API and studio

`jsreport-express` is the main extension you need when you want to add jsreport studio or API. Many other extensions works in conjunction with `jsreport-express` and extends studio ui or API. Just to name some of them:

- [jsreport-templates](https://github.com/jsreport/jsreport-templates)
- [jsreport-data](https://github.com/jsreport/jsreport-data)
- [jsreport-scripts](https://github.com/jsreport/jsreport-scripts)
- [jsreport-statistics](https://github.com/jsreport/jsreport-statistics)
- [jsreport-authentication](https://github.com/jsreport/jsreport-authentication)

And many others. Where some of them are working also without `jsreport-express` and some of them doesn't.  This extension is designed to be just a wrapper for ui and it doesn't work standalone. The minimal configuration requires at least [jsreport-templates](https://github.com/jsreport/jsreport-templates) to be installed.

##Configuration

`jsreport-express` uses some options from the global configuration:

`httpPort` (number) - http port on which is jsreport running, if both httpPort and httpsPort are specified, jsreport will automaticaly create http redirects from http to https, if any of httpPort and httpsPort is specified default process.env.PORT will be used

`httpsPort` (number) - https port on which jsreport is running

`certificate` object - path to key and cert file used by https

##Attaching to existing express app
`jsreport-express` by default creates a new express.js application and starts to listen on specified port. In some cases you may rather use your own express.js app and just let `jsreport-express` to add specific routes to it. This can be done in the following way:

```js
var jsreport = require('jsreport-core')(),//or just jsreport
    express = require('express');

var app = express();

app.get('/', function (req, res) {
  res.send('Hello from the main application');
});

var reportingApp = express();
app.use('/reporting', reportingApp);

jsreport.init({
  express: { app :reportingApp } 
}).start().then(function() {
  app.listen(3000);
});
```