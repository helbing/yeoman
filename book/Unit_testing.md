### Unit testing

1. `Yeoman`的单元测试依赖于`yeoman-test`和`mocha`

   > 安装依赖

   ```tex
   npm install --save-dev yeoman-test mocha
   ```

   > 基本骨架

   - 在根目录创建`tests`目录，所有的测试文件都放在其中
   - 在`package.json`中加入

   ```json
   "scripts": {
     "test": "./node_modules/mocha/bin/mocha ./tests/*/*.test.js"
   },
   ```

   以后需要跑单元测试的时候，只需要在脚手架的根目录执行`npm test`即可

2. 测试准备

   我们要测试的脚手架有两个`generaotor`

   ```tex
   |- package
   |- generators
   |  |- app
   |  |  |- index.js
   |  |  |- templates
   |  |  |  |- copy.html
   |  |- route
   |  |  |- index.js
   |  |  |- templates
   |  |  |  |- tpl.html
   ```

   > 测试项

   - `generator`互相调用间如何测试
   - 复制文件和复制模板如何测试
   - `argument`和`option`如何测试

   >app/index.js

   ```javascript
   var Generator = require('yeoman-generator');
   var _ = require('lodash');

   module.exports = class extends Generator {

       constructor(args, opts) {
           super(args, opts);

           this.argument('name', {
               type: String,
               default: 'helbing'
           });

           this.option('sex', {
               type: String,
               default: 'man'
           });
       }

       prompting() {
           var done = this.async();

           return this.prompt([{
               type    : 'input',
               name    : 'age',
               message : 'How old are you ?',
               default : 22
           }]).then(function (answers) {

               this.options = _.merge(this.options, answers);

               done();
           }.bind(this));
       }

       writing() {

           this.composeWith(require.resolve('../route'), {
               name: this.options.name,
               age: this.options.age,
               sex: this.options.sex
           });

           this.fs.copy(
               this.templatePath('copy.html'),
               this.destinationPath('public/copy.html')
           );
       }
   };
   ```

   > app/templates/copy.html

   ```html
   <!doctype html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport"
             content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
       <meta http-equiv="X-UA-Compatible" content="ie=edge">
       <title>Document</title>
   </head>
   <body>
       This is html
   </body>
   </html>
   ```

   > route/index.js

   ```javascript
   var Generator = require('yeoman-generator');

   module.exports = class extends Generator {

       constructor(args, opts) {
           super(args, opts);

           this.option('name', {
               type: String,
               default: 'helbing'
           });

           this.option('age', {
               type: Number
           });

           this.option('sex', {
               type: String,
               default: 'man'
           });
       }

       writing() {

           this.fs.copyTpl(
               this.templatePath('tpl.html'),
               this.destinationPath('public/tpl.html'),
               {
                   name: this.options.name,
                   age: this.options.age,
                   sex: this.options.sex
               }
           );
       }

   };
   ```

   > route/templates/tpl.html

   ```html
   <!doctype html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport"
             content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
       <meta http-equiv="X-UA-Compatible" content="ie=edge">
       <title>Document</title>
   </head>
   <body>
       <%= name %><br><%= age %><br><%= sex %><br>
   </body>
   </html>
   ```

3. 开始测试

   - 创建`tests/app/index.test.js`用于测试`generator`

   - 在`index.test.js`中写入

     ```javascript
     var helpers = require('yeoman-test');
     var assert = require('yeoman-assert');
     var path = require('path');
     var tempDir = path.join(__dirname, '../temp');

     describe('helbing-vue:app', function () {

         it('文件复制', function(done) {

             // 生成运行上下文，指向/generator/app/index.js
             var runContext = helpers.run(path.join(__dirname, '../../generators/app'));

             // 设置临时目标目录，临时目标目录为/tests/temp
             runContext.inDir(tempDir);

             // argument参数
             runContext.withArguments([

                 'helbing'
             ]);

             // option参数
             runContext.withOptions({

                 sex: 'man'
             });

             // prompt参数
             runContext.withPrompts({

                 age: 23
             });

             runContext.on('end', function () {

                 // 判断文件有没有生成
                 assert.file([

                     tempDir + '/public/copy.html',
                     tempDir + '/public/tpl.html'
                 ]);

                 // 判断文件内容是否符合要求
                 assert.fileContent(
                     tempDir + '/public/copy.html',
                     /This is html/
                 );

                 assert.fileContent(
                     tempDir + '/public/tpl.html',
                     /helbing<br>23<br>man<br>/
                 );

                 // 删除临时目标目录
                 this.cleanTestDirectory();

                 done();
             });
         });

     });
     ```

   - 在脚手架的根目录执行`npm test`开始测试