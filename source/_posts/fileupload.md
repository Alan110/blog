---
title: 文件上传
date: 2017-07-12 06:51:30
tags:
---

最近碰到了文件上传的需求，突然发现这个需求其实很常用，包括我的博客如何上传的图片等。我也希望自己写一个图床。

要做好文件上传，其实也并不容易


## form表单

```html
<form action="http://baidu.com" target="" id="uploadForm" enctype="multipart/form-data">
　　<input id="file" type="file" name="file"/>
　　<input type="submit" name="submit" id="submit" value="upload" />
</form>
```

**enctype 指明了表单数据在上传之前进行的编码方式**

| name            | describe                                          |
| : ------------- | :-------------                                    |
| application/x-www-form-urlencoded                                   | 在发送前urlencode编码所有字符（默认）, key,vlaue对             |
| multipart/form-data                                                 | 不对字符编码。 在使用包含文件上传控件的表单时，必须使用该值。         |
| text/plain                                                          | 空格转换为 "+" 加号，但不对特殊字符编码。                                |

* application/x-www-form-urlencoded             
    使用get方式提交时，把表单数据(name1=value1&name2=value2...)以键值对append到url后，用  '?' 分割url和参数。使用post方式提交时，把表单数据以键值对放在请求体中传输。              

* multipart/form-data            
    表单数据被编码为一条消息，页上的每个input对应消息中的一个部分
    用boundary=---------------------------36243265420146"分割各个部分（boundary值由浏览器生成）。它不会对字符进行编码，一般用于传输二进制文件（图片、视频、、、）  


> form表单的缺点

1. 会刷新页面，跳转到action的地址
2. 不够灵活，比如取消啊，,预览啊，或者别的验证，或者多文件上传之类的都不好做。


## HTML5之FormData、FileReader

