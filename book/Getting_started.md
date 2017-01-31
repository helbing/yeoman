### Getting started

#### 1. 创建目录

创建一个目录作为你的脚手架目录，`mkdir generaotr-name`，`name`为你的脚手架名称

#### 2. ``package.json`配置

1. 通过`npm init`初始化你的`package.json`文件

2. `package.json`的必要配置

   - `name`属性必须是`generator-`开头]
   - `keywords`属性必须包含`yeoman-generator`，这样才会`Yeoman`官方收录，[http://yeoman.io/generators/](http://yeoman.io/generators/)
   - 如果需要添加其他的属性可以去 [npm官网文档](https://docs.npmjs.com/files/package.json#files) 中查看。

   例子:

   ```json
   {
     "name": "generator-demo",
     "version": "0.0.1",
     "description": "a generator demo",
     "keywords": [
       "yeoman-generator"
     ]
   }
   ```

3. 安装`yeoman-generator`这个必要的依赖包，`npm install --save yeoman-generator`

#### 3. 创建`generaotr`

1. 在脚手架目录中新建`generator`目录，如`mkdir -p generators/app`

2. 在`generator`目录中，创建`index.js`，并写入

   ```javascript
   var Generator = require('yeoman-generator');

   module.exports = class extends Generator {

       constructor(args, opts) {
           super(args, opts);
       }

       initializing() {
           this.log('Hello World');
       }
   };
   ```

   > （*）这里使用的`yoeman-generator`版本是`1.1.0`版本，与`1.0.0`以下的版本的语法差别有点大

3. 注册`generator`，在根目录的`package.json`写入

   ```json
   "files": [
     "generators/app"
   ]
   ```

#### 4. 本地测试

1. 全局安装`yo`，`npm install -g yo`
2. 本地安装你的脚手架，在脚手架根目录执行`npm link`，`npm`会将当前的`npm`包复制一份到`npm`的全局安装目录
3. 执行`yo`命令，选择你的脚手架然后回车，这时候就能看到`Hello World`了