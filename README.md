# gulp-muli
gulp 常规配置 ，构建前端自动化工作流 ，可用于多页面
## 项目启动

```
// 常用命令
开发环境： npm run dev
生产环境： npm run build

## 项目目录
```
```
├── README.md         # 项目说明
├── config            # gulp路径配置
├── dist              # 打包路径
|
├── gulpfile.js       # gulp配置文件
├── package.json      # 依赖包
|
├── src               # 项目文件夹
│   ├── include       # 公用页面引入
│   ├── index.html    # 首页
|   ├── detail.html   # 详情页
|   |—— ...其他页面
│   ├── static        # 资源文件夹
│   │   ├── images    # 图库
│   │   ├── js        # 脚本
│   │     └── lib     # 引入的插件
│   │     └── xx.js   # 页面相应的脚本
│   │   ├── styles    # 样式（scss, css）
│   │     └── css     # 编译后的样式
│   │     └── fonts   # 字体文件
│   │     └── less    # less文件
│   │     └── lib     # 插件样式
│   │   ├── sprite    # 精灵图
│   └── views         # 页面
|
├── assets            # 打包到dist中assets文件中

```
```
assets文件夹
* 一级目录中assets文件夹，可以存放不需要编译的文件内容，比如一些插件，图片，字体文件等
```
// gulp task
```

执行压缩： gulp zip
编译页面： gulp html
编译脚本： gulp scripts
编译样式： gulp styles
压缩图片： gulp images
  ...
  
```
```
可直接用于移动端 基于750
精灵图需改对的应插件，单位用rem
   1.首先修改gulp.spritesmith\node_modules\spritesheet-templates\lib\spritesheet-templates.js
   
    ```
    ['x', 'y', 'offset_x', 'offset_y', 'height', 'width', 'total_height', 'total_width'].forEach(function (key) {
    if (item[key] !== undefined) {
      px[key] = item[key]/75 + 'rem';
    }
    });
    ```
   修改的地方是item[key]/75+'rem';这句，我的是设置了750px宽度，所以这里除以75来转换得到rem值。

   2.修改gulp.spritesmith\node_modules\spritesheet-templates\lib\templates\css.template.handlebars
    在模板页中加入生成background-size内容
    {{/block}}
      {{#block "sprites"}}
      .cicon {
          display: inline-block;
          background-size: {{spritesheet.px.width}} {{spritesheet.px.height}};
      }
      {{#each sprites}}
      {{{selector}}} {
        background-image: url({{{escaped_image}}});
        background-position: {{px.offset_x}} {{px.offset_y}};
        width: {{px.width}};
        height: {{px.height}};
      }
      {{/each}}
    {{/block}}
    
     PS：rem单位下在不同设备中可能出现图片中出现了雪碧图中其他图的边边角角，所以这里需要设置图片合成的时候彼此之间有一定的间隙，
     这个只要是gulpfile中设置下padding即可  不想用精灵图或者pc端直接不用理会这些
  ```
   ```
   其他看gulpfile.js注释。
   ```
