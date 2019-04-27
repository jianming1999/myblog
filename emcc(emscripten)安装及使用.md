emcc

Install:

```
$ git clone https://github.com/emscripten-core/emsdk.git
$ cd emsdk
$ ./emsdk install --build=Release sdk-incoming-64bit binaryen-master-64bit
$ ./emsdk activate --build=Release sdk-incoming-64bit binaryen-master-64bit
```

Or 
```
$ ./emsdk install latest
$ ./emsdk activate latest
```

```
source ./emsdk_env.sh --build=Release
```


Build:
```
emcc index.c -s WASM=1 -O3 -o index.js
emcc hello.c -s WASM=1 -o hello.html
```

Run Server:
```
emrun --no_browser --port 8080 .
```
