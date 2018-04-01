NodeJs特性：
  			事件驱动、异步编程、无阻塞IO
		
NodeJS在客户端Javasceipt的基础上添加了File API，以实现文件操作


JS是脚本语言，脚本语言都是需要一个解析器才能运行

HtmL里的JS——浏览器充当解析器
独立运行的JS—- nodeJs就是一个解析器


模块：require \exports\module

require函数 用于当前模块加载和使用别动模块，传入模块名，返回一个导入模块导出对象
例如：var foo1=require(‘./foo’)
           var data = require(‘./data.json’)
exports对象是当前模块的导出对象，用于导出模块公有方法和属性，别动模块通过require函数使用当前模块时得到的就是当前模块的exports对象
         exports.hello = function(){
			console.log();
		}
module 访问当前模块的一些相关信息，最多用途时替换当前模块导出对象

module.exports = function(){console.log()}

NodeJs使用CMD模块系统，主模块作为程序入口点，所有模块在执行过程中只初始化一次


防爆仓   —  数据流

文件属性的