html5有个新的js对象，FormData可以用来模拟表单数据, 然后通过[ajax上传文件](https://developer.mozilla.org/en-US/docs/Web/API/FormData#append).
* [FormData API](https://developer.mozilla.org/en-US/docs/Web/API/FormData#append)
* FormData 对象可以通过form，dom元素构造，也可以通过new FormData(), 然后append键值对
* FormData 对象的字段类型可以是 Blob, File, 或者 string: 如果它的字段类型不是Blob也不是File，则会被转换成字符串类型。
* FormData 的字段属性不能**直接删除**，需要调用FormData.delete,不过目前兼容性不好

下面是一个通过表单，利用ajax上传的例子。

```html
<form enctype="multipart/form-data" method="post" name="fileinfo">
  <label>Your email address:</label>
  <input type="email" autocomplete="on" autofocus name="userid" placeholder="email" required size="32" maxlength="64" /><br />
  <label>Custom file label:</label>
  <input type="text" name="filelabel" size="12" maxlength="32" /><br />
  <label>File to stash:</label>
  <input type="file" name="file" required />
  <input type="submit" value="Stash the file!" />
</form>
<div></div>
```

```javascript
var form = document.forms.namedItem("fileinfo");
form.addEventListener('submit', function(ev) {

  var oOutput = document.querySelector("div"),
      oData = new FormData(form);

  oData.append("CustomField", "This is some extra data");

  var oReq = new XMLHttpRequest();
  oReq.open("POST", "stash.php", true);
  oReq.onload = function(oEvent) {
    if (oReq.status == 200) {
      oOutput.innerHTML = "Uploaded!";
    } else {
      oOutput.innerHTML = "Error " + oReq.status + " occurred when trying to upload your file.<br \/>";
    }
  };

  oReq.send(oData);
  ev.preventDefault();
}, false);
```


也可以不经过表单，通过input 的onchange事件拿到要上传的文件

```html
<form>
    <input type="file" id="webupload" style="width:170px;"  accept="image/png">
</form>
```

```javascript
var input = doucument.querySelector('#webupload');
input.onchange = function(e){
    let file e.target.files[0];

    // 准备formData
    var formData = new FormData();
    formData.append('file', file);

    // 开始上传
    $.ajax({
        url: this.server,
        type: 'POST',
        data: formData,
        processData: false, // 不处理发送的数据，因为data值是Formdata对象，不需要对数据做处理
        cache: false,
        dataType: 'json',
        contentType: false, 
        error: function (e) {
        },
        success: (res) => {
        }
    })

}

```

> 注意如果用jquery，**processData要为false**，不做编码处理

> 当二次上传的时候，input还保留了以前的value，如果选择到了同一个文件，**则不会触发onchange事件**，所以，在上传完毕后要清空input的value

> 我测试直接设置input.value = '' , 是不成功的。
> 另一个方案是，**使用form元素的reset(),方法**，会清空form里面所有input的值


**FormData的兼容性**

现代浏览器Android4.4以上都支持，IE10及其以上支持
![](http://o99eh3ii0.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170712084359.png)

## FormData 上传base64图片

FormData支持blob对象，所以把base64转成blob对象即可, 以下是一个转换算法示例

```JavaScript
var base64ToBlob =  function (base64, mime) {
        mime = mime || '';
        var sliceSize = 1024;
        var byteChars = window.atob(base64);
        var byteArrays = [];

        for (var offset = 0, len = byteChars.length; offset < len; offset += sliceSize) {
            var slice = byteChars.slice(offset, offset + sliceSize);

            var byteNumbers = new Array(slice.length);
            for (var i = 0; i < slice.length; i++) {
                byteNumbers[i] = slice.charCodeAt(i);
            }

            var byteArray = new Uint8Array(byteNumbers);

            byteArrays.push(byteArray);
        }

        return new Blob(byteArrays, { type: mime });
    },


    var base64ImageContent = base64String.replace(/^data:image\/(png|jpg|jpeg);base64,/, "");
    var blob = utils.base64ToBlob(base64ImageContent, 'image/png');
    var fileOfBlob = new File([blob], 'img'+ Math.random() +'.jpg'); // 指定文件名
    var formData = new FormData();
    formData.append('file', fileOfBlob);
```

> 这里转成blob对象后并没有直接放入FormData,而是又转成了file对象，目的是为了指定文件本身的文件名，当然如果对文件名没有要求的，可以忽略这一步。大部分后端会依赖文件名以及后缀，做一些处理。

## 分片上传超大文件

上传文件过大会有很多问题，往往server只支持2MB大小的数据。此时可以利用分片上传超大文件

原理是利用**file对象的slice**方法，切割文件，发送多个请求，然后后端拼接文件。

* 需要知道总共的分片数，
* 当前的分片数据
* 每片的字节大小
* 当前的分片index

```javascript

    /**
        * 分片上传
        * 递归上传切割出来的分片
        * 注意上传的额外formData，告诉server如何合并文件
        */
    chunkUploadFile(file) {

        let chunkUpload = () => {

            curChunkIndex += 1;
            let start = curChunkIndex * this.chunkSize;  // 切割数据
            let end = Math.min(file.size, start + this.chunkSize);

            // 准备formData
            var formData = new FormData();
            let chunkFile = file.slice(start, end);
            chunkFile = new File([chunkFile], file.name);
            chunkFile.filename = file.name;
            formData.append('file', chunkFile);
            formData.append('size', chunkFile.size);
            formData.append('lastModifiedDate', file.lastModifiedDate);
            formData.append('name', file.name);
            formData.append('chunks', chunkCount);
            formData.append('type', file.type);
            formData.append('chunk', curChunkIndex);
            formData.append('id', 'WU_FILE_0');  // 这个名字是固定，估计是server用来合并文件
            for (var prop in this.formData) {
                formData.append(prop, this.formData[prop]);
            }

            // 开始上传
            $.ajax({
                url: this.server,
                type: 'POST',
                data: formData,
                processData: false, // 不处理发送的数据，因为data值是Formdata对象，不需要对数据做处理
                cache: false,
                dataType: 'json',
                contentType: false,
                error: function (e) {
                    self.$emit('error', e);
                },
                success: (res) => {
                    if (curChunkIndex + 1 < chunkCount) {
                        chunkUpload();
                    } else {
                        self.$emit('success', res);
                    }
                }
            })
        }

        // 上传之前
        this.$emit('beforeUpload', file);
        let self = this;
        let chunkCount = Math.ceil(file.size / this.chunkSize);
        let curChunkIndex = -1;

        chunkUpload();

    },

```

## flash

flash 虽然可以实现以上的所有能力，但它终将死亡，这里就不再叙述了。能用到flash的一般都是为了兼容老的浏览器。
希望web生态越来好，紧跟标准。


## 总结

其实文件上传，FormData是属于xhr的level2的新特性之一，可以关注一下底层规范。
原生js的能力已经越来越强大了，很多新能力已经能在实际生产中使用了，我们要学道，而不是术，底层原理是基础,关注规范。