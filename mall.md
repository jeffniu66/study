---
typora-root-url: ../study
typora-copy-images-to: ./mall_images
---

# 1. SpringCloud Alibaba

## 1.1 SpringCloud Alibaba介绍

<img src="mall_images/image-20201211233213094.png" alt="image-20201211233213094" style="zoom:50%;" />

版本适配

![image-20201211234738534](/mall_images/image-20201211234738534.png)

## 1.2 SpringCloud Alibaba-Nacos作为注册中心

github地址

```java
https://github.com/alibaba/spring-cloud-alibaba/blob/master/README-zh.md
```

Linux/Unix/Mac操作系统，执行命令启动nacos

```shell
sh startup.sh -m standalone
```

nacos访问地址

```java
http://127.0.0.1:8848/nacos/#/login
```

## 1.6 SpringCloud Alibaba-OSS



# 4. Vue

## 4.1 MVVM思想

M: 即Model，模型，包括一些数据和一些基本操作

V: 即View，视图，页面渲染结果

VM: 即View-Model，模型与视图间的双向操作（无需开发人员干涉）

在MVVM之前，开发人员从后端获取需要的数据模型，然后通过DOM操作Model，渲染到View中。而后当用户操作视图，我们还需要通过View中的数据，然后同步到Model中。

而MVVM中的VM要做的事情是把DOM操作完全封装起来，开发人员不需要关心Model和View之间如何互相影响。

## 4.2 Vue介绍及使用

1.使用 npm init -y 初始化项目，初始化后会生成package.json文件，表示由npm来管理的项目。

2.使用 npm install vue 来安装vue相关包，会生成node_modules/vue目录。

<img src="mall_images/image-20201205095805181.png" alt="image-20201205095805181" style="zoom:50%;" />

3.Vue版HelloWorld

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model="num" />
        <h1>{{name}},非常帅,有{{num}}个人点赞</h1>
    </div>

    <script src="./node_modules/vue/dist/vue.js"></script>
    
    <script>
        let vm = new Vue({ // 创建一个vue对象
            el: "#app", // el全称element
            data: {
                name: "张三",
                num: 1
            }
        });
        
    </script>
</body>
</html>
```

## 4.3 Vue基本语法和插件安装

vscode可以安装vetur和vue 3 snippets插件，方便提示代码。

chrome可以安装vue devtool插件。

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <input type="text" v-model="num" />
        <button v-on:click="num++">点赞</button>
        <button v-on:click="cancel">取消</button>
        <h1>{{name}},非常帅,有{{num}}个人点赞</h1>
    </div>

    <script src="./node_modules/vue/dist/vue.js"></script>
    
    <script>
        // 1.vue声明式渲染
        let vm = new Vue({ // 创建一个vue对象
            el: "#app", // el全称element
            data: { // 绑定数据
                name: "张三",
                num: 1
            },
            methods: { // 封装方法
                cancel(){
                    this.num--;
                }
            },
        });
        // 2.双向绑定，模型变化，视图变化。反之亦然。

        // 3.事件处理

        // 4.v-xx 指令

        // 1、创建Vue实例，关联页面的模板，将自己的数据（data）渲染到关联的模板，响应式的
        // 2、指令来简化对DOM的一些操作
        // 3、声明方法来做更简单的操作。methods里面可以封装方法
    </script>
</body>
</html>
```

## 4.4 Vue指令-单向绑定&双向绑定

浏览器可以模拟网速慢的效果，开3G

<img src="mall_images/image-20201205112147895.png" alt="image-20201205112147895" style="zoom:50%;" />

### 4.4.1 v-text、v-html

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        {{msg}} {{1+1}} {{hello()}}<br/> <!--网速慢情况下存在插值闪烁问题-->
        <span v-html="msg"></span>
        <br/>
        <span v-text="msg"></span>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        new Vue({
            el:"#app",
            data:{
                msg:"<h1>Hello</h1>"
            },
            methods: {
                hello(){
                    return "World"
                }
            },
        })
    </script>
</body>
</html>
```

### 4.4.2 v-bind

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <a v-bind:href="link">gogogo</a>

        <!-- class,style {class名: 加上?}-->
        <span v-bind:class="{active:isActive, 'text-danger':hasError}"
        :style="{color: color1, fontSize: size}">你好</span>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                link: "http://www.baidu.com",
                isActive: true,
                hasError: true,
                color1: 'red',
                size: '36px'
            }
        })
    </script>
</body>
</html>
```

### 4.4.3 v-model

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>

    <!-- 表单项，自定义组件 -->
    <div id="app">
        精通的语言：
            <input type="checkbox" v-model="language" value="Java"/>Java<br/>
            <input type="checkbox" v-model="language" value="C++"/>C++<br/>
            <input type="checkbox" v-model="language" value="Python"/>Python<br/>
        选中了{{language.join(",")}}
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el: "#app",
            data: {
                language: []
            }
        })
    </script>
</body>
</html>
```

### 4.4.4 v-on

```vue
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div id="app">

        <!--事件中直接写js片段-->
        <button v-on:click="num++">点赞</button>
        <!--事件指定一个回调函数，必须是Vue实例中定义的函数-->
        <button @click="cancel()">取消</button>
        <!---->
        <h1>有{{num}}个赞</h1>

        <!-- 事件修饰符 -->
        <div style="border: 1px solid red;padding: 20px;" v-on:click.once="hello">
            大div
            <div style="border: 1px solid blue;padding: 20px" @click.stop="hello">
                小div<br/>
                <a href="http://www.baidu.com" @click.prevent.stop="hello">百度</a>
            </div> 
        </div>

        <!-- 按键修饰符 -->
        <input type="text" v-model="num" v-on:keyup.up="num+=2" @keyup.down="num-=2"/><br/>

        提示
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        let vm = new Vue({
            el:"#app",
            data:{
                num: 1
            },
            methods: {
                cancel(){
                    this.num--;
                },
                hello(){
                    alert("点击了")
                }
            },
        })
    </script>
</body>
</html>
```

### 4.4.5 v-for

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <ul>
            <li v-for="(user,index) in users" :key="user.name" v-if="user.gender == '女'">
                <!-- 1、显示user信息：v-for="item in items" -->
               当前索引：{{index}} ==> {{user.name}}  ==>   {{user.gender}} ==>{{user.age}} <br>
                <!-- 2、获取数组下标：v-for="(item,index) in items" -->
                <!-- 3、遍历对象：
                        v-for="value in object"
                        v-for="(value,key) in object"
                        v-for="(value,key,index) in object" 
                -->
                对象信息：
                <span v-for="(v,k,i) in user">{{k}}=={{v}}=={{i}}；</span>
                <!-- 4、遍历的时候都加上:key来区分不同数据，提高vue渲染效率 -->
            </li>

            
        </ul>

        <ul>
            <li v-for="(num,index) in nums" :key="index"></li>
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>
    <script>         
        let app = new Vue({
            el: "#app",
            data: {
                users: [{ name: '柳岩', gender: '女', age: 21 },
                { name: '张三', gender: '男', age: 18 },
                { name: '范冰冰', gender: '女', age: 24 },
                { name: '刘亦菲', gender: '女', age: 18 },
                { name: '古力娜扎', gender: '女', age: 25 }],
                nums: [1,2,3,4,4]
            },
        })
    </script>
</body>

</html>
```

### 4.4.6 v-if、v-show

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <!-- 
        v-if，顾名思义，条件判断。当得到结果为true时，所在的元素才会被渲染。
        v-show，当得到结果为true时，所在的元素才会被显示。 
    -->
    <div id="app">
        <button v-on:click="show = !show">点我呀</button>
        <!-- 1、使用v-if显示 -->
        <h1 v-if="show">if=看到我....</h1>
        <!-- 2、使用v-show显示 -->
        <h1 v-show="show">show=看到我</h1>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>
        
    <script>
        let app = new Vue({
            el: "#app",
            data: {
                show: true
            }
        })
    </script>

</body>

</html>
```

### 4.4.7 v-else、v-else-if

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <button v-on:click="random=Math.random()">点我呀</button>
        <span>{{random}}</span>

        <h1 v-if="random>=0.75">
            看到我啦？！ &gt;= 0.75
        </h1>

        <h1 v-else-if="random>=0.5">
            看到我啦？！ &gt;= 0.5
        </h1>

        <h1 v-else-if="random>=0.2">
            看到我啦？！ &gt;= 0.2
        </h1>

        <h1 v-else>
            看到我啦？！ &lt; 0.2
        </h1>

    </div>


    <script src="../node_modules/vue/dist/vue.js"></script>
        
    <script>         
        let app = new Vue({
            el: "#app",
            data: { random: 1 }
        })     
    </script>
</body>

</html>
```

## 4.5 计算属性和监听器

