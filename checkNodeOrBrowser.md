判断NODE环境或浏览器环境
```javascript
// Only Node.js has the classes of which variable type is process.
    if (typeof process !== 'undefined' && Object.prototype.toString.call(process) === '[object process]') {
        // Node.js request module.
        adapter = require('./adapters/http');
    } else if (typeof XMLHttpRequest !== 'undefined') {
        // The browser request module.
        adapter = require('./adapters/xhr');
    }
```
