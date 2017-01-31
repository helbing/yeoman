### User Interactions

#### 交互

`Prompts`是`generator`和用户交互的主要方式，`yeoman-generator`的`prompt`是有[Inquirer.js](https://github.com/SBoudrias/Inquirer.js)提供的，你可以参考它的`API`文档

`Prompts`的使用方式如下所示

```javascript
module.exports = class extends Generator {
  	prompting() {
    	return this.prompt([{
          	// 文本类型
      		type    : 'input',
      		name    : 'name',
      		message : 'Your project name',
          	// 设置默认值，默认为当前的目录名
      		default : this.appname
        }, {
          	// 布尔类型
      		type    : 'confirm',
      		name    : 'cool',
      		message : 'Would you like to enable the Cool feature?',
          	default	: false
    	}]).then((answers) => {
          	// 回调，获取到用户的输入
          	// 其他的输入类型，可以去参考Inquirer.js的官方文档
      		this.log('app name', answers.name);
      		this.log('cool feature', answers.cool);
    	});
  	}
};
```

#### 记住用户喜好

用户可能会多次用同一个脚手架创建项目，那么他每一次都得重新输入一遍他创建项目的一些偏好。`Yeoman`拓展了`Inquirer.js`并加入了`store`属性用于记录用户的输入，那么当用户下次再使用这个脚手架创建项目的时候，会将上一次输入的值作为默认值，如: 

```javascript
this.prompt({
  	type    : 'input',
  	name    : 'username',
  	message : 'What\'s your Github username',
  	store   : true
});
```

#### Arguments

假设我们需要输入`yo yeoman-name project-nam`这样的命令，那么我们就可以使用`argument`获取到`project-name`的值

> 例子

```javascript
var Generator = require('yeoman-generator');

module.exports = class extends Generator {

    constructor(args, opts) {
        super(args, opts);

        // 使用argument获取到appname的值，argument只能写在构造函数里
        this.argument('appname', {
            type: String,
            required: true,
            default: 'test'
        });
    }

    end() {
        // 输出值
        this.log(this.options.appname);
    }

};
```

> 配置项

1. `desc`，参数描述
2. `required` ，Boolean,是否是必需的
3. `optional` ，Boolean,是否是可选的
4. `type`，值为`String`，`Number`，` Array`或`Object`
5. `default`，默认值

#### Options

假设我们需要输入`yo yeoman-name --type=coffee`这样的命令，那么我们就可以使用`option`判断命令中是否有`--coffees`这个项

> 例子

```javascript
module.exports = class extends Generator {
  	// note: arguments and options should be defined in the constructor.
  	constructor(args, opts) {
    	super(args, opts);

    	// option只能写在构造函数里
    	this.option('type', {
          	type: String,
          	default: 'js'
    	});
  	}
  
  	end() {
      	this.log(this.options.type);
  	}
});
```

> 配置项

1. `desc`，选项描述
2. `alias`，别名
3. `type`，值为`Boolean`，`String`或`Number`，如果`type: Boolean`命令中可以不需要带值，如: `yo yeoman-name --type`
4. `default`，默认值
5. `hide`，Boolean，是否从帮助隐藏

