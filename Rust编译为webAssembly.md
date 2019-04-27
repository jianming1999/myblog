安装Rustup

Rustup是一个命令行应用，能够下载并在不同版本的Rust工具链中进行切换

```
brew install cargo
curl https://sh.rustup.rs -sSf | sh
source $HOME/.cargo/env 
source  ~/.bash_profile
rustup target add wasm32-unknown-unknown --toolchain nightly 
cargo install --git https://github.com/alexcrichton/wasm-gc 
//减小wasm的size
```

cargo可以将整个工程编译为wasm，首先使用cargo创建工程：

cargo new project

下一步，把下面的代码加到 Cargo.toml 中
```
[lib]
path = "src/lib.rs"
crate-type = ["cdylib"]
```

demo：https://github.com/jakedeichert/wasm-astar

编译:

cargo +nightly build --target wasm32-unknown-unknown --release



编译出来的wasm大小为82Kb,使用wasm-gc压缩 small-wasm_astar.wasm 的大小为 67Kb

wasm-gc wasm_astar.wasm small-wasm_astar.wasm
