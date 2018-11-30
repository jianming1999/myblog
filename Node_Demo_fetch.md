node-fetch

```
var fetch = require('node-fetch');
var cheerio = require('cheerio');
fetch('http://www.baidu.com',{
	method: 'GET',
	headers: {
		'Cookie': 'xxx'
	}
}).then(function(res){
// fetch('http://www.baidu.com').then(function(res){
  console.log('then:\n');  
  return res.text();
}).then(function(res){
  console.log(res);
  // let $ = cheerio.load(res);
  // let h = new Function($('head>script').eq(0).html() + '; return h;')();
  // console.log('h:', typeof h);
}).catch(function(e){
  console.log('catch:',e);
})
```

