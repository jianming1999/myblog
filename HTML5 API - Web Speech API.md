语音合成（文字转语音）
var utterThis = new window.SpeechSynthesisUtterance('你好，世界！');
window.speechSynthesis.speak(utterThis);
又或者是使用实例对象的一些属性，包括：
* text – 要合成的文字内容，字符串。
* lang – 使用的语言，字符串， 例如："zh-cn" 
* voiceURI – 指定希望使用的声音和服务，字符串。
* volume – 声音的音量，区间范围是0到1，默认是1。
* rate – 语速，数值，默认值是1，范围是0.1到10，表示语速的倍数，例如2表示正常语速的两倍。
* pitch – 表示说话的音高，数值，范围从0（最小）到2（最大）。默认值为1。
因此上面的代码也可以写作：
var utterThis = new window.SpeechSynthesisUtterance();
utterThis.text = '你好，世界！';
不仅如此，该实例对象还暴露了一些方法：
* onstart – 语音合成开始时候的回调。
* onpause – 语音合成暂停时候的回调。
* onresume – 语音合成重新开始时候的回调。
* onend – 语音合成结束时候的回调。
接下来是speechSynthesis对象，主要作用是触发行为，例如读，停，还原等：
* speak() – 只能接收SpeechSynthesisUtterance作为唯一的参数，作用是读合成的话语。 
* stop() – 立即终止合成过程。 
* pause() – 暂停合成过程。 
* resume() – 重新开始合成过程。 
* getVoices – 此方法不接受任何参数，用来返回浏览器支持的语音包列表 
