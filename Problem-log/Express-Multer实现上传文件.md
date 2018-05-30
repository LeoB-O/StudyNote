# Multer上传文件

## 概述

Multer是一个Express框架下用来上传文件的中间件。使用可以说是非常简单，但是因为某些奇怪的原因我折腾了好半天。

## 使用

代码其实很简单，就下面几行：

```javascript
var multer = require('multer')
var upload = multer({dest:'upload/'});

var app = express();

app.post('/upload-single',upload.single('myfile'),function(req,res,next){
    var file=req.file;
    //file里有各种文件的信息
})

app.listen(3000);
```

## 问题

我在测试的时候使用的是postman，因为使用html建立表单有跨域问题，然后我现在又不知道怎么解决跨域问题:)所以很僵硬，只能使用postman。

使用postman的时候，它总是提示我req.file undefined。原因居然是。。multer需要一个field-name，这个field-name需要和upload.single('xxx')里面的xxx保持一致。。。

也就是说，建立表单的时候，需要对表单元素指定name属性，对应的表单元素的name属性应该与对应的upload.single('xxx')里面的xxx同名。

比如说，我上面定义了upload.single('myfile')，那么html就应该这样写：

```html
<form method='post' action='/'>
    <input type='file' name='myfile'/>
</form>
```

但是一开始，我postman里面用的是binary上传的文件，并没有设置field-name的选项，查了一下才知道怎么解决。

## 解决

一开始各种百度，都没有用，上谷歌，分分钟解决。

谷歌给了我一个github上的issue，看了这个issue我才知道上面写的问题，之前连问题是什么都不知道。

所以，废话说的有点多，解决方法就是，postman里面用form-data就好了，不要用binary。。。这个问题还挺无语的。。其实不是我代码的问题，但是却折腾了我好久。。心塞塞

## 体会

遇到问题。。。尽量上谷歌！！！谷歌！！谷歌！！。垃圾百度是没有用的！！！。