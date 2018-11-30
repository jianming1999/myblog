### 一、概述：

文件系统模块是一个简单包装的标准 POSIX 文件 I/O 操作方法集。可以通过调用 require("fs") 来获取该模块。文件系统模块中的所有方法均有异步和同步版本。

文件系统模块中的异步方法需要一个完成时的回调函数作为最后一个传入形参。

回调函数的构成由调用的异步方法所决定，通常情况下回调函数的第一个形参为返回的错误信息。

如果异步操作执行正确并返回，该错误形参则为null或者undefined。如果使用的是同步版本的操作方法，一旦出现错误，会以通常的抛出错误的形式返回错误。

可以用try和catch等语句来拦截错误并使程序继续进行。

### 二、文件操作说明

1、文件完整的读写

同步读取文件:如果同步读取文件失败，则需要使用try...catch 捕获该错误。

```
const fs = require('fs')
 
var data = fs.readFileSync('文件名称, 'utf-8')
try{
    var data = fs.readFileSync('文件名', 'utf-8')
}catch(err){
    console.log(error)
}
```

异步读取文件

```
var fs = require('fs');//引入fs模块
fs.readFile('文件名称（有路径加上路径）', 'utf-8', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
```

写文件

以下为异步模式下写入文件的语法格式：（如果文件存在，该方法写入的内容会覆盖旧的文件内容）

```
fs.writeFile(filename, data[, options], callback)
```

参数使用说明如下：

- path - 文件路径。

- data - 要写入文件的数据，可以是 String(字符串) 或 Buffer(流) 对象。

- options - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'

- callback - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回。

```
const fs = require(''fs)
 
fs.writeFile('被写入的文件名', data, function(err){
    if(err){
        console.log(err)
    }else{
        console.log('ok')
    }
})
```

2、从指定位置开始读取文件

- 1）打开文件

```
fs.open(path, flags[, mode], callback)
```

参数使用说明如下：

- path - 文件的路径。

- flags - 文件打开的行为。具体值详见下文。

- mode - 设置文件模式(权限)，文件创建默认权限为 0666(可读，可写)。

- callback - 回调函数，带有两个参数如：callback(err, fd)。

- flags 参数可以是以下值：

Flag	描述

r	以读取模式打开文件。如果文件不存在抛出异常。

r+	以读写模式打开文件。如果文件不存在抛出异常。

rs	以同步的方式读取文件。

rs+	以同步的方式读取和写入文件。

w	以写入模式打开文件，如果文件不存在则创建。

wx	类似 'w'，但是如果文件路径存在，则文件写入失败。

w+	以读写模式打开文件，如果文件不存在则创建。

wx+	类似 'w+'， 但是如果文件路径存在，则文件读写失败。

a	以追加模式打开文件，如果文件不存在则创建。

ax	类似 'a'， 但是如果文件路径存在，则文件追加失败。

a+	以读取追加模式打开文件，如果文件不存在则创建。

ax+	类似 'a+'， 但是如果文件路径存在，则文件读取追加失败。

代码示例：

```
var fs = require("fs");
 
// 异步打开文件
console.log("准备打开文件！");
fs.open('test.txt', 'r+', function (err, fd) {
    if (err) {
        return console.error(err);
    }
    console.log("文件打开成功！");
    console.log("写入文件")
    fs.writeFile('test.txt', "写入12334545", function (err) {
        if (err) {
            return console.error(err)
        }
        console.log("写入成功")
    });
    console.log("读取文件")
    fs.readFile('test.txt', function (err,data) {
        if (err) {
            return console.error(err)
        }
        console.log("读取数据：" + data.toString())
        console.log("读取成功")
    });
    
});
```

输出：

准备打开文件！
文件打开成功！
写入文件
读取文件
写入成功
读取数据：写入12334545
读取成功

2）读取文件

以下为异步模式下读取文件的语法格式：（该方法使用了文件描述符来读取文件）

