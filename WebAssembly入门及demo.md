```
var xhr = new XMLHttpRequest();
xhr.open('GET', url, false);
xhr.responseType = 'arraybuffer';
xhr.send(null);
return new Uint8Array(xhr.response);
```


# Load
```
function loadWebAssembly (path) {
  return fetch(path)                   // 加载文件        
    .then(res => res.arrayBuffer())    // 转成 ArrayBuffer
    .then(WebAssembly.instantiate)     // 编译 + 实例化
    .then(mod => mod.instance)         // 提取生成都模块
}
loadWebAssembly('path/to/math.wasm')
  .then(instance => {
    const { add, square } = instance.exports
    // ...
})
```
OR
```
function loadWebAssembly(filename, imports = {}) {
return fetch(filename)
    .then(response => response.arrayBuffer())
    .then(buffer => WebAssembly.compile(buffer)) 
    //WebAssembly.compile 可以用来编译 wasm 的二进制源码，
    //它接受 BufferSource 格式的参数，返回一个 Promise。
    .then(module => {           
        imports.env = imports.env || {};
        // 开辟内存空间 && 创建变量映射表
        Object.assign(imports.env, {
            memoryBase: 0,
            tableBase: 0,
            memory: new WebAssembly.Memory({ initial: 256, maximum: 256 }),
            table: new WebAssembly.Table({ initial: 0, maximum: 0, 
                    element: 'anyfunc' })
        })
        // 创建 WebAssembly 实例
        return new WebAssembly.instantiate(module, imports)
    })
}
```


# Compile
```
WebAssembly.compile(new Uint8Array(`
  00 61 73 6d   01 00 00 00   01 0c 02 60   02 7f 7f 01
  7f 60 01 7f   01 7f 03 03   02 00 01 07   10 02 03 61
  64 64 00 00   06 73 71 75   61 72 65 00   01 0a 13 02
  08 00 20 00   20 01 6a 0f   0b 08 00 20   00 20 00 6c
  0f 0b`.trim().split(/[\s\r\n]+/g).map(str => parseInt(str, 16))
)).then(module => {
  const instance = new WebAssembly.Instance(module)
//使用 WebAssembly.Instance 将模块对象转成 WebAssembly 实例
  const { add, square } = instance.exports
//通过 instance.exports 可以拿到 wasm 代码输出的接口
  console.log('2 + 4 =', add(2, 4))
  console.log('3^2 =', square(3))
  console.log('(2 + 5)^2 =', square(add(2 + 5)))
})
```

# Get the emsdk repo
git clone https://github.com/emscripten-core/emsdk.git

# Enter that directory
cd emsdk


# Fetch the latest version of the emsdk (not needed the first time you clone)
git pull

# Download and install the latest SDK tools.
./emsdk install latest

# Make the "latest" SDK "active" for the current user. (writes ~/.emscripten file)
./emsdk activate latest

# Activate PATH and other environment variables in the current terminal
source ./emsdk_env.sh

# 编译
```
emcc hello.c -s WASM=1 -o hello.html
```
-s WASM=1 — 指定我们想要的wasm输出形式。如果我们不指定这个选项，Emscripten默认将只会生成asm.js。

-o hello.html — 指定这个选项将会生成HTML页面来运行我们的代码，并且会生成wasm模块以及编译和实例化wasim模块所需要的“胶水”js代码，这样我们就可以直接在web环境中使用了。



```javascript
var wasmCode = new Uint8Array([0,97,115,109,1,0,0,0,1,133,128,128,128,0,1,96,0,1,127,3,130,128,128,128,0,1,0,4,132,128,128,128,0,1,112,0,0,5,131,128,128,128,0,1,0,1,6,129,128,128,128,0,0,7,145,128,128,128,0,2,6,109,101,109,111,114,121,2,0,4,109,97,105,110,0,0,10,138,128,128,128,0,1,132,128,128,128,0,0,65,42,11]);
var wasmModule = new WebAssembly.Module(wasmCode);
var wasmInstance = new WebAssembly.Instance(wasmModule, {});
console.log(wasmInstance.exports.main());
```

就能把以上代码编译成可运行的 WebAssembly 模块。

为了加载并执行编译出的 f.wasm 模块，需要通过 JS 去加载并调用模块上的 f 函数，为此需要以下 JS 代码：
```javascript
fetch('f.wasm') // 网络加载 f.wasm 文件
    .then(res => res.arrayBuffer()) // 转成 ArrayBuffer
    .then(WebAssembly.instantiate) // 编译为当前 CPU 架构的机器码 + 实例化
    .then(mod => { // 调用模块实例上的 f 函数计算
    console.log(mod.instance.f(50));
    });
```
