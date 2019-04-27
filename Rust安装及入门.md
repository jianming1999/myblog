Rust 工具链
Rust 工具链包括：
* 		rustup：负责安装 Rust、切换 Rust 版本、下载标准库文件等
* 		rustc：Rust 的编译器（一般通过 Cargo 命令调用）
* 		cargo：Rust 的项目管理工具（类似 Node 的 NPM）
运行：curl https://sh.rustup.rs -sSf | sh即可安装
安装 WASM 工具链
wasm-pack 用于将 Rust 项目打包成单个 WASM 文件（类似 Webpack），运行 curl https://rustwasm.github.io/wasm-pack/installer/init.sh -sSf | sh安装。
cargo-generate 用于快速生成 WASM 项目的脚手架（类似 create-react-app），运行 cargo install cargo-generate即可安装。
创建一个 WASM 项目
首先运行 cargo generate --git https://github.com/rustwasm/wasm-pack-template，按照提示创建项目。

https://imweb.io/topic/5c06b8b0611a25cc7bf1d7d5

```
$ rustc --version
$ rustup toolchain install stable
$ rustup toolchain install nightly
$ rustup default nightly
$ rustup default stable
$ rustup show
$ rustup self uninstall
$ which rustc

$ rustup toolchain uninstall nightly



rustup update
rustup target add wasm32-unknown-emscripten --toolchain stable
rustup target add wasm32-unknown-unknown --toolchain nightly

rustc +nightly --target wasm32-unknown-unknown -O hello.rs
cargo  +nightly build --target wasm32-unknown-unknown --release


cargo install --git https://github.com/alexcrichton/wasm-gc
wasm-gc hello.wasm small-hello.wasm
```

https://www.hellorust.com/setup/wasm-target/