```
fs.read(fd, buffer, offset, length, position, callback)
```

参数使用说明如下：

- fd - 通过 fs.open() 方法返回的文件描述符。

- buffer - 数据写入的缓冲区。

- offset - 缓冲区写入的写入偏移量。

- length - 要从文件中读取的字节数。

- position - 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。

- callback - 回调函数，有三个参数err, bytesRead, buffer，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。

代码示例：

```
var fs = require("fs");
var buf = Buffer.alloc(1024);
 
console.log("准备打开已存在的文件！");
fs.open('test.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件：");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }
      console.log(bytes + "  字节被读取");
      
      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }
   });
});
```

输出：

准备打开已存在的文件！
文件打开成功！
准备读取文件：
14  字节被读取
写入12334545
3）写入文件

```
fs.write(fd, buffer, offset, length[, position], callback) 
```

fs.write将buffer缓冲器内容写入fd文件描述符,

offset和length决定了将缓冲器中的哪部分写入文件。

position指明将数据写入文件从头部算起的偏移位置，若position为null，数据将从当前位置开始写入

callback 回调函数接受两个参数(err, written)，其中written标识有多少字节的数据已经写入

注意：写完后要关闭它

```
var fs=require('fs');
var buf= Buffer.from('I Like studying');
console.log(buf)
fs.open('test.txt','a',function(err,fd) {
    fs.write(fd,buf, 0,buf.length,0,function(err,written,buffer) {
        fs.write(fd,buf,0,buf.length,0,function(err,written,buffer) {
            if(err) console.log("写文件操作失败。");
            else console.log("写文件操作成功。");
             fs.close(fd,function(){
                 console.log("关闭成功")
             }); 
        }); 
    });
})
```

输出：

Buffer(15) [73, 32, 76, 105, 107, 101, 32, 115, …]
写文件操作成功。
关闭成功
同级的test.txt文件被写入

I Like studyingI Like studying

4）关闭文件

以下为异步模式下关闭文件的语法格式：（该方法使用了文件描述符来读取文件）

```
fs.close(fd, callback)
```

参数使用说明如下：

- fd - 通过 fs.open() 方法返回的文件描述符。

- callback - 回调函数，没有参数。


代码示例：

```
var fs = require("fs");
var buf = Buffer.alloc(1024);
 
console.log("准备打开已存在的文件！");
fs.open('test.txt', 'r+', function (err, fd) {
    if (err) {
        return console.error(err);
    }
    console.log("文件打开成功！");
    console.log("准备读取文件：");
    fs.read(fd, buf, 0, buf.length, 0, function (err, bytes) {
        if (err) {
            console.log(err);
        }
        console.log(bytes + "  字节被读取");
 
        // 仅输出读取的字节
        if (bytes > 0) {
            console.log(buf.slice(0, bytes).toString());
        }
        // 关闭文件
        fs.close(fd, function (err) {
            if (err) {
                console.log(err);
            }
            console.log("文件关闭成功");
        });
    });
});
```

输出：

准备打开已存在的文件！
文件打开成功！
准备读取文件：
14  字节被读取
写入12334545
文件关闭成功

三、目录的操作

1）创建目录

```
var fs = require('fs'); // 引入fs模块
 
// 创建 newdir 目录
fs.mkdir('目录名和路径', function(err) {
   if (err) {
       throw err;
   }
   console.log('make dir success.');
})
```

2）读取目录

```
var fs = require('fs'); // 引入fs模块
 
fs.readdir('目录名和路径', function(err, files) {
   if (err) {
       throw err;
   }
   // files是一个数组，每个元素是此目录下的文件或文件夹的名称
   console.log(files);
})
```


四、Fs与Stream之间的联系

通常简单的读取文件时可以用readFile，其工作方式是将文件读取到内存。但是当内存较大时这样做就会浪费很多的内存，因为用户需要等到整个文件缓存到内存才能接受的文件数据，这样导致用户体验相当不好。

