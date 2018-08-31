# Froala WYSIWYG Editor Node.JS SDK (with Qiniu Cloud)

[![npm](https://img.shields.io/npm/v/wysiwyg-editor-node-sdk.svg)](https://www.npmjs.com/package/wysiwyg-editor-node-sdk)
[![npm](https://img.shields.io/npm/dm/wysiwyg-editor-node-sdk.svg)](https://www.npmjs.com/package/wysiwyg-editor-node-sdk)
[![npm](https://img.shields.io/npm/l/wysiwyg-editor-node-sdk.svg)](https://www.npmjs.com/package/wysiwyg-editor-node-sdk)

Easing the [Froala WYSIWYG HTML Editor](https://github.com/froala/wysiwyg-editor) server side integration in Node.JS projects.

## Installation

1. Clone this repo or download the zip.

2. Run `npm install`.

3. (Optional) Run `bower install` to install the editor JS.

3. Load `lib` directory in your project and import it: `var FroalaEditor = require('path/to/lib/froalaEditor.js');`

4. To run examples:
* `npm start` to start a nodejs server form `examples` directory at `http://localhost:3000/`

## Import lib
```javascript
var FroalaEditor = require('path/to/lib/froalaEditor.js');
```

## Documentation

基础用法：
 * [Official documentation](https://www.froala.com/wysiwyg-editor/docs/sdks/nodejs)
 
使用七牛云：
```javascript
app.post('/upload_image_validation', function (req, res) {
  var options = {
    accessKey: 'Your AccessKey', // AK
    secretKey: 'Your Secretkey', // SK
    bucket: 'Bucket Name', // 存储空间名称
    zone: 'Zone_z0', // 机房位置 华东Zone_z0 华北Zone_z1 华南Zone_z2 北美Zone_na0
    domain: 'Your domain', // 测试域名或cdn加速域名
    fieldname: 'myImage',
    validation: function(filePath, mimetype, callback) {
    
      gm(filePath).size(function(err, value){

        if (err) {
          return callback(err);
        }

        if (!value) {
          return callback('Error occurred.');
        }

        if (value.width != value.height) {
          return callback(null, false);
        }
        return callback(null, true);
      });
    }
  }

  FroalaEditor.Image.uploadQn(req, '/uploads/', options, function(err, data) {

    if (err) {
      return res.send(JSON.stringify(err));
    }
    res.send(data);
  });
});
```
- 使用七牛云后，文件将不再保存至本地（设置文件路径是为了暂存文件，用于实现用户自定义验证功能，验证结束后文件会删除）
- 关于七牛云的更多信息，请访问[官方文档](http://developer.qiniu.com/kodo)

## Help
- Found a bug or have some suggestions? Just submit an issue.
- Having trouble with your integration? [Contact Froala Support team](http://froala.dev/wysiwyg-editor/contact).


## License

The Froala WYSIWYG Editor Node.JS SDK is licensed under MIT license. However, in order to use Froala WYSIWYG HTML Editor plugin you should purchase a license for it.

Froala Editor has [3 different licenses](http://froala.com/wysiwyg-editor/pricing) for commercial use.
For details please see [License Agreement](http://froala.com/wysiwyg-editor/terms).


