---
title: webuploader入门指南
date: 2017-03-11 09:29:15
tags:
---


## 简单示例


```css
    .webuploader-container {
        position: relative;
    }
    .webuploader-element-invisible {
        position: absolute !important;
        clip: rect(1px 1px 1px 1px); /* IE6, IE7 */
        clip: rect(1px,1px,1px,1px);
    }
    .webuploader-pick {
        position: relative;
        display: inline-block;
        cursor: pointer;
        background: #00b7ee;
        padding: 10px 15px;
        color: #fff;
        text-align: center;
        border-radius: 3px;
        overflow: hidden;
    }
    .webuploader-pick-hover {
        background: #00a2d4;
    }

    .webuploader-pick-disable {
        opacity: 0.6;
        pointer-events:none;
    }

```

这些css是预定义钩子，用来重置input的样式和一些状态下的样式，可以自己定义

```javascript
var uploader = WebUploader.create({
    auto: true, // 选择完毕后自动上传
    swf: 'path_of_swf/Uploader.swf',
    pick :{
        id: '#filePicker',
        label: '点击选择图片'
    },
    chunked : true, 
    threads : 1, // 设置并发请求数
    chunkSize : 1.7 * 1024 * 1024, // 分片大小
    server: '../../server/fileupload.php',
    accept : {
        title: 'Images',
        extensions: 'gif,jpg,jpeg,bmp,png',
        mimeTypes: 'image/*',
        compress : false
    }

});

```

[详细Api](http://fex.baidu.com/webuploader/doc/index.html#WebUploader_Uploader_md5File)


## 携带自定义参数

```javascript
// 初始化的时候直接添加  
var uploader = new WebUploader.Uploader({  
    ...  
    formData: {  
        uid: 123  
    }  
    ...  
});  
  
// 初始化以后添加  
uploader.options.formData.uid = 123;  
```


> 局部设置

```javascript
uploader.on( 'uploadBeforeSend', function( block, data ) {  
    // block为分块数据。  
  
    // file为分块对应的file对象。  
    var file = block.file;  
  
  
    // 修改data可以控制发送哪些携带数据。  
    data.uid = 123;  
    data.formData = {};
}); 
```

## 关键事件点

```javascript
uploader.on('fileQueued',function(file) {
    
  uploader.md5File( file )

        // 及时显示进度
        .progress(function(percentage) {
            //console.log('Percentage:', percentage);
        })

        // 完成
        .then(function(val) {
            console.log('md5 result:', val);
            uploader.option.formData.md5 = val;
            uploader.upload();
        });
})

uploader.on('uploadBeforeSend',function(block , data) {
})

uploader.on('uploadProgress',function(file , percentage) {
    
})

uploader.on('uploadSuccess',function(file ,response) {
    
})

uploader.on('uploadError',function(file ,reason) {
    console.log(reason);
})


```
