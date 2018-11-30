node - request

```
var request = require("request");

var options = { method: 'GET',
  url: 'http://www.xxx.com/menu',
  qs: { name: 'card' },
  headers: 
   { 
     'Cookie': 'xxxxxxx',
     'cache-control': 'no-cache' } };

request(options, function (error, response, body) {
  if (error) throw new Error(error);

  console.log(body);
});

```
