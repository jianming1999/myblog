```
'use strict';
//这是一个简单的应用
var path         = require('path');
var fs = require("fs") ;
global.l = console.log;
 
//检查某个目录是否存在
var stat = fs.statSync(path.join(__dirname,'content'));
l(stat.isDirectory());//为true的话那么存在，如果为false不存在
//检查某个文件是否存在
try{
    fs.statSync(path.join(__dirname, 'content/a1.txt'));
    //如果可以执行到这里那么就表示存在了
    console.log('haode');
}catch(e){
    //捕获异常
}
```