### 4.5.1 计算属性和侦听器

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <!-- 某些结果是基于之前数据实时计算出来的，我们可以利用计算属性。来完成 -->
        <ul>
            <li>西游记； 价格：{{xyjPrice}}，数量：<input type="number" v-model="xyjNum"> </li>
            <li>水浒传； 价格：{{shzPrice}}，数量：<input type="number" v-model="shzNum"> </li>
            <li>总价：{{totalPrice}}</li>
            {{msg}}
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>
        //watch可以让我们监控一个值的变化。从而做出相应的反应。
        new Vue({
            el: "#app",
            data: {
                xyjPrice: 99.98,
                shzPrice: 98.00,
                xyjNum: 1,
                shzNum: 1,
                msg: ""
            },
            computed: {
                totalPrice(){
                    return this.xyjPrice*this.xyjNum + this.shzPrice*this.shzNum
                }
            },
            watch: {
                xyjNum(newVal,oldVal){
                    if(newVal>=3){
                        this.msg = "库存超出限制";
                        this.xyjNum = 3
                    }else{
                        this.msg = "";
                    }
                }
            },
        })
    </script>

</body>

</html>
```

### 4.5.2 过滤器

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <!-- 过滤器常用来处理文本格式化的操作。过滤器可以用在两个地方：双花括号插值和 v-bind 表达式 -->
    <div id="app">
        <ul>
            <li v-for="user in userList">
                {{user.id}} ==> {{user.name}} ==> {{user.gender == 1?"男":"女"}} ==>
                {{user.gender | genderFilter}} ==> {{user.gender | gFilter}}
            </li>
        </ul>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>

    <script>

        Vue.filter("gFilter", function (val) {
            if (val == 1) {
                return "男~~~";
            } else {
                return "女~~~";
            }
        })

        let vm = new Vue({
            el: "#app",
            data: {
                userList: [
                    { id: 1, name: 'jacky', gender: 1 },
                    { id: 2, name: 'peter', gender: 0 }
                ]
            },
            filters: {
                //// filters 定义局部过滤器，只可以在当前vue实例中使用
                genderFilter(val) {
                    if (val == 1) {
                        return "男";
                    } else {
                        return "女";
                    }
                }
            }
        })
    </script>
</body>

</html>
```

## 4.6 组件化

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>

    <div id="app">
        <button v-on:click="count++">我被点击了 {{count}} 次</button>

        <counter></counter>
        <counter></counter>
        <counter></counter>
        <counter></counter>
        <counter></counter>

        <button-counter></button-counter>
    </div>
    <script src="../node_modules/vue/dist/vue.js"></script>


    <script>
        //1、全局声明注册一个组件
        Vue.component("counter", {
            template: `<button v-on:click="count++">我被点击了 {{count}} 次</button>`,
            data() {
                return {
                    count: 1
                }
            }
        });

        //2、局部声明一个组件
        const buttonCounter = {
            template: `<button v-on:click="count++">我被点击了 {{count}} 次~~~</button>`,
            data() {
                return {
                    count: 1
                }
            }
        };

        new Vue({
            el: "#app",
            data: {
                count: 1
            },
            components: {
                'button-counter': buttonCounter
            }
        })
    </script>
</body>

</html>
```

## 4.7 生命周期和钩子函数

```vue
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <span id="num">{{num}}</span>
        <button @click="num++">赞！</button>
        <h2>{{name}}，有{{num}}个人点赞</h2>
    </div>

    <script src="../node_modules/vue/dist/vue.js"></script>
    
    <script>
        let app = new Vue({
            el: "#app",
            data: {
                name: "张三",
                num: 100
            },
            methods: {
                show() {
                    return this.name;
                },
                add() {
                    this.num++;
                }
            },
            beforeCreate() {
                console.log("=========beforeCreate=============");
                console.log("数据模型未加载：" + this.name, this.num);
                console.log("方法未加载：" + this.show());
                console.log("html模板未加载：" + document.getElementById("num"));
            },
            created: function () {
                console.log("=========created=============");
                console.log("数据模型已加载：" + this.name, this.num);
                console.log("方法已加载：" + this.show());
                console.log("html模板已加载：" + document.getElementById("num"));
                console.log("html模板未渲染：" + document.getElementById("num").innerText);
            },
            beforeMount() {
                console.log("=========beforeMount=============");
                console.log("html模板未渲染：" + document.getElementById("num").innerText);
            },
            mounted() {
                console.log("=========mounted=============");
                console.log("html模板已渲染：" + document.getElementById("num").innerText);
            },
            beforeUpdate() {
                console.log("=========beforeUpdate=============");
                console.log("数据模型已更新：" + this.num);
                console.log("html模板未更新：" + document.getElementById("num").innerText);
            },
            updated() {
                console.log("=========updated=============");
                console.log("数据模型已更新：" + this.num);
                console.log("html模板已更新：" + document.getElementById("num").innerText);
            }
        });
    </script>
</body>

