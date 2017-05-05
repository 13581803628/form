# jQuery Form [![Build Status](https://travis-ci.org/jquery-form/form.svg?branch=master)](https://travis-ci.org/jquery-form/form)

## 概述
`jQuery Form` 插件允许你轻松、自然地使用`AJAX`升级`HTML form`表单。`ajaxForm` 和 `ajaxSubmit`是这个插件的主要方法函数，它们从`form`表单元素中整合表单信息数据然后决定怎么管理提交进程。这两种方法都支持许多选项，可以完全控制数据的提交方式。

不需要特殊的标签，只是一个正常的form表单。使用AJAX提交表单不会比这更容易！

## 社区
想要贡献jQuery表单？真棒！有关详细信息，请参阅[贡献文档](CONTRIBUTING.md)。

### 行为守则
请注意，这个项目是伴随[贡献者行为准则](CODE_OF_CONDUCT.md)发布的，以确保这个项目是**每个人**都欢迎的欢迎之地。你必须同意遵守其条款才可参与此项目。

## 要求
需要jQuery 1.7或更高版本。兼容jQuery 2和3。

## 下载
* **开发版：** [src/jquery.form.js](https://github.com/jquery-form/form/blob/master/src/jquery.form.js)
* **生产版/压缩版：** [dist/jquery.form.min.js](https://github.com/jquery-form/form/blob/master/dist/jquery.form.min.js)（压缩不等于阉割-_-!压缩是把缩进代码扁平化）

### CDN 在线链接
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery.form/4.2.1/jquery.form.min.js" integrity="sha384-tIwI8+qJdZBtYYCKwRkjxBGQVZS3gGozr3CtI+5JF/oL1JmPEHzCEnIKbDbLTCer" crossorigin="anonymous"></script>
```
或者
```html
<script src="https://cdn.jsdelivr.net/jquery.form/4.2.1/jquery.form.min.js" integrity="sha384-tIwI8+qJdZBtYYCKwRkjxBGQVZS3gGozr3CtI+5JF/oL1JmPEHzCEnIKbDbLTCer" crossorigin="anonymous"></script>
```

---

## API 功能

### jqXHR
`jqXHR` 对象是调用`ajaxSubmit`方法后以`jqxhr`为键存储在元素 _data_-缓存中的。它可以像这样被访问：

```javascript
var form = $('#myForm').ajaxSubmit({ /* 选项 */ });
var xhr = form.data('jqxhr');

xhr.done(function() {
...
});
```

### ajaxForm( options )
通过添加必要的监听事件准备一个使用`AJAX`提交的表单。它不会**直接**提交表单。在你文档的`ready`函数中使用`ajaxForm`方法将一个已存在的form表单转为`AJAX`提交的表单，或者使用`delegation`选项处理尚未添加到DOM的表单。 
当你想要插件管理所有事件绑定时，请使用`ajaxForm`。

```javascript
// 转化所有的表单为AJAX提交
$('form').ajaxForm({
	target: '#myResultsDiv'
});
```

### ajaxSubmit( options )
通过`AJAX`方法立即提交表单。在最常见的用例中，这是为了响应用户单击表单上的提交按钮而被调用的。如果要将自己的提交处理程序绑定到表单，请使用ajaxSubmit。

```javascript
// 绑定提交处理程序到表单中
$('form').on('submit', function(e) {
	e.preventDefault(); // 防止原生的提交行为
	$(this).ajaxSubmit({
		target: 'myResultsDiv'
	})
});
```

---

## 选项
**注意：** 所有的标准 [$.ajax](http://api.jquery.com/jQuery.ajax) 选项都可以被使用。

### beforeSerialize
在表单序列化之前调用的回调函数。在表单的值被序列检索之前提供了操作表单信息的机会。在这个函数中返回`false`可以阻止表单被提交。使用两个参数调用回调函数：`jQuery`包装的表单对象和`options`对象。

```javascript
beforeSerialize: function($form, options) {
    // return false to cancel submit
}
```

### beforeSubmit
表单提交之前调用的回调函数。在这个函数中返回`false`可以阻止表单被提交。使用三个参数调用回调函数：数组格式的表单数据，`jQuery`包装的表单对象和`options`对象。

```javascript
beforeSubmit: function(arr, $form, options) {
    // form data array is an array of objects with name and value properties
    // [ { name: 'username', value: 'jresig' }, { name: 'password', value: 'secret' } ]
    // return false to cancel submit
}
```

### filtering
处理字段前调用的回调函数。这提供了一种过滤元素的方法。

```javascript
filtering: function(el, index) {
	if ( !$(el).hasClass('ignore') ) {
		return el;
	}
}
```

### clearForm
布尔值标记，指明成功提交后这个表单是否清空。

### data
对象，包含提交表单时附带的额外数据。

```
data: { key1: 'value1', key2: 'value2' }
```

### dataType
响应的预期数据类型。以下几个值其中之一：**`null`**、**`'xml'`**、**`'script'`**或**`'json'`**。`dataType`选项提供了一种指定服务器响应应如何处理的方法。这直接映射到jQuery的dataType方法。支持以下值：

* 'xml'：服务器响应被视为XML，`success`回调方法（如果指定）将被传递这个`responseXML`值
* 'json'：服务器响应将被求值并传递给`success`回调函数（如果指定）
* 'script'：在全局环境中评估服务器响应

### delegation
设置为`true`来启用对`delegation`事件的支持 （要求JQuery 1.7版本以上）

```javascript
// 准备所有现有和未来的表单，以ajax形式提交
$('form').ajaxForm({
    delegation: true
});
```

### error
在错误时被调用的回调函数。

### forceSync
仅在明确使用`iframe`选项或在不支持`XHR2`的浏览器上上传文件时适用。设置为`true`，当上传文件时，在上传表单之前消除短暂的延迟。这个延迟用于允许浏览器在执行本机表单提交之前渲染`DOM`元素更新。这样可以在向用户显示通知时提高可用性，例如“Please Wait ...”。

### iframe
布尔标记值，指明表单是否应将服务器响应瞄向一个`iframe`而不是尽可能地利用XHR。

### iframeSrc
字符串值，当使用一个`iframe`时用于`iframe`的`src`属性。

### iframeTarget
标识要用作文件上传的响应目标的iframe元素。默认情况下，插件将创建一个临时iframe元素来捕获上传文件时的响应。此选项允许你使用现有的`iframe`。当使用此选项时，插件将不会尝试处理来自服务器的响应。

### method
用于请求的`HTTP`方法（如`'POST'`、`'GET'`、`'PUT'`）。

### replaceTarget
可选择与`target`选项一起使用。如果整个`target`应该被替换则设置为`true`，如果只有目标内容应该被替换则设置为`false`。

### resetForm
布尔标记值，指明如果提交成功是否重置表单。

### semantic
布尔标记值，指明数据是否必须以严格的语义顺序提交（较慢）。请注意，正常表单序列化是以语义顺序完成的，但`type ="image"`的`input`输入元素除外。如果你的服务器有严格的语义要求，并且你的表单包含`type ="image"`的`input`输入元素，则应仅将`semantic`选项设置为`true`。

### success
当表单已经提交完成后执行的回调函数。如果设置了一个'success'回调函数，则当服务器返回了响应信息后将调用这个函数。它传递以下标准jQuery参数： 

1. `data`，根据`dataType`参数或`dataFilter`回调函数格式化，如果指定
2. `textStatus`，字符串
3. `jqXHR`，对象
4. `$form` 包含表单元素的jQuery对象。

### target
标识页面中随服务器响应更新的元素。该值可以被指定为`jQuery`选择字符串，`jQuery`对象或`DOM`元素。

### type
用于请求的HTTP方法（例如“POST”，“GET”，“PUT”）。 `method`选项的别名。如果两者都存在，则被`method`值覆盖。

### uploadProgress
通过上传进度信息调用回调函数（如果浏览器支持）。回调传递以下参数：

1. `event`：浏览器事件
2. `position`：（整数）
3. `total`：（整数）
4. `percentComplete`：（整数）

### url
表单上传的URL地址。

---

## 实用方法
### formSerialize
将表单序列化成查询字符串。这个方法会返回这样格式的一个字符串：`name1=value1&name2=value2`

```javascript
var queryString = $('#myFormId').formSerialize();
```

### fieldSerialize
将字段元素序列化为查询字符串。当您需要仅序列化表单的一部分时，这很方便。此方法将返回一个这样格式的字符串： `name1=value1&name2=value2`

```javascript
var queryString = $('#myFormId .specialFields').fieldSerialize();
```

### fieldValue
返回数组中匹配集中的元素的值。此方法总是返回一个数组。如果无法确定有效值，则数组将为空，否则将包含一个或多个值。
Returns the value(s) of the element(s) in the matched set in an array. This method always returns an array. If no valid value can be determined the array will be empty, otherwise it will contain one or more values.

### resetForm
通过调用form元素的本机DOM方法将表单重置为原始状态。

### clearForm
清除表单元素。此方法清除所有文本输入，密码输入和文本区域元素，清除任何选择元素中的选择，并取消选中所有单选框和复选框输入。它不清除隐藏的字段值。

### clearFields
清除所选的字段元素。当你需要清除表单的一部分时，这很方便。

---

## File Uploads
这个表单插件支持在支持这些功能的浏览器上使用 [XMLHttpRequest Level 2]("http://www.w3.org/TR/XMLHttpRequest/") 和 [FormData](https://developer.mozilla.org/en/XMLHttpRequest/FormData) 对象。截至今天（2012年3月），包括Chrome，Safari和Firefox。在这些浏览器（以及将来的Opera和IE10）文件上传将通过XHR对象无缝地进行，并且随着上传的进行，进度更新可用。对于旧版浏览器，使用后备技术，涉及`iframe`。[更多信息](http://malsup.com/jquery/form/#file-upload)

---

## 贡献者
该项目已由[github.com/malsup/form](https://github.com/malsup/form/) 转让， 由[Mike Alsup](https://github.com/malsup)提供。
详情参照 [CONTRIBUTORS](CONTRIBUTORS.md)。

## 许可

该项目根据MIT和LGPLv3许可证进行双重许可：

* [MIT](LICENSE)
* [GNU Lesser General Public License v3.0](LICENSE-LGPLv3)

---

3.51-的附加文档和示例：[http://malsup.com/jquery/form/](http://malsup.com/jquery/form/)
