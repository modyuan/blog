# browserify使用

简单好用的一个工具。把后端（npm）中的库可以让前端用require的方式使用。

例如：
`npm i --save uniq`

在index.js 中 require这个库 `var u = require('uniq') `

然后 `browserify index.js -o bundle.js`

在html中引用bundle.js 即可。