</html>
```

## 4.8 vue模块化开发

1.全局安装webpack（<font color="red">mac下需要获取root权限 sudo -s，或者修改权限sudo chmod -R 777 /usr/local/lib/node_modules/</font>）

```shell
npm install webpack -g
```

2.全局安装vue脚手架

```shell
npm install -g @vue/cli-init
# mac下使用以下命令
npm install vue-cli -g
```

3.初始化vue项目，vue脚手架使用webpack模板初始化一个appname项目

```shell
vue init webpack appname
```

4.初始化过程会进行一系列的选择

<img src="/mall_images/image-20201212110731135.png" alt="image-20201212110731135" style="zoom:50%;" />

5.运行项目

```shell
npm run dev
```

## 4.9 整合ElementUI快速开发

网站地址

```java
https://element.eleme.cn/#/zh-CN
```

element安装

```shell
npm i element-ui -S
```

快速上手

```java
https://element.eleme.cn/#/zh-CN/component/quickstart
```

# 商品服务

## API

### 三级分类

#### 配置网关路由与路径重写

1.启动renren-fast后台项目

2.打开vscode启动前端项目renren-fast-vue，使用命令启动

```shell
npm run dev
```

3.登录人人后台

```java
http://localhost:8001/
账号 admin
密码 admin
```

#### 网关统一配置跨域

跨域：指的是浏览器不能执行其它网站的脚本。它是由浏览器的同源策略造成的，是<font color="red">浏览器对javascript施加的安全限制。</font>

同源策略：是指协议，域名，端口都要相同，其中有一个不同都会产生跨域；

<img src="/mall_images/image-20201213162729892.png" alt="image-20201213162729892" style="zoom:50%;" />



<img src="/mall_images/image-20201213163341187.png" alt="image-20201213163341187" style="zoom:50%;" />

```java
https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS
```

<img src="/mall_images/image-20201213164322090.png" alt="image-20201213164322090" style="zoom:50%;" />

<img src="/mall_images/image-20201213164553433.png" alt="image-20201213164553433" style="zoom:50%;" />

#### 删除-删除效果细化

vue发送GET和POST代码模板

```js
"http-get请求": {  
"prefix": "httpget",  
"body": [  
"this.\\$http({",  
"url: this.\\$http.adornUrl(''),",  
"method: 'get',",  
"params: this.\\$http.adornParams({})",  
"}).then(({data}) => {",  
"})"  
],  
"description": "httpGET请求"  
},  
"http-post请求": {  
"prefix": "httppost",  
"body": [  
"this.\\$http({",  
"url:this.\\$http.adornUrl(''),",  
"method: 'post',",  
"data: this.\\$http.adornData(data, false)",  
"}).then(({ data }) => { });"  
],  
"description": " httpPOST请求"  
}  
```

#### 新增-新增效果完成

![image-20210209165840650](/mall_images/image-20210209165840650.png)

<font color="gree">category.vue</font>

```vue
<template>
  <div>
    <el-tree
      :data="menus"
      :props="defaultProps"
      :expand-on-click-node="false"
      show-checkbox
      node-key="catId"
      :default-expanded-keys="expandedKey"
    >
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button
            v-if="node.level <= 2"
            type="text"
            size="mini"
            @click="() => append(data)"
          >
            Append
          </el-button>
          <el-button type="text" size="mini" @click="() => edit(data)">
            edit
          </el-button>
          <el-button
            v-if="node.childNodes == 0"
            type="text"
            size="mini"
            @click="() => remove(node, data)"
          >
            Delete
          </el-button>
        </span>
      </span>
    </el-tree>

    <el-dialog title="提示" :visible.sync="dialogVisible" width="30%">
      <el-form :model="category">
        <el-form-item label="分类名称">
          <el-input v-model="category.name" autocomplete="off"></el-input>
        </el-form-item>
      </el-form>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="addCategory">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  props: {},
  data() {
    return {
      category: {
        name: "",
        parentCid: 0,
        catLevel: 0,
        showStatus: 1,
        sort: 0,
        catId: null,
      },
      dialogVisible: false,
      menus: [],
      expandedKey: [],
      defaultProps: {
        children: "children",
        label: "name",
      },
    };
  },
  methods: {
    edit(data) {
      console.log("要修改的数据", data);
      this.dialogVisible = true;

      this.category.name = data.name;
      this.category.catId = data.catId
    },
    append(data) {
      console.log("append", data);
      this.dialogVisible = true;

      this.category.parentCid = data.catId;
      this.category.catLevel = data.catLevel * 1 + 1; // 防止是个字符串
    },
    // 添加三级分类
    addCategory() {
      console.log("提交的三级分类数据", this.category);
      this.$http({
        url: this.$http.adornUrl("/product/category/save"),
        method: "post",
        data: this.$http.adornData(this.category, false),
      }).then(({ data }) => {
        this.$message({
          message: "菜单保存成功",
          type: "success",
        });
        // 关闭对话框
        this.dialogVisible = false;
        // 刷新出新的菜单
        this.getMenus();
        // 设置需要默认展开的菜单
        this.expandedKey = [this.category.parentCid];
      });
    },
    remove(node, data) {
      console.log("remove", node, data);
      var ids = [data.catId];
      this.$confirm(`是否删除【${data.name}】菜单?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$http({
            url: this.$http.adornUrl("/product/category/delete"),
            method: "post",
            data: this.$http.adornData(ids, false),
          }).then(({ data }) => {
            this.$message({
              message: "菜单删除成功",
              type: "success",
            });
            // 刷新出新的菜单
            this.getMenus();
            // 设置需要默认展开的菜单
            this.expandedKey = [node.parent.data.catId];
          });
        })
        .catch(() => {});
    },
    getMenus() {
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get",
      }).then(({ data }) => {
        console.log("成功获取到菜单数据...", data.data);
        this.menus = data.data;
      });
    },
  },
  //计算属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {
    this.getMenus();
  },
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style scoped>
</style>
```

#### 修改-基本修改效果完成

<font color="gree">category.vue</font>

```vue
<template>
  <div>
    <el-tree
      :data="menus"
      :props="defaultProps"
      :expand-on-click-node="false"
      show-checkbox
      node-key="catId"
      :default-expanded-keys="expandedKey"
    >
      <span class="custom-tree-node" slot-scope="{ node, data }">
        <span>{{ node.label }}</span>
        <span>
          <el-button
            v-if="node.level <= 2"
            type="text"
            size="mini"
            @click="() => append(data)"
          >
            Append
          </el-button>
          <el-button type="text" size="mini" @click="() => edit(data)">
            edit
          </el-button>
          <el-button
            v-if="node.childNodes == 0"
            type="text"
            size="mini"
            @click="() => remove(node, data)"
          >
            Delete
          </el-button>
        </span>
      </span>
    </el-tree>

    <el-dialog
      :title="title"
      :visible.sync="dialogVisible"
      width="30%"
      :close-on-click-modal="false"
    >
      <el-form :model="category">
        <el-form-item label="分类名称">
          <el-input v-model="category.name" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="图标">
          <el-input v-model="category.icon" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="计量单位">
          <el-input
            v-model="category.productUnit"
            autocomplete="off"
          ></el-input>
        </el-form-item>
      </el-form>

      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitData">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  components: {},
  props: {},
  data() {
    return {
      title: "",
      dialogType: "", // edit,add
      category: {
        name: "",
        parentCid: 0,
        catLevel: 0,
        showStatus: 1,
        sort: 0,
        productUnit: "",
        icon: "",
        catId: null,
      },
      dialogVisible: false,
      menus: [],
      expandedKey: [],
      defaultProps: {
        children: "children",
        label: "name",
      },
    };
  },
  methods: {
    edit(data) {
      console.log("要修改的数据", data);
      this.dialogType = "edit";
      this.title = "修改分类";
      this.dialogVisible = true;

      // 发送请求获取当前节点最新的数据
      this.$http({
        url: this.$http.adornUrl(`/product/category/info/${data.catId}`),
        method: "get",
      }).then(({ data }) => {
        // 请求成功
        console.log("要回显的数据: ", data);
        this.category.name = data.data.name;
        this.category.catId = data.data.catId;
        this.category.icon = data.data.icon;
        this.category.productUnit = data.data.productUnit;
        this.category.parentCid = data.data.parentCid;
      });
    },
    append(data) {
      console.log("append", data);
      this.dialogType = "add";
      this.title = "添加分类";
      this.dialogVisible = true;
      this.category.parentCid = data.catId;
      this.category.catLevel = data.catLevel * 1 + 1; // 防止是个字符串

      this.category.name = "";
      this.category.catId = null;
      this.category.icon = "";
      this.category.productUnit = "";
      this.category.sort = 0;
      this.category.showStatus = 1;
    },
    submitData() {
      if (this.dialogType == "add") {
        this.addCategory();
      }
      if (this.dialogType == "edit") {
        this.editCategory();
      }
    },
    // 修改三级分类
    editCategory() {
      var { catId, name, icon, productUnit } = this.category; // {}结构表达式
      this.$http({
        url: this.$http.adornUrl("/product/category/update"),
        method: "post",
        data: this.$http.adornData({ catId, name, icon, productUnit }, false),
      }).then(({ data }) => {
        this.$message({
          message: "菜单修改成功",
          type: "success",
        });
        // 关闭对话框
        this.dialogVisible = false;
        // 刷新出新的菜单
        this.getMenus();
        // 设置需要默认展开的菜单
        this.expandedKey = [this.category.parentCid];
      });
    },
    // 添加三级分类
    addCategory() {
      console.log("提交的三级分类数据", this.category);
      this.$http({
        url: this.$http.adornUrl("/product/category/save"),
        method: "post",
        data: this.$http.adornData(this.category, false),
      }).then(({ data }) => {
        this.$message({
          message: "菜单保存成功",
          type: "success",
        });
        // 关闭对话框
        this.dialogVisible = false;
        // 刷新出新的菜单
        this.getMenus();
        // 设置需要默认展开的菜单
        this.expandedKey = [this.category.parentCid];
      });
    },
    remove(node, data) {
      console.log("remove", node, data);
      var ids = [data.catId];
      this.$confirm(`是否删除【${data.name}】菜单?`, "提示", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
        type: "warning",
      })
        .then(() => {
          this.$http({
            url: this.$http.adornUrl("/product/category/delete"),
            method: "post",
            data: this.$http.adornData(ids, false),
          }).then(({ data }) => {
            this.$message({
              message: "菜单删除成功",
              type: "success",
            });
            // 刷新出新的菜单
            this.getMenus();
            // 设置需要默认展开的菜单
            this.expandedKey = [node.parent.data.catId];
          });
        })
        .catch(() => {});
    },
    getMenus() {
      this.$http({
        url: this.$http.adornUrl("/product/category/list/tree"),
        method: "get",
      }).then(({ data }) => {
        console.log("成功获取到菜单数据...", data.data);
        this.menus = data.data;
      });
    },
  },
  //计算属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {
    this.getMenus();
  },
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style scoped>
</style>
```

#### 修改-拖拽效果

#### 修改-拖拽数据收集

#### 修改-拖拽功能完成

#### 修改-批量拖拽效果

#### 删除-批量删除&小结

### 品牌管理

#### 使用逆向工程的前后端代码

![image-20210209215315820](/mall_images/image-20210209215315820.png)

![image-20210210100828365](/mall_images/image-20210210100828365.png)

![image-20210210100902753](/mall_images/image-20210210100902753.png)

<font color="gree">brand.vue</font>

```vue
<template>
  <div class="mod-config">
    <el-form :inline="true" :model="dataForm" @keyup.enter.native="getDataList()">
      <el-form-item>
        <el-input v-model="dataForm.key" placeholder="参数名" clearable></el-input>
      </el-form-item>
      <el-form-item>
        <el-button @click="getDataList()">查询</el-button>
        <el-button v-if="isAuth('product:brand:save')" type="primary" @click="addOrUpdateHandle()">新增</el-button>
        <el-button v-if="isAuth('product:brand:delete')" type="danger" @click="deleteHandle()" :disabled="dataListSelections.length <= 0">批量删除</el-button>
      </el-form-item>
    </el-form>
    <el-table
      :data="dataList"
      border
      v-loading="dataListLoading"
      @selection-change="selectionChangeHandle"
      style="width: 100%;">
      <el-table-column
        type="selection"
        header-align="center"
        align="center"
        width="50">
      </el-table-column>
      <el-table-column
        prop="brandId"
        header-align="center"
        align="center"
        label="品牌id">
      </el-table-column>
      <el-table-column
        prop="name"
        header-align="center"
        align="center"
        label="品牌名">
      </el-table-column>
      <el-table-column
        prop="logo"
        header-align="center"
        align="center"
        label="品牌logo地址">
      </el-table-column>
      <el-table-column
        prop="descript"
        header-align="center"
        align="center"
        label="介绍">
      </el-table-column>
      <el-table-column
        prop="showStatus"
        header-align="center"
        align="center"
        label="显示状态[0-不显示；1-显示]">
      </el-table-column>
      <el-table-column
        prop="firstLetter"
        header-align="center"
        align="center"
        label="检索首字母">
      </el-table-column>
      <el-table-column
        prop="sort"
        header-align="center"
        align="center"
        label="排序">
      </el-table-column>
      <el-table-column
        fixed="right"
        header-align="center"
        align="center"
        width="150"
        label="操作">
        <template slot-scope="scope">
          <el-button type="text" size="small" @click="addOrUpdateHandle(scope.row.brandId)">修改</el-button>
          <el-button type="text" size="small" @click="deleteHandle(scope.row.brandId)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-pagination
      @size-change="sizeChangeHandle"
      @current-change="currentChangeHandle"
      :current-page="pageIndex"
      :page-sizes="[10, 20, 50, 100]"
      :page-size="pageSize"
      :total="totalPage"
      layout="total, sizes, prev, pager, next, jumper">
    </el-pagination>
    <!-- 弹窗, 新增 / 修改 -->
    <add-or-update v-if="addOrUpdateVisible" ref="addOrUpdate" @refreshDataList="getDataList"></add-or-update>
  </div>
</template>

<script>
  import AddOrUpdate from './brand-add-or-update'
  export default {
    data () {
      return {
        dataForm: {
          key: ''
        },
        dataList: [],
        pageIndex: 1,
        pageSize: 10,
        totalPage: 0,
        dataListLoading: false,
        dataListSelections: [],
        addOrUpdateVisible: false
      }
    },
    components: {
      AddOrUpdate
    },
    activated () {
      this.getDataList()
    },
    methods: {
      // 获取数据列表
      getDataList () {
        this.dataListLoading = true
        this.$http({
          url: this.$http.adornUrl('/product/brand/list'),
          method: 'get',
          params: this.$http.adornParams({
            'page': this.pageIndex,
            'limit': this.pageSize,
            'key': this.dataForm.key
          })
        }).then(({data}) => {
          if (data && data.code === 0) {
            this.dataList = data.page.list
            this.totalPage = data.page.totalCount
          } else {
            this.dataList = []
            this.totalPage = 0
          }
          this.dataListLoading = false
        })
      },
      // 每页数
      sizeChangeHandle (val) {
        this.pageSize = val
        this.pageIndex = 1
        this.getDataList()
      },
      // 当前页
      currentChangeHandle (val) {
        this.pageIndex = val
        this.getDataList()
      },
      // 多选
      selectionChangeHandle (val) {
        this.dataListSelections = val
      },
      // 新增 / 修改
      addOrUpdateHandle (id) {
        this.addOrUpdateVisible = true
        this.$nextTick(() => {
          this.$refs.addOrUpdate.init(id)
        })
      },
      // 删除
      deleteHandle (id) {
        var ids = id ? [id] : this.dataListSelections.map(item => {
          return item.brandId
        })
        this.$confirm(`确定对[id=${ids.join(',')}]进行[${id ? '删除' : '批量删除'}]操作?`, '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$http({
            url: this.$http.adornUrl('/product/brand/delete'),
            method: 'post',
            data: this.$http.adornData(ids, false)
          }).then(({data}) => {
            if (data && data.code === 0) {
              this.$message({
                message: '操作成功',
                type: 'success',
                duration: 1500,
                onClose: () => {
                  this.getDataList()
                }
              })
            } else {
              this.$message.error(data.msg)
            }
          })
        })
      }
    }
  }
</script>
```

<font color="gree">brand-add-or-update.vue</font>

```vue
<template>
  <el-dialog
    :title="!dataForm.id ? '新增' : '修改'"
    :close-on-click-modal="false"
    :visible.sync="visible">
    <el-form :model="dataForm" :rules="dataRule" ref="dataForm" @keyup.enter.native="dataFormSubmit()" label-width="80px">
    <el-form-item label="品牌名" prop="name">
      <el-input v-model="dataForm.name" placeholder="品牌名"></el-input>
    </el-form-item>
    <el-form-item label="品牌logo地址" prop="logo">
      <el-input v-model="dataForm.logo" placeholder="品牌logo地址"></el-input>
    </el-form-item>
    <el-form-item label="介绍" prop="descript">
      <el-input v-model="dataForm.descript" placeholder="介绍"></el-input>
    </el-form-item>
    <el-form-item label="显示状态[0-不显示；1-显示]" prop="showStatus">
      <el-input v-model="dataForm.showStatus" placeholder="显示状态[0-不显示；1-显示]"></el-input>
    </el-form-item>
    <el-form-item label="检索首字母" prop="firstLetter">
      <el-input v-model="dataForm.firstLetter" placeholder="检索首字母"></el-input>
    </el-form-item>
    <el-form-item label="排序" prop="sort">
      <el-input v-model="dataForm.sort" placeholder="排序"></el-input>
    </el-form-item>
    </el-form>
    <span slot="footer" class="dialog-footer">
      <el-button @click="visible = false">取消</el-button>
      <el-button type="primary" @click="dataFormSubmit()">确定</el-button>
    </span>
  </el-dialog>
</template>

<script>
  export default {
    data () {
      return {
        visible: false,
        dataForm: {
          brandId: 0,
          name: '',
          logo: '',
          descript: '',
          showStatus: '',
          firstLetter: '',
          sort: ''
        },
        dataRule: {
          name: [
            { required: true, message: '品牌名不能为空', trigger: 'blur' }
          ],
          logo: [
            { required: true, message: '品牌logo地址不能为空', trigger: 'blur' }
          ],
          descript: [
            { required: true, message: '介绍不能为空', trigger: 'blur' }
          ],
          showStatus: [
            { required: true, message: '显示状态[0-不显示；1-显示]不能为空', trigger: 'blur' }
          ],
          firstLetter: [
            { required: true, message: '检索首字母不能为空', trigger: 'blur' }
          ],
          sort: [
            { required: true, message: '排序不能为空', trigger: 'blur' }
          ]
        }
      }
    },
    methods: {
      init (id) {
        this.dataForm.brandId = id || 0
        this.visible = true
        this.$nextTick(() => {
          this.$refs['dataForm'].resetFields()
          if (this.dataForm.brandId) {
            this.$http({
              url: this.$http.adornUrl(`/product/brand/info/${this.dataForm.brandId}`),
              method: 'get',
              params: this.$http.adornParams()
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.dataForm.name = data.brand.name
                this.dataForm.logo = data.brand.logo
                this.dataForm.descript = data.brand.descript
                this.dataForm.showStatus = data.brand.showStatus
                this.dataForm.firstLetter = data.brand.firstLetter
                this.dataForm.sort = data.brand.sort
              }
            })
          }
        })
      },
      // 表单提交
      dataFormSubmit () {
        this.$refs['dataForm'].validate((valid) => {
          if (valid) {
            this.$http({
              url: this.$http.adornUrl(`/product/brand/${!this.dataForm.brandId ? 'save' : 'update'}`),
              method: 'post',
              data: this.$http.adornData({
                'brandId': this.dataForm.brandId || undefined,
                'name': this.dataForm.name,
                'logo': this.dataForm.logo,
                'descript': this.dataForm.descript,
                'showStatus': this.dataForm.showStatus,
                'firstLetter': this.dataForm.firstLetter,
                'sort': this.dataForm.sort
              })
            }).then(({data}) => {
              if (data && data.code === 0) {
                this.$message({
                  message: '操作成功',
                  type: 'success',
                  duration: 1500,
                  onClose: () => {
                    this.visible = false
                    this.$emit('refreshDataList')
                  }
                })
              } else {
                this.$message.error(data.msg)
              }
            })
          }
        })
      }
    }
  }
</script>
```

#### 效果优化与快速显示开关

#### 云存储开通与使用

#### OSS整合测试

#### OSS获取服务端签名

流程

![image-20210210113916547](/mall_images/image-20210210113916547.png)

文档参考地址

```java
https://help.aliyun.com/document_detail/31926.html?spm=a2c4g.11174283.6.1739.673c7da23lY0jH
```

<font color="gree">**xmall-third-party**</font>

<font color="gree">OssController.java</font>

```java
package com.lzd.xmall.thirdparty.controller;

import com.aliyun.oss.OSS;
import com.aliyun.oss.common.utils.BinaryUtil;
import com.aliyun.oss.model.MatchMode;
import com.aliyun.oss.model.PolicyConditions;
import com.lzd.common.utils.R;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.LinkedHashMap;
import java.util.Map;

@RestController
public class OssController {

    @Autowired
    OSS ossClient;
    @Value("${spring.cloud.alicloud.oss.endpoint}")
    private String endpoint;
    @Value("${spring.cloud.alicloud.oss.bucket}")
    private String bucket;
    @Value("${spring.cloud.alicloud.access-key}")
    private String accessId;

    @RequestMapping("/oss/policy")
    public R policy() {
//        String accessId = "<yourAccessKeyId>"; // 请填写您的AccessKeyId。
//        String accessKey = "<yourAccessKeySecret>"; // 请填写您的AccessKeySecret。
//        String endpoint = "oss-cn-hangzhou.aliyuncs.com"; // 请填写您的 endpoint。
        String bucket = "xmall-hello"; // 请填写您的 bucketname 。
        String host = "https://" + bucket + "." + endpoint; // host的格式为 bucketname.endpoint
        // callbackUrl为 上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
//        String callbackUrl = "http://88.88.88.88:8888";
        String format = new SimpleDateFormat("yyyy-MM-dd").format(new Date());
        String dir = format + "/"; // 用户上传文件时指定的前缀。

        Map<String, String> respMap = null;
        // 创建OSSClient实例。
        try {
            long expireTime = 30;
            long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
            Date expiration = new Date(expireEndTime);
            // PostObject请求最大可支持的文件大小为5 GB，即CONTENT_LENGTH_RANGE为5*1024*1024*1024。
            PolicyConditions policyConds = new PolicyConditions();
            policyConds.addConditionItem(PolicyConditions.COND_CONTENT_LENGTH_RANGE, 0, 1048576000);
            policyConds.addConditionItem(MatchMode.StartWith, PolicyConditions.COND_KEY, dir);

            String postPolicy = ossClient.generatePostPolicy(expiration, policyConds);
            byte[] binaryData = postPolicy.getBytes("utf-8");
            String encodedPolicy = BinaryUtil.toBase64String(binaryData);
            String postSignature = ossClient.calculatePostSignature(postPolicy);

            respMap = new LinkedHashMap<String, String>();
            respMap.put("accessid", accessId);
            respMap.put("policy", encodedPolicy);
            respMap.put("signature", postSignature);
            respMap.put("dir", dir);
            respMap.put("host", host);
            respMap.put("expire", String.valueOf(expireEndTime / 1000));
            // respMap.put("expire", formatISO8601Date(expiration));

            // 跨域在网关统一解决

        } catch (Exception e) {
            // Assert.fail(e.getMessage());
            System.out.println(e.getMessage());
        } finally {
            ossClient.shutdown();
        }
        return R.ok().put("data", respMap);
    }
}
```

<font color="gree">application.yml</font>

```yaml
spring:
  cloud:
    nacos:
      discovery:
        server-addr: 127.0.0.1:8848
    alicloud:
      access-key: 
      secret-key: 
      oss:
        endpoint: 
        bucket: xmall-hello
  application:
    name: xmall-third-party

server:
  port: 30000
```

<font color="gree">bootstrap.properties</font>

```properties
spring.application.name=xmall-third-party
spring.cloud.nacos.config.server-addr=127.0.0.1:8848
spring.cloud.nacos.config.namespace=72118ff9-72a6-4eb5-8b43-2c8c0ad133b7

spring.cloud.nacos.config.ext-config[0].data-id=oss.yml
spring.cloud.nacos.config.ext-config[0].group=DEFAULT_GROUP
spring.cloud.nacos.config.ext-config[0].refresh=true
```

<font color="gree">**xmall-gateway**</font>

<font color="gree">application.yml</font>

```yaml
spring:
  cloud:
    gateway:
      routes:
        - id: test_route
          uri: https://www.baidu.com
          predicates:
            - Query=url,baidu

        - id: qq_route
          uri: https://www.qq.com
          predicates:
            - Query=url,qq

        - id: product_route
          uri: lb://xmall-product
          predicates:
          - Path=/api/product/**
          filters:
          - RewritePath=/api/(?<segment>.*),/$\{segment}

        - id: third_party_route
          uri: lb://xmall-third-party
          predicates:
            - Path=/api/thirdparty/**
          filters:
            - RewritePath=/api/thirdparty/(?<segment>.*),/$\{segment}

        - id: admin_route
          uri: lb://renren-fast
          predicates:
            - Path=/api/**
          filters:
            - RewritePath=/api/(?<segment>.*),/renren-fast/$\{segment}

        - id: xmall_host_route
          uri: lb://xmall-product
          predicates:
          - Host=**.xmall.com

## 前端项目，/api
## http://localhost:9999/api/captcha.jpg  http://localhost:8080/api/captcha.jpg
## http://localhost:9999/api/product/category/list/tree http://localhost:8080/product/category/list/tree
```

访问地址

```java
http://localhost:9999/api/thirdparty/oss/policy
```



#### OSS前后联调测试上传

![image-20210210184142874](/mall_images/image-20210210184142874.png)

<font color="gree">**renren-fast-vue**</font>

<font color="gree">singleUpload.vue</font>

```vue
<template>
   
  <div>
    <el-upload
      action="http://xmall-hello.oss-cn-shenzhen.aliyuncs.com"
      :data="dataObj"
      list-type="picture"
      :multiple="false"
      :show-file-list="showFileList"
      :file-list="fileList"
      :before-upload="beforeUpload"
      :on-remove="handleRemove"
      :on-success="handleUploadSuccess"
      :on-preview="handlePreview"
    >
      <el-button size="small" type="primary">点击上传</el-button>
      <div slot="tip" class="el-upload__tip">
        只能上传jpg/png文件，且不超过10MB
      </div>
    </el-upload>
    <el-dialog :visible.sync="dialogVisible">
      <img width="100%" :src="fileList[0].url" alt="" />
    </el-dialog>
  </div>
</template>
<script>
import { policy } from "./policy";
import { getUUID } from "@/utils";

export default {
  name: "singleUpload",
  props: {
    value: String,
  },
  computed: {
    imageUrl() {
      return this.value;
    },
    imageName() {
      if (this.value != null && this.value !== "") {
        return this.value.substr(this.value.lastIndexOf("/") + 1);
      } else {
        return null;
      }
    },
    fileList() {
      return [
        {
          name: this.imageName,
          url: this.imageUrl,
        },
      ];
    },
    showFileList: {
      get: function () {
        return (
          this.value !== null && this.value !== "" && this.value !== undefined
        );
      },
      set: function (newValue) {},
    },
  },
  data() {
    return {
      dataObj: {
        policy: "",
        signature: "",
        key: "",
        ossaccessKeyId: "",
        dir: "",
        host: "",
        // callback:'',
      },
      dialogVisible: false,
    };
  },
  methods: {
    emitInput(val) {
      this.$emit("input", val);
    },
    handleRemove(file, fileList) {
      this.emitInput("");
    },
    handlePreview(file) {
      this.dialogVisible = true;
    },
    beforeUpload(file) {
      let _self = this;
      return new Promise((resolve, reject) => {
        policy()
          .then((response) => {
            console.log("响应的数据", response);
            _self.dataObj.policy = response.data.policy;
            _self.dataObj.signature = response.data.signature;
            _self.dataObj.ossaccessKeyId = response.data.accessid;
            _self.dataObj.key = response.data.dir + getUUID() + "_${filename}";
            _self.dataObj.dir = response.data.dir;
            _self.dataObj.host = response.data.host;
            console.log("响应的数据2", _self.dataObj);
            resolve(true);
          })
          .catch((err) => {
            reject(false);
          });
      });
    },
    handleUploadSuccess(res, file) {
      console.log("上传成功...");
      this.showFileList = true;
      this.fileList.pop();
      this.fileList.push({
        name: file.name,
        url:
          this.dataObj.host +
          "/" +
          this.dataObj.key.replace("${filename}", file.name),
      });
      this.emitInput(this.fileList[0].url);
    },
  },
};
</script>
<style>
</style>
```

```js
import http from '@/utils/httpRequest.js'
export function policy() {
   return  new Promise((resolve,reject)=>{
        http({
            url: http.adornUrl("/thirdparty/oss/policy"),
            method: "get",
            params: http.adornParams({})
        }).then(({ data }) => {
            resolve(data);
        })
    });
}
```

解决跨域问题

参考地址

```java
https://help.aliyun.com/document_detail/91868.html?spm=a2c4g.11186623.2.10.4c397d9cvR8EXV
```

![image-20210210184334345](/mall_images/image-20210210184334345.png)

#### 表单校验&自定义校验器

#### JSR303数据校验

1.给Bean添加校验注解:javax.validation.constraints, 并定义自己的message提示

<font color="gree">BrandEntity.java</font>

```java
/**
	 * 品牌名
	 */
	@NotBlank(message = "品牌名必须提交")
	private String name;
```

2.开启校验注解@Valid

效果：校验错误以后会有默认的响应。

<font color="gree">BrandController.java</font>

```java
 /**
     * 保存
     */
    @RequestMapping("/save")
    //@RequiresPermissions("product:brand:save")
    public R save(@Valid @RequestBody BrandEntity brand){
		brandService.save(brand);

        return R.ok();
    }
```

3.给校验的bean后紧跟一个BindingResult，就可以获取到校验的结果

<font color="gree">BrandController.java</font>

```java
/**
     * 保存
     */
    @RequestMapping("/save")
    //@RequiresPermissions("product:brand:save")
    public R save(@Valid @RequestBody BrandEntity brand, BindingResult result) {
        if (result.hasErrors()) {
            Map<String, String> map = new HashMap<>();
            // 1.获取校验的错误结果
            result.getFieldErrors().forEach((item) -> {
                // FieldError 获取到错误提示
                String message = item.getDefaultMessage();
                // 获取错误的属性的名字
                String field = item.getField();
                map.put(field, message);
            });

            return R.error(400, "提交的数据不合法").put("data", map);
        } else {
            brandService.save(brand);
        }

        return R.ok();
    }
```

4.分组校验

给校验注解标注什么情况需要进行校验

```java
@NotBlank(message = "品牌名必须提交", groups = {AddGroup.class, UpdateGroup.class})
```

在具体的业务逻辑上指定校验组

```java
public R save(@Validated({AddGroup.class}) @RequestBody BrandEntity brand/*, BindingResult result*/) 
```

最后，基本的校验做完

<font color="gree">BrandEntity.java</font>

```java
package com.lzd.xmall.product.entity;

import com.baomidou.mybatisplus.annotation.TableId;
import com.baomidou.mybatisplus.annotation.TableName;
import com.lzd.common.valid.AddGroup;
import com.lzd.common.valid.ListValue;
import com.lzd.common.valid.UpdateGroup;
import com.lzd.common.valid.UpdateStatusGroup;
import lombok.Data;
import org.hibernate.validator.constraints.URL;

import javax.validation.constraints.*;
import java.io.Serializable;

/**
 * 品牌
 *
 * @author liuzhidong
 * @email liuzhidong2016@gmail.com
 * @date 2020-10-11 17:35:27
 */
@Data
@TableName("pms_brand")
public class BrandEntity implements Serializable {
    private static final long serialVersionUID = 1L;

    /**
     * 品牌id
     */
    @NotNull(message = "修改必须指定品牌id", groups = {UpdateGroup.class})
    @Null(message = "新增不能指定id", groups = {AddGroup.class})
    @TableId
    private Long brandId;
    /**
     * 品牌名
     */
    @NotBlank(message = "品牌名必须提交", groups = {AddGroup.class, UpdateGroup.class})
    private String name;
    /**
     * 品牌logo地址
     */
    @NotBlank(groups = {AddGroup.class})
    @URL(message = "logo必须是一个合法的url地址", groups = {AddGroup.class, UpdateGroup.class})
    private String logo;
    /**
     * 介绍
     */
    private String descript;
    /**
     * 显示状态[0-不显示；1-显示]
     */
    @NotNull(groups = {AddGroup.class, UpdateStatusGroup.class})
    @ListValue(vals = {0, 1}, groups = {AddGroup.class, UpdateStatusGroup.class})
    private Integer showStatus;
    /**
     * 检索首字母
     */
    @NotEmpty(groups = {AddGroup.class})
    @Pattern(regexp = "^[a-zA-Z]$", message = "检索首字母必须是一个字母", groups = {AddGroup.class, UpdateGroup.class})
    private String firstLetter;
    /**
     * 排序
     */
    @NotNull(groups = {AddGroup.class})
    @Min(value = 0, message = "排序必须大于等于0", groups = {AddGroup.class, UpdateGroup.class})
    private Integer sort;

}
```

#### 统一异常处理

@ControllerAdvice

1.编写异常处理类，使用@ControllerAdvice

2.使用@ExceptionHandler标注方法可以处理的异常

![image-20210213125053401](/mall_images/image-20210213125053401.png)

<font color="gree">XmallExceptionControllerAdvice.java</font>

```java
package com.lzd.xmall.product.exception;

import com.lzd.common.exception.BizCodeEnum;
import com.lzd.common.utils.R;
import lombok.extern.slf4j.Slf4j;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

import java.util.HashMap;
import java.util.Map;

/**
 * 集中处理所有异常
 */
@Slf4j
@RestControllerAdvice(basePackages = "com.lzd.xmall.product.controller")
public class XmallExceptionControllerAdvice {

    @ExceptionHandler(value = MethodArgumentNotValidException.class)
    public R handleValidException(MethodArgumentNotValidException e) {
        log.error("数据校验出现问题{}, 异常类型: {}", e.getMessage(), e.getClass());
        BindingResult bindingResult = e.getBindingResult();

        Map<String, String> errorMap = new HashMap<>();
        bindingResult.getFieldErrors().forEach((fieldError) -> {
            errorMap.put(fieldError.getField(), fieldError.getDefaultMessage());
        });
        return R.error(BizCodeEnum.VALID_EXCEPTION.getCode(), BizCodeEnum.VALID_EXCEPTION.getMsg()).put("data", errorMap);
    }

    @ExceptionHandler(value = Throwable.class)
    public R handleException(Throwable throwable) {

        return R.error(BizCodeEnum.UNKNOW_EXEPTION.getCode(), BizCodeEnum.UNKNOW_EXEPTION.getMsg());
    }
}
```

#### JSR303分组校验（多场景的复杂校验）

参考JSR303数据校验之分组校验

默认没有指定分组的校验注解@NotBlank，在分组校验情况@Validated({AddGroup.class})不生效，只会在@Validated不指定分组情况下生效。

#### JSR303自定义校验注解

1.自己编写一个自定义校验注解@ListValue

2.编写一个自定义的校验器ConstraintValidator

3.关联自定义校验器和自定义校验注解

<font color="gree">**xmall-common**</font>

<font color="gree">ListValue.java</font>

```java
package com.lzd.common.valid;

import javax.validation.Constraint;
import javax.validation.Payload;
import java.lang.annotation.*;


@Documented
@Constraint(
        validatedBy = {ListValueConstraintValidator.class【可以指定多个不同的校验器，适配不同类型的校验】}
)
@Target({ElementType.METHOD, ElementType.FIELD, ElementType.ANNOTATION_TYPE, ElementType.CONSTRUCTOR, ElementType.PARAMETER, ElementType.TYPE_USE})
@Retention(RetentionPolicy.RUNTIME)
public @interface ListValue {

    String message() default "{com.lzd.common.valid.ListValue.message}";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};

    int[] vals() default {};
}
```

<font color="gree">ListValueConstraintValidator.java</font>

```java
package com.lzd.common.valid;

import javax.validation.ConstraintValidator;
import javax.validation.ConstraintValidatorContext;
import java.util.HashSet;
import java.util.Set;

public class ListValueConstraintValidator implements ConstraintValidator<ListValue, Integer> {

    private Set<Integer> set = new HashSet<>();

    @Override
    public void initialize(ListValue constraintAnnotation) {

        int[] vals = constraintAnnotation.vals();
        for (int val : vals) {
            set.add(val);
        }
    }

    // 判断是否校验成功

    /**
     * @param integer                    // 需要校验的值
     * @param constraintValidatorContext
     * @return
     */
    @Override
    public boolean isValid(Integer integer, ConstraintValidatorContext constraintValidatorContext) {
        return set.contains(integer);
    }
}
```

### 概念-SPU&SKU&规格参数&销售属性

<font color="gree">SPU&SKU</font>

![image-20210214103258255](/mall_images/image-20210214103258255.png)

![image-20210214115920074](/mall_images/image-20210214115920074.png)

![image-20210609222101374](/mall_images/image-20210609222101374.png)

![image-20210609232727866](/mall_images/image-20210609232727866.png)

![image-20210609233536299](/mall_images/image-20210609233536299.png)



### 属性分组

#### 前端组件抽取&父子组件交互

# 性能与压力测试

![image-20210131081331603](/mall_images/image-20210131081331603.png)

![image-20210131082201219](/mall_images/image-20210131082201219.png)





# 缓存

![image-20210202074332999](/mall_images/image-20210202074332999.png)

## 整合redis

1、引入data-redis-starter

2、简单配置redis的host等信息

3、使用SpringBoot自动配置好的StringRedisTemplate来操作redis

## 压力测试出的内存泄漏及解决

1、SpringBoot2.0以后默认使用lettuce作为操作redis的客户端。它使用netty进行网络通信。

2、lettuce的bug导致netty堆外内存溢出，-Xmx300m；netty如果没有指定堆外内存，默认使用-Xmx300m，可以通过

-Dio.netty.maxDirectMemory进行设置。

<font color="red">解决方案：不能使用-Dio.netty.maxDirectMemory只去调大堆外内存。</font>

<font color="red">1、升级lettuce客户端。</font>

<font color="red">2、切换使用jedis。</font>

lettuce、jedis操作redis的底层客户端。Spring再次封装redisTemplate

## 缓存穿透

![image-20210204081358437](/mall_images/image-20210204081358437.png)

## 缓存雪崩

![image-20210204081627216](/mall_images/image-20210204081627216.png)

## 缓存击穿

![image-20210204081828985](/mall_images/image-20210204081828985.png)

### 加锁解决缓存击穿问题

<font color="red">在分布式情况下，想要锁住所有，必须使用分布式锁，加锁和解锁都需要原子操作</font>

## 分布式锁-Redisson

### lock看门狗原理-redisson如何解决死锁问题

加锁

```java
lock.lock(); // 阻塞式等待。默认加的锁都是30s时间。
```

1. 锁的自动续期，如果业务超长，运行期间自动给锁续上新的30s。不用担心业务时间长，锁自动过期被删掉。
2. 加锁的业务只要运行完成，就不会给当前锁续期，即使不手动解锁，锁默认在30s以后自动删除。

```java
lock.lock(10, TimeUnit.SECONDS); // 10秒自动解锁，自动解锁时间一定要大于业务的执行时间
```

问题：lock.lock(10, TimeUnit.SECONDS); 在锁时间到了以后，不会自动续期。

1. 如果我们传递了锁的超时时间，就发送给redis执行脚本，进行占锁，默认超时就是我们指定的时间。
2. 如果我们未指定锁的超时时间，就使用30*1000【LockWatchdogTimeout看门狗的默认时间】
3. 只要占锁成功，就会启动一个定时任务【重新给锁设置过期时间，新的过期时间就是看门狗的默认时间】，每隔10s都会自动再次续期，internalLockLeaseTime【看门狗时间】/ 3, 10s

<font color="red">最佳实践: lock.lock(30, TimeUnit.SECONDS); 省掉了续期操作。</font>

# 商城业务

## 商品上架

### 129、商城业务-商品上架-nested数据类型场景

![image-20210523122359160](/mall_images/image-20210523122359160.png)



## 检索服务

### 搭建页面环境

## 异步

### 异步复习

![image-20210215102909493](/mall_images/image-20210215102909493.png)

<font color="red">**在以后的业务代码里，前三种方式都不用。【将所有的多线程异步任务都交给线程池执行】,保证当前系统中池只有一个。每个异步任务，直接提交给线程池让它自己去执行就行了。**</font>

<font color="gree">**xmall-search**</font>

<font color="gree">ThreadTest.java</font>

```java
package com.lzd.xmall.search.thread;

import java.util.concurrent.*;

public class ThreadTest {

    public static ExecutorService executorService = Executors.newFixedThreadPool(10);

    public static void main(String[] args) throws ExecutionException, InterruptedException {

        System.out.println("main start...");

        // 1.第一种方式
//        new Thread01().start();

        // 2.第二种方式
//        new Thread(new Runable01()).start();

        // 3.第三种方式
//        FutureTask<Integer> futureTask = new FutureTask<>(new Callable01());
//        new Thread(futureTask).start();
//        Integer i = futureTask.get(); // 阻塞
//        System.out.println("返回的结果：" + i);

        // 4.线程池 给线程池直接提交任务 可以达到控制资源的效果 能保持性能稳定
        executorService.execute(new Runable01());

        System.out.println("main end...");
    }

    public static class Callable01 implements Callable<Integer> {

        @Override
        public Integer call() throws Exception {
            System.out.println("当前线程：" + Thread.currentThread().getId());
            int i = 10 / 2;
            System.out.println("运行结果：" + i);
            return i;
        }
    }

    public static class Runable01 implements Runnable {

        @Override
        public void run() {
            System.out.println("当前线程：" + Thread.currentThread().getId());
            int i = 10 / 2;
            System.out.println("运行结果：" + i);
        }
    }

    public static class Thread01 extends Thread {

        @Override
        public void run() {
            System.out.println("当前线程：" + Thread.currentThread().getId());
            int i = 10 / 2;
            System.out.println("运行结果：" + i);
        }
    }
}
```

### 线程池讲解

```java
// 原生创建线程池
        /**
         * 七大参数
         * corePoolSize: 核心线程数(一直存在除非(allowCoreThreadTimeOut))，线程池创建好以后就准备就绪的线程数量，就等待来接收异步任务去执行
         * maximumPoolSize: 最大线程数量，控制资源
         * keepAliveTime: 存活时间，如果线程数量大于核心数量，释放空闲的线程（maximumPoolSize-corePoolSize）
         * TimeUnit: 时间单位
         * BlockingQueue<Runnable>: 阻塞队列，如果任务有很多，就会将多的任务放到队列里面。只要有线程空闲，就会去队列里面取出新的任务继续执行
         * ThreadFactory: 线程的创建工厂
         * RejectedExecutionHandler: 如果队列满了，按照指定的拒绝策略拒绝执行任务
         *
         * 工作顺序：
         * 1.线程池创建了，准备好core数量的核心线程，准备接受任务
         * 2.core满了，就将再进来的任务放入阻塞组队中。空闲的core就会自己去阻塞队列中获取任务执行
         * 3.阻塞队列满了，就直接开新线程执行，最大只能开到max指定数量
         * 4.max满了就用拒绝策略拒绝任务
         * 5.max没满，执行完后，在指定的时间KeepAliveTime以后，释放max-core线程
         *
         */
        ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(5,
                200,
                10,
                TimeUnit.SECONDS, // 默认是Integer的最大值
                new LinkedBlockingQueue<>(100000),
                Executors.defaultThreadFactory(),
                new ThreadPoolExecutor.AbortPolicy());
```

![image-20210215162109776](/mall_images/image-20210215162109776.png)

### CompletableFuture异步编排

A, B, C三个任务，C要依赖于A的结果，这就要用到异步编排了。

<font color="red">这是JDK1.8以后才提供的功能</font>

### CompletableFuture-启动异步

```java
public static void main(String[] args) throws ExecutionException, InterruptedException {

        System.out.println("main start...");

//        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
//            System.out.println("当前线程：" + Thread.currentThread().getId());
//            int i = 10 / 2;
//            System.out.println("运行结果：" + i);
//        }, executorService);
        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
            System.out.println("当前线程：" + Thread.currentThread().getId());
            int i = 10 / 2;
            System.out.println("运行结果：" + i);
            return i;
        }, excutor);
        System.out.println(future.get());

        System.out.println("main end...");
    }
```

### 完成回调与异常感知

![image-20210215190030194](/mall_images/image-20210215190030194.png)

```java
public static void main(String[] args) throws ExecutionException, InterruptedException {

        System.out.println("main start...");

//        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
//            System.out.println("当前线程：" + Thread.currentThread().getId());
//            int i = 10 / 2;
//            System.out.println("运行结果：" + i);
//        }, executorService);
        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
            System.out.println("当前线程：" + Thread.currentThread().getId());
            int i = 10 / 0;
            System.out.println("运行结果：" + i);
            return i;
        }, excutor).whenComplete((result, exception) -> {
            System.out.println("异步任务成功完成了...结果是：" + result + "；异常是：" + exception);
        }).exceptionally(throwable -> {
            // 可以感知异常，同时返回默认值
            return 10;
        });
        System.out.println(future.get());

        System.out.println("main end...");
    }
```

### handle最终处理

```java
public static void main(String[] args) throws ExecutionException, InterruptedException {

        System.out.println("main start...");

//        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
//            System.out.println("当前线程：" + Thread.currentThread().getId());
//            int i = 10 / 2;
//            System.out.println("运行结果：" + i);
//        }, executorService);
        // 方法完成后的感知
//        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
//            System.out.println("当前线程：" + Thread.currentThread().getId());
//            int i = 10 / 0;
//            System.out.println("运行结果：" + i);
//            return i;
//        }, excutor).whenComplete((result, exception) -> {
//            System.out.println("异步任务成功完成了...结果是：" + result + "；异常是：" + exception);
//        }).exceptionally(throwable -> {
//            // 可以感知异常，同时返回默认值
//            return 10;
//        });

        // 方法执行完成后的处理
        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
            System.out.println("当前线程：" + Thread.currentThread().getId());
            int i = 10 / 2;
            System.out.println("运行结果：" + i);
            return i;
        }, excutor).handle((result, exception) -> {
            if (result != null) {
                return result * 2;
            }
            if (exception != null) {
                return 0;
            }
            return 0;
        });

        System.out.println(future.get());

        System.out.println("main end...");
    }
```

### 线程串行化

![image-20210215202048641](/mall_images/image-20210215202048641.png)

```java
public static void main(String[] args) throws ExecutionException, InterruptedException {

        System.out.println("main start...");
//        CompletableFuture<Void> future = CompletableFuture.supplyAsync(() -> {
//            System.out.println("当前线程：" + Thread.currentThread().getId());
//            int i = 10 / 2;
//            System.out.println("运行结果：" + i);
//            return i;
//        }, excutor).thenRunAsync(() -> { // 不能获取到上一步的结果
//            System.out.println("任务2启动了...");
//        }, excutor);

//        CompletableFuture<Void> future = CompletableFuture.supplyAsync(() -> {
//            System.out.println("当前线程：" + Thread.currentThread().getId());
//            int i = 10 / 2;
//            System.out.println("运行结果：" + i);
//            return i;
//        }, excutor).thenAcceptAsync(result -> { // 能接受上一步结果，但是无返回值
//            System.out.println("任务2启动了..." + result);
//        }, excutor);

        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            System.out.println("当前线程：" + Thread.currentThread().getId());
            int i = 10 / 2;
            System.out.println("运行结果：" + i);
            return i;
        }, excutor).thenApplyAsync(result -> { // 能接受上一步结果并且有返回值
            System.out.println("任务2启动了..." + result);
            return "Hello" + result;
        }, excutor);

        System.out.println(future.get());

        System.out.println("main end...");
    }
```

### 两任务组合-都要完成

![image-20210216094204321](/mall_images/image-20210216094204321.png)

![image-20210216094002583](/mall_images/image-20210216094002583.png)

![image-20210216094435445](/mall_images/image-20210216094435445.png)

```java
CompletableFuture<Integer> future01 = CompletableFuture.supplyAsync(() -> {
            System.out.println("任务1线程开始：" + Thread.currentThread().getId());
            int i = 10 / 2;
            System.out.println("任务1结束：");
            return i;
        }, excutor);

        CompletableFuture<String> future02 = CompletableFuture.supplyAsync(() -> {
            System.out.println("任务2线程开始：" + Thread.currentThread().getId());
            System.out.println("任务2结束：");
            return "Hello";
        }, excutor);

//        future01.runAfterBothAsync(future02, () -> { // 不能感知到结果
////            System.out.println("任务3开始...");
////        }, excutor);

//        future01.thenAcceptBothAsync(future02, (f1, f2) -> {
//            System.out.println("任务3开始...之前的结果: " + f1 + "=>" + f2);
//        }, excutor);

        CompletableFuture<String> future = future01.thenCombineAsync(future02, (f1, f2) -> {
            return f1 + ": " + f2 + " Wolrd";
        }, excutor);

        System.out.println(future.get());
```

### 两任务组合-一个完成

![image-20210216102146205](/mall_images/image-20210216102146205.png)

![image-20210216100906793](/mall_images/image-20210216100906793.png)

### 多任务组合

![image-20210216104106449](/mall_images/image-20210216104106449.png)

```java
 CompletableFuture<String> futureImg = CompletableFuture.supplyAsync(() -> {
            System.out.println("查询商品图片信息...");
            return "hello.jpg";
        }, excutor);

        CompletableFuture<String> futureAttr = CompletableFuture.supplyAsync(() -> {
            System.out.println("查询商品属性...");
            return "黑色+256G";
        }, excutor);

        CompletableFuture<String> futureDesc = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(3000);
                System.out.println("查询商品介绍...");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "华为";
        }, excutor);

//        CompletableFuture<Void> allOf = CompletableFuture.allOf(futureImg, futureAttr, futureDesc);
//        allOf.get(); // 等待所有结果完成

        CompletableFuture<Object> anyOf = CompletableFuture.anyOf(futureImg, futureAttr, futureDesc);
        anyOf.get();

//        System.out.println(futureImg.get() + "=>" + futureAttr.get() + "=>" + futureDesc.get());
        System.out.println(anyOf.get());

        System.out.println("main end...");
```

## 商品详情

### 环境搭建

使用SwitchHosts软件进行域名设置

![image-20210519222245577](/mall_images/image-20210519222245577.png)

修改nginx配置/mydata/nginx/conf/conf.d/xmall.conf，匹配上item.xmall.com

![image-20210519223155602](/mall_images/image-20210519223155602.png)

修改网关配置

![image-20210519224010763](/mall_images/image-20210519224010763.png)

把商品详情页复制到项目里，修改名为item.html

![image-20210519230021144](/mall_images/image-20210519230021144.png)

![image-20210519230036682](/mall_images/image-20210519230036682.png)

把静态资源放入nginx里

![image-20210519230542626](/mall_images/image-20210519230542626.png)

![image-20210519230558929](/mall_images/image-20210519230558929.png)

修改item.html文件的访问路径

![image-20210519232635475](/mall_images/image-20210519232635475.png)

![image-20210519233619220](/mall_images/image-20210519233619220.png)

修改nginx配置

![image-20210519233040105](/mall_images/image-20210519233040105.png)

## 认证服务

### 单点登录

![image-20210725204954694](/mall_images/image-20210725204954694.png)

![image-20210725214351003](/mall_images/image-20210725214351003.png)

![image-20210726233542216](/mall_images/image-20210726233542216.png)

![image-20210726234700045](/mall_images/image-20210726234700045.png)

![image-20210726235531706](/mall_images/image-20210726235531706.png)

### 核心

<font color=red>1.给登录服务器留下登录痕迹</font>

<font color=red>2.登录服务器要将token信息重定向的时候，带到url地址上</font>

<font color=red>3.其它系统要处理url地址上的关键token，只要有，将token保存到自己的session中</font>

<font color=red>4.自己系统将用户保存在自己的会话中</font>

## 购物车

### 购物车数据结构

![image-20210728220853891](/mall_images/image-20210728220853891.png)



## 分布式事务

@Transactional属于本地事务，在分布式系统中，只能控制自己的回滚，控制不了其它服务的回滚

<font color=red>分布式事务：最大原因：网络问题+分布式机器</font>

## 订单业务

### 创建业务交换机&队列

![image-20210828152636231](/mall_images/image-20210828152636231.png)

xmall-ware MyRabbitConfig.java

```java
package com.lzd.xmall.ware.config;

import org.springframework.amqp.core.*;
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.HashMap;

@Configuration
public class MyRabbitConfig {

    @Bean
    public MessageConverter messageConverter() {
        return new Jackson2JsonMessageConverter();
    }

    @RabbitListener(queues = "stock.release.stock.queue")
    public void handler(Message message) {

    }

    @Bean
    public Exchange stockEventExchange() {
        return new TopicExchange("stock-event-exchange", true, false);
    }

    /**
     * 延迟队列
     *
     * @return
     */
    @Bean
    public Queue stockDelayQueue() {
        HashMap<String, Object> arguments = new HashMap<>();
        arguments.put("x-dead-letter-exchange", "stock-event-exchange");
        arguments.put("x-dead-letter-routing-key", "stock.release");
        // 消息过期时间 2分钟
        arguments.put("x-message-ttl", 120000);
        return new Queue("stock.delay.queue", true, false, false, arguments);
    }

    /**
     * 普通队列，用于解锁库存
     *
     * @return
     */
    @Bean
    public Queue stockReleaseStockQueue() {
        return new Queue("stock.release.stock.queue", true, false, false, null);
    }

    /**
     * 交换机和延迟队列绑定
     *
     * @return
     */
    @Bean
    public Binding stockLockedBinding() {
        return new Binding("stock.delay.queue",
                Binding.DestinationType.QUEUE,
                "stock-event-exchange",
                "stock.locked",
                null);
    }

    /**
     * 交换机和普通队列绑定
     *
     * @return
     */
    @Bean
    public Binding stockReleaseBinding() {
        return new Binding("stock.release.stock.queue",
                Binding.DestinationType.QUEUE,
                "stock-event-exchange",
                "stock.release.#",
                null);
    }
}
```

### 定时关单完成

![image-20210910223938295](/mall_images/image-20210910223938295.png)

![image-20210911110333350](/mall_images/image-20210911110333350.png)

![image-20210911110308062](/mall_images/image-20210911110308062.png)

### 消息积压、积压、重复等解决方案

![image-20210911182716763](/mall_images/image-20210911182716763.png)

![image-20210911184854270](/mall_images/image-20210911184854270.png)

![image-20210911184915396](/mall_images/image-20210911184915396.png)

![image-20210911185557457](/mall_images/image-20210911185557457.png)



## 支付

1.进入“蚂蚁金服开放平台”

```java
https://open.alipay.com/platform/home.htm
```

![image-20210913224837549](/mall_images/image-20210913224837549.png)

## 秒杀

![image-20210925164852318](/mall_images/image-20210925164852318.png)

面对超高并发

![image-20210930205516149](/mall_images/image-20210930205516149.png)







# 附录：安装Nginx

随便启动一个nginx实例，只是为了复制出配置

```shell
docker run -p80:80 --name nginx -d nginx:1.10
```

将容器内的配置文件拷贝到当前目录

```shell
docker container cp nginx:/etc/nginx .
```

停止原容器

```shell
docker stop nginx
```

执行命令删除原容器

```shell
docker rm nginx
```

创建新的Nginx，执行以下命令

```shell
docker run -p 80:80 --name nginx \
 -v /mydata/nginx/html:/usr/share/nginx/html \
 -v /mydata/nginx/logs:/var/log/nginx \
 -v /mydata/nginx/conf/:/etc/nginx \
 -d nginx:1.10
```









