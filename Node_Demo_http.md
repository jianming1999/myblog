node-http

```
var http = require("http");

var options = {
  "method": "GET",
  "hostname": "www.xxx.com.cn",
  "port": null,
  "path": "/project/xxx/menu?name=card",
  "headers": {
    "cache-control": "no-cache",
    "postman-token": "b0fefa22-1ec1-8624-359d-f9eb06643261"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();

```
