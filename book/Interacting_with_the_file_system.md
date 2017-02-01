### Interacting with the file system

#### 设置目录

> 设置源目录

- 通过`this.sourceRoot()`可以获取到默认的源目录，默认为当前`generator`的下的`templates`

  ```tex
  /Users/helbing/Code/generator-helbing-vue/generators/app/templates
  ```

- 通过给`sourceRoot`函数传值可以修改源目录

  ```javascript
  this.sourceRoot('new/template/path');
  ```

  ```tex
  // 结果为
  /Users/helbing/Code/yeoman/new/template/path
  // 相当于
  // ./new/template/path
  ```

> 设置目标目录

- 通过`this.destinationRoot()`可以获取到目标目录，即是当前目录路径
- 通过给`this.destinationRoot`函数传值可以修改目标目录

#### 复制文件

> 复制文件

复制源目录下的`index.html`到目标目录下的`public/index.html`

```javascript
this.fs.copy(
	this.templatePath('index.html'),
   	this.destinationPath('public/index.html')
);
```

> 复制模板

复制源目录下的`index.html`模板文件到目标目录下的`public/index.html`并渲染`title`值（使用的模板引擎为`ejs`）

```javascript
this.fs.copyTpl(
	this.templatePath('index.html'),
   	this.destinationPath('public/index.html'),
  	{ title: 'Templating with Yeoman' }
);
```

`index.html`的内容为

```ejs
<html>
  <head>
    <title><%= title %></title>
  </head>
</html>
```

