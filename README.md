# nodejs-captcha
Creates Captcha in base64 format

# installation
`npm install nodejs-captcha`

# usage
```javascript 
// import library
var captcha = require("nodejs-captcha");

// Create new Captcha
var possible_options=[
  //a string as the argument below(will be the text used in the captcha)
  "specific t3xt",
  //an object as the argument below(with all the possible keys)
  {
    length:11, value:"thegr3ywo1f", charset:['a','b','1','2'], //charset would only be used if there is no value
    width:100, height:40, numberOfCircles:10, image:"base64string"  //I don't know why you would manually set these(in this line) but it IS possible
  },
  //a number used as the argument below(will be the length of text used in the captcha)
  6,
  //undefined as the argument below(nothing, like captcha())
  undefined
]
let options=possible_options[ Math.floor(Math.random()*possible_options.length) ]
var newCaptcha = captcha( options );

// Value of the captcha
var value = newCaptcha.value

// Image in base64 
var imagebase64 = newCaptcha.image;

// Width of the image
var width = newCaptcha.width;

// Height of the image
var height = newCaptcha.heigth;

```
### sample usage with nodejs http
``` javascript

"use strict";
var http = require("http");
var captcha = require("nodejs-captcha");
var PORT = 8181;

function handleRequest(req, res) {
  if (req.method === "GET" && (req.url === '/' || req.url.indexOf("index") > -1)){
    let result = captcha();
    let source = result.image;
    res.end(
      `
    <!doctype html>
    <html>
        <head>
            <title>Test Captcha</title>
        </head>
        <body>
        <label>Test image</label>
        <img src="${source}" />
        </body>
    </html>
    `
    );
  }else{
      res.end('');
  }
}

//Create a server
var server = http.createServer({}, handleRequest);

//Start server
server.listen(PORT, function() {
  console.log("Server listening on: https://localhost:" + PORT);
});
```

It is recommended to store the value of the captcha in order to check the validity of the user's answer to challange
