#### 一个能够让你快速构建AngularJS项目的轻量级脚手架。
目前市面上已经存在众多脚手架，包括：`vue-cli`、`create-react-app`、`angular-cli`等，但大多数脚手架均针对单页应用。面对多页应用，用户需要手写 `webpack.config.js`，手动创建每个页面的入口文件、`index.html`并配置到`entry`中，可以说配置比较繁琐。
#### 使用angular-m-cli，您可以做到
* 快速构建项目原型
* 自动生成新页面并完成相关配置（包括入口文件、css、`index.html`）
* 利用本地服务器完成开发
* 使用`mock`进行接口测试，实现前后端分离
* 一键删除指定页面和相关配置
* 一键打包

### 如何使用
 #### 一、起步
你需要将该项目克隆到本地，安装相关依赖
```
git clone https://github.com/1335382915/angular-m-cli.git
//进入到angular-m-cli目录下
npm i
```

除此之外，你还需要将该工程链接到全局执行环境中，方便全局使用
```
npm link
```

现在，我们随便进入一个文件夹，输入`ng`命令，看看是否可以全局使用了

![](http://upload-images.jianshu.io/upload_images/1495096-6399e6a2495d7741.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* -v | --version：查看angular-m-cli版本
* -h | --help：查看帮助
* init | i  <projectName>：创建一个新的项目
* add | a  <pageName>：添加新的页面
* list | l ：列举出所有页面
* delete | d <pageName>：删除指定页面

#### 二、使用
注：所有的命令均必须在项目的根目录中输入，在本例子中是`NGApp`
##### 1. 创建工程

在命令行中输入`npm init NGApp`，等待片刻

![](http://upload-images.jianshu.io/upload_images/1495096-04c154cffc6bd046.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进入到`NGApp`文件夹下，安装依赖`cd NGApp && npm i`

完成依赖安装后，我们就可以使用`npm start`启动项目啦

![angular-m-cli](http://upload-images.jianshu.io/upload_images/1495096-bb4e61b121dfd594.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

访问3005端口即可进入`mock`服务的配置页面

![](http://upload-images.jianshu.io/upload_images/1495096-80206821e105aba0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面我们来看一下目录结构

![](http://upload-images.jianshu.io/upload_images/1495096-4142b17ce01198e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

* `common`存放通用的js代码块
* `css`存放每个页面的样式文件，默认只有`home.scss`

* `doc`存放mock文档
* `entry`存放每个页面的入口js文件，例如：home页的入口文件为`entry/home/main.js`
```
import angular from 'angular';
import {homeInstruction} from '../../common/app';
import '../../css/home.scss';

const homeCtrl = $scope => {
	$scope.pageInfo = "Hello Angular";
	console.log(homeInstruction);
}
const helloNG = () => ({
	restrict:'EACM',
	template:`<p class="home-title" >{{pageInfo}}</p>`
})

angular.module('home', [])
	.controller('homeCtrl',['$scope', 
		$scope => homeCtrl($scope)
	])
	.directive('helloNg', helloNG)
	.directive('ngText', homeInstruction);
```
使用es6去书写ng代码，能够大幅提高代码可读性和组织性
* `image`存放项目图片
* `mock2easy`
*   `node_modules`
* `pages`存放页面html，目录结构与entry一致，例如：home页的html文件为`pages/home/index.html`
```
<!DOCTYPE html>
<html ng-app="home">
<head>
	<title></title>
</head>
<body ng-controller="homeCtrl" ng-cloak>
	<div class="container" >
		<img src="../../image/angular.jpg" />
	</div>
	<hello-ng></hello-ng>
	<ng-text></ng-text>
	<script type="text/javascript" src="https://cdn.bootcss.com/angular.js/1.6.6/angular.min.js"></script>
	<script type="text/javascript" src="/vendor.__bundle.js"></script>
	<script type="text/javascript" src="/home/main.__bundle.js"></script>
</body>
</html>
```
最下面的两个 `script`标签引入的是虚拟内存中的文件，在生产环境中我们需要将这两个标签去除。
* `package.json`
* `webpack.config.js`
##### 2. 添加新页面
在命令行中输入`npm add user`，我们再一次启动项目，将url中的`home`改为`user`

![](http://upload-images.jianshu.io/upload_images/1495096-491d9bfd3203d6f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/1495096-0162f9cab02c9a7a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看似简单的操作，但实质上user的相关文件和配置已经自动生成
```
├── css
│   ├── home.scss
│   └── user.scss
├── entry
│   ├── home
│   │   ├── main.js
│   └── user
│       ├── main.js
├── pages
│   ├── home
│   │   ├── index.html
│   └── user
│       ├── index.html
└── common
```

##### 3. 开发环境
目前，我们已经可以创建新的项目，添加新的页面并成功将项目运行在本地，也就是开发环境。你需要知道开发环境下angular-m-cli可以做哪些事情：
* 使用es6书写ng
* 编写sass
* 模块导入sass、css
* 自动打开首页（home）
* 热更新css文件和js文件
* 错误映射
* 使用mock服务测试接口

##### 4. 生产环境
在命令行中输入`npm run build`，构建成功后我们可以看到`entry`文件夹发生了变化：
```
├── entry
│   ├── home
│   │   ├── main.js
│   │   ├── main.bundle.js //打包后的home所需js
│   │   └── main.bundle.css //打包后的home所需css
│   ├── user
│   │   ├── main.js 
│   │   ├── main.bundle.js //打包后的user所需js
│   │   └── main.bundle.css //打包后的user所需css
│   └── vender.bundle.js //打包后的通用js
```

下一步，你并不需要手动将打包后的文件引入到html中，因为这个工作已经由angular-m-cli自动帮你完成，你只需要将每个页面的
```
<script type="text/javascript" src="/vendor.__bundle.js"></script>
<script type="text/javascript" src="/pageName/main.__bundle.js">
```
删除即可。

`pageName`为你的页面名

现在，你可以将`entry`、`pages`、`image`这三个文件夹存放至服务器了。

![](http://upload-images.jianshu.io/upload_images/1495096-f80bdfd8ce2955ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 5. 其他指令
* 列举页面：`ng list`
* 删除页面及其配置项：`ng delete <pageName>`

##### 6. 自定义配置（webpack.config.js）
1. 指定通用模块

通用模块在开发环境下会生成到虚拟内存中：`/vender.__bundle.js`，在生产环境下会生成到`entry/vender.bundle.js`中。

```
//默认的通用模块
var commonModule1 = path.resolve(__dirname, customConfig.jsCommonDir + '/app');
//添加新的通用模块
var commonModule2 = path.resolve(__dirname, customConfig.jsCommonDir + '/myModule');

var entry = {
    vendor: [commonModule1, commonModule2]
};
```
通过以上配置，`common/app.js`和`common/myModule.js`均会被打包至vender中。

2. 开发环境下修改自动打开的页面
```
var customConfig = {
    htmlDir: 'pages',
    htmlEntry: 'index.html',
    jsDir: 'entry',
    jsCommonDir: 'common',
    jsEntry: 'main.js',
    serverEntryDir: 'home', //将其修改为user即可自动打开user页面
    devServerPort: 3000
};
```

##### 6. 生产环境转换成开发环境

删除`<link href="../../entry/pageName/main.bundle.css" rel="stylesheet"/>`
将
```
<script type="text/javascript" src="../../entry/vendor.bundle.js"></script>
<script type="text/javascript" src="../../entry/pageName/main.bundle.js"></script>
```
替换为
```
<script type="text/javascript" src="/vendor.bundle.js"></script>
<script type="text/javascript" src="/pageName/main.bundle.js">
```
`pageName`为你的页面名
