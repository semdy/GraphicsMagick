GM : GraphicsMagick for node.js

首先得安装 GraphicsMagick 或者 ImageMagick。

然后执行：

$ sudo npm install gm

我安装的是ImageMagick，在ubuntu系统下快速安装：

$ sudo apt-get install imagemagick

HOW TO USE

GM 文档：http://aheckmann.github.io/gm/docs.html

使用ImageMagick

var imageMagick = gm.subClass({ imageMagick: true });

然后就像文档中使用gm那样使用ImageMagick即可（举个例子）

imageMagick(“img.png”).resize(300, 100).autoOrient().write(’/path’, callback);


####Example (nodejs + Express)

```bash
var gm = require('gm')
,  fs = require('fs')
,	imageMagick = gm.subClass({ imageMagick : true });
exports.imgUpload = function(req, res) {
	res.header('Content-Type', 'text/plain');
  var path = req.files.img.path;	//获取用户上传过来的文件的当前路径
  var sz = req.files.img.size;
  if (sz > 2*1024*1024) {
    fs.unlink(path, function() {	//fs.unlink 删除用户上传的文件
      res.end('1');
    });
  } else if (req.files.img.type.split('/')[0] != 'image') {
    fs.unlink(path, function() {
      res.end('2');
    });
  } else {
    imageMagick(path)
    .resize(150, 150, '!') //加('!')强行把图片缩放成对应尺寸150*150！
    .autoOrient()
    .write('public/images/user/'+req.files.img.name, function(err){
      if (err) {
        console.log(err);
        res.end();
      }
      fs.unlink(path, function() {
        return res.end('3');
      });
    });
  }
};
```