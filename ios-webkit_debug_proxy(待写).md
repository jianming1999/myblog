ios webkit调试工具

# examples:
ios_webkit_debug_proxy -f chrome-devtools://devtools/bundled/inspector.html
ios_webkit_debug_proxy -f ~/chromium/src/third_party/WebKit/Source/devtools/front_end/inspector.html
ios_webkit_debug_proxy -f http://foo.com:1234/bar/inspector.html

# install
brew install ios-webkit-debug-proxy

# update
brew update
brew reinstall --HEAD libimobiledevice
brew reinstall -s ios-webkit-debug-proxy

# github
https://github.com/google/ios-webkit-debug-proxy
