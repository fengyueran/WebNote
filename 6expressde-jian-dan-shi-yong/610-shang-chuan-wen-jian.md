首先，在网页插入上传文件的表单。
```
<form action="/upload" method="POST" enctype="multipart/form-data">
  Select an zip to upload:
  <input type="file" name="image">
  <input type="submit" value="Upload zip">
</form>
```
然后，服务器脚本建立指向/upload目录的路由。这时可以安装formidable模块，它提供了上传文件的许多功能。
```
app.post('/upload', function (req, res) {
    var form = new formidable.IncomingForm();
    form.uploadDir = "uploads/";
    form.keepExtensions = true;
    form.parse(req, function(err, fields, files) {
        res.writeHead(200, {'content-type': 'text/plain'});
        res.write('received upload:\n\n');
        res.end();
        console.log("parse done");
        //console.log(files);
    });
});
```
formidable解析数据后，会将这两种数据分别放到files和fields两个回调参数中。并把上传文件写入uploadDir下。