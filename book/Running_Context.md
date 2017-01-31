### Running Context

#### 生命周期

`Yeoman`的生命周期主要分为

1. `constructor`，构造函数
2. `initializing`，初始化方法，
3. `prompting`，用户提示选项
4. `configuring`，保存用户配置项且配置项目（如: 创建`.editorconfig`等）
5. `default`，默认函数
6. 自定义函数阶段，如下面例子中创建的`method1`和`method2`，如果不想创建的自定义函数自动执行，可以将函数设置为私有的，只需要在函数名称中加入`_private_`前缀
7. `writing`，处理一些特殊操作，如: 路由，控制器等
8. `conflicts`，处理冲突
9. `install`，安装依赖阶段，如: `npm`，`bower`和`composer`
10. `end`，最后阶段，用于清除不需要的文件或提示信息等

> 例子

```javascript
var Generator = require('yeoman-generator');

module.exports = class extends Generator {

    constructor(args, opts) {
        super(args, opts);
        this.log('This is constructor');
    }
  
  	method1() {
      	this.log('This is method1');
  	}
  	
  	method2() {
      	this.log('This is method2');
  	}

    initializing() {
        this.log('This is initializing');
    }

    prompting() {
        this.log('This is prompting');
    }

    configuring() {
        this.log('This is configuring');
    }

    default() {
        this.log('This is default');
    }

    writing() {
        this.log('This is writing');
    }

    conflicts() {
        this.log('This is conflicts');
    }

    install() {
        this.log('This is install');
    }

    end() {
        this.log('This is end');
    }

};
  
/**
 * 结果为
 * This is constructor
 * This is initializing
 * This is prompting
 * This is configuring
 * This is method1
 * This is method2
 * This is default
 * This is writing
 * This is conflicts
 * This is install
 * This is end
 */
```

虽然`Yeoman`定义了这些生命周期，但是可以完全不按照默认的来，可以将所有操作都写在一个函数里。但是使用官方定义的生命周期会更容易来管理我们写的`generator`

#### 异步任务

有多种方法可以暂停运行循环，直到任务完成做异步工作。

最简单的方法是 **return a promise**。一旦`promise`解决，循环将继续，或者如果它失败了，将引发一个异常。

如果你依赖的异步接口不支持`promise`，然后你可以依赖 `this.async()` 的方法。调用 `this.async()`会返回一个函数，一旦任务完成后调用。例如：

```javascript
asyncTask: function () {
	var done = this.async();

  	getUserEmail(function (err, name) {
    	done(err);
  	});
}
```

如果`done`函数调用一个错误的参数，运行循环将停止并引发异常。