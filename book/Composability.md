### Composability

`Composabiity`即是允许`generator`间互相调用，这么做的好处是防止单个`generator`代码量过大，难以维护，也方便日后在其他脚手架中复用。

> 例子

1. 创建`generators/app/index.js`和`generators/route/index.js`

   > 目录结构如下所示

   ```tex
   |- package.json
   |- generators
   |  |- app
   |  |  |- index.js
   |  |- route
   |  |  |- index.js
   ```

2. 注册`generator`，在`package.json`中写入

   ```json
   "files": [
     "generators/app",
     "generators/route"
   ],
   ```

3. 初始化两个`generator`

   ```javascript
   var Generator = require('yeoman-generator');

   module.exports = class extends Generator {

       constructor(args, opts) {
           super(args, opts);
       }
     	
     	end() {
           this.log(this.options.namespace);
       }
   };
   ```

4. 使用`composeWith`函数关联两个`generator`

   - 在`app/index.js`的构造函数（可以是任意一个生命周期）中写入

   ```javascript
   this.composeWith(require.resolve('../route'));
   ```

   - 这时候执行`yo name`，`name`是你定义的脚手架名称，结果为

   ```javascript
   name:app
   name:route
   ```

5. `option`传递

`composeWith`的第二个参数可以用于传递`option`，`generator`间无法传递`argument`

> (*) 在`yeoman-generator`的`1.0.0`以下版本是允许传递`argument`

```javascript
this.composeWith(require.resolve('../route'), {
  	type: 'coffee'
});
```

在`generators/route/index.js`的构造函数中加入

```javascript
this.option('type', {
 	type: String
});

this.log('type: ' + this.options.type);
```

这时候执行`yo name`，`name`是你定义的脚手架名称，结果为

```javascript
type: coffee
name:app
name:route
```

