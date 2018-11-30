一般情况下，前端的文件上传一般都是通过form表单的（<input type="file" />）来完成文件的上传，如果使用node中间层完成跨域，文件的上传就需要在node中间层处理成可读流，转成formData完成转发。

### 一、form表单文件上传

　　这是最常见的文件上传方式，通过form表单实现，简单实用，设置一下method、enctype、action属性就可以了，多文件上传需要设置multiple属性（部分浏览器支持还是有些问题的）。

```html
<form method="post" enctype="multipart/form-data" action="/api/upload">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="file" name="file">
    <input type="submit">
</form>
```

### 二、FormData实现文件上传

　　FormData对象用以将数据编译成键值对,以便向后台发送数据。其主要用于发送表单数据,但亦可用于发送带键数据(keyed data),而独立于表单使用。对部分浏览器对multiple属性不支持的情况，可以使用formData的提交方式完成。

```html
<!-- 获取上传文件转成formData类型的文件 -->
<input multiple id="file" name="file" type="file" />
<button id="btn">提交</button>
```

```javascript
const oFile = document.getElementById('file')
const oBtn = document.getElementById('btn')

oBtn.addEventListener('click', () => {
    files = oFile.files
    const formData = new FormData()
    formData.append('file', files[0])
    formData.append('file1', files[1])

    fetch('/api/upload', {
        method: "POST",
        body: formData
    })
})
```

- 使用fetch请求不要设置Content-Type,否则无法请求
- fetch请求默认是不带cookie


### 三、node中间层完成文件上传跨域

　　跨域是因为浏览器的同源策略造成，跨域的方法有很多中，这里使用的是node中间层代理完成（服务端之间的请求是不存在跨域问题）。

　　node无法直接解析上传的文件，需要引入拓展包connect-multiparty完成，这样就可以拿到文件数据。

　　拿到上传文件，需要在node中转发请求后台server，这里的文件不能直接发给后台,需要将上传的文件使用fs.createReadStream转成可读流，同时引入 form-data 包（node环境是没有formData对象的），这样就可以实现node中间层转发文件类型

　　node部分代码:

```javascript
const fs = require('fs')
const path = require('path')
const FormData = require('form-data')
const express = require('express')
const fetch = require('node-fetch')
const router = express.Router()
const multipart = require('connect-multiparty');
var multipartMiddleware = multipart()

router.post('/upload', multipartMiddleware, function (req, res) {
    // console.log(req.body, req.files);

    const { path: filePath, originalFilename } = req.files.file
    const newPath = path.join(path.dirname(filePath), originalFilename)

    fs.rename(filePath, newPath, function (err) {
        if (err) {
            return;
        }
        else {
            const file = fs.createReadStream(newPath)
            const form = new FormData()
            form.append('file', file)

            fetch('http://localhost:8080/upload', {
                method: "POST",
                body: form
            })
        }
    })
    res.json({})
});
```

注意：

- node无法直接解析上传文件，需要引入npm包connect-multiparty中间件，或者引入npm包multiparty
- node拿到文件，需要使用fs.createReadStream转成可读流
- node环境没有formData对象，需要引入npm包form-data
- fetch请求提交formData数据，不能设置Comtemt-Type

