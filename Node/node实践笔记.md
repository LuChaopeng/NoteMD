1. 在使用ws库的事件监听函数时，ws.on('message', function (msg) {｝)的参数msg让我很迷，直接log是<buffer>，却可以以模板字面量输出。查了文档得知这个msg是个buffer对象

   `Event: 'message' `

   `data {Buffer|ArrayBuffer|Buffer[]} isBinary {Boolean}`

   并得知buffer是node.js的内容，可以用toString()转换为字符串编码