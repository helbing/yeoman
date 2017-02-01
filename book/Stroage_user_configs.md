### Stroage user configs

- `config.set()`

  > 用法

  ```javascript
  this.config.set('name', 'Yeoman Demo')
  ```

  在根目录会生成`.yo-rc.json`，配置信息好保存在其中

- `config.get()`

  > 用法

  ```javascript
  this.config.get('name');
  ```

  会获取到`.yo-rc.json`中的配置，如果没有返回`undefined`

- `config.getAll()`

  > 用法

  ```javascript
  this.config.getAll();
  ```

  获取到配置`json`对象

- `config.delete()`

  > 用法

  ```javascript
  this.config.delete('name');
  ```

  删除配置信息

- `config.defaults()`

  > 用法

  ```javascript
  this.config.defaults({
    	name: 'change default'
  });
  ```

  如果在`.yo-rc.json`中存在`name`这个配置项，那么不发生改变