```
fs.readFile("文件名称","utf-8", function(err, data){})
```

因为 http.createServer(function(req,res){})中的req，res两个参数都是流所以可将上述代码改成如下代码，即可解决上述问题

```
fs.createReadStream(__dirname + '大文件名称').pipe(res) ;
```

五、path模块

该模块用于处理和转换文件路径的实用程序。几乎所有这些方法只支持字符串转换。和所有的模块使用方法一样，再使用前需要先将其引用进来require('path')，再对其进行相应地属性操作。

1.path.dirname(p)

获取指定路径中目录名，如果当前为目录，则返回上一级目录


```
var path = require("path")
 
var myPath = path.dirname(__dirname + '/lib/lib.js');
console.log(myPath);//C:\Users\99781\Desktop\test/lib
2.path.basename(p,[ext])
```

获取路径中文件名,后缀是可选的，如果加，请使用'.ext'方式来匹配，则返回值中不包括后缀名；

```
var path=require('path');
 
var myPath = path.basename(__dirname + '/lib/lib.js', '.js');
console.log(myPath);//lib
```

3.获取绝对路径path.resolve(path1, [path2]..[pathn])

```
var path = require("path")
var myPath = path.resolve('path1', 'path2', 'a/b\\c/');
console.log(myPath);//C:\Users\99781\Desktop\test\path1\path2\a\b\c
```

4.获取相对路径path.relative(from, to)

from 当前路径，并且方法返回值是基于from指定到to的相对路径
to 到哪路径

```
console.log(path.relative("1.js","./lib"));//..\lib
```

5、path.join([...paths])

将给定的路径拼接起来，如果给定的路径长度为零，则返回”.“,表示当前的目录

```
var path = require('path');
// 输出 a\b\c
console.log(path.join('a', 'b', 'c', 'd',".."))
// 输出 a\b\c\d
console.log(path.join('a', 'b', 'c', 'd'))
```

6、path.parse(path)

path是一个字符串类型

path.parse() 方法返回一个对象，对象的属性表示 path 的元素。 尾部文件分隔符会被忽略，会返回以下属性：

dir <string>
root <string>
base <string>
name <string>
ext <string>

```
var path = require('path');
console.log(path.parse("https://www.csdn.net/"));
// 输出
//Object {root: "", dir: "https:/", base: "www.csdn.net", ext: ".net", name: "www.csdn"}
```

7、path.format(pathObject)

pathObject <Object>

dir <string>
root <string>
base <string>
name <string>
ext <string>

返回: <string>

path.format() 方法会从一个对象返回一个路径字符串。 与 path.parse() 相反。

```
var path = require('path');
console.log(path.parse("https://www.csdn.net/"));
// 输出{root: "", dir: "https:/", base: "www.csdn.net", ext: ".net", name: "www.csdn"}
console.log(path.format({root: "", dir: "https:/", base: "www.csdn.net", ext: ".net", name: "www.csdn"}))
//输出https:/\www.csdn.net
```

8、Windows 与 POSIX

path 模块的默认操作会根据 Node.js 应用程序运行的操作系统的不同而变化。 比如，当运行在 Windows 操作系统上时，path 模块会认为使用的是 Windows 风格的路径。

例如，对 Windows 文件路径 C:\temp\myfile.html 使用 path.basename() 函数，运行在 POSIX 上与运行在 Windows 上会产生不同的结果：

```
var path = require('path');
var obj = path.basename('C:\\temp\\myfile.html');
console.log(obj)//windows上输出：myfile.html；POSIX上输出：'C:\\temp\\myfile.html'

// 如果在POSIX上想输出windows上的形式可以在前面加上win32属性即可，如下：
var obj2 = path.win32.basename('C:\\temp\\myfile.html');
console.log(obj2)//会输出windows上形式的：myfile.html；
```

