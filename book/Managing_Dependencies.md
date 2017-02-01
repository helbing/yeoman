### Managing Dependencies

1. 安装`npm`依赖

   ```javascript
   this.npmInstall(['lodash'], { 'saveDev': true });

   // 相当于执行
   npm install lodash --save-dev
   ```

2. 安装`yarn`依赖

   ```javascript
   this.yarnInstall(['lodash'], { 'dev': true });

   // 相当于执行
   yarn add lodash --dev
   ```

3. 安装`bower`依赖

   ```javascript
   this.bowerInstall(['lodash']);

   相当于执行
   bower install lodash
   ```

4. 安装`composer`依赖

   ```javascript
   this.spawnCommand('composer', ['install']);

   // 相当于执行命令
   composer install
   // 也可以用于执行其他的包管理工具
   ```

5. 结合使用

   ```javascript
   this.installDependencies({
     	npm: false,
      	bower: true,
     	yarn: true
   });

   // 自动安装所有的依赖，可以执行哪些依赖不安装
   ```

   ​