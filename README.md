
<style>

    .success {
        padding:5px;
        display:inline;
        color:#1B5E20;
        background-color:#C8E6C9;
    }
    .warning {
        padding:5px;
        display:inline;
        color:#E65100;
        background-color:#FFE0B2;
        width:100%;
    }
    .danger {
        padding:5px;
        display:inline;
        color:#B71C1C;
        background-color:#FFCDD2;
    }
    .info {
        padding:5px;
        display:inline;
        color:#006064;
        background-color:#B2EBF2;
    }
    .doubt {
        padding:5px;
        display:inline;
        color:#AAA;
        background-color:#DDDDDD;
    }
    .asso {
        padding:5px;
        display:inline;
        color:#555;
        background-color:#FFCC00;        
    }
    
    .alert {
        display:inline-block;
        width:100%;
        padding:5px;
        line-height:30px;
        margin-top:10px;
    }
</style>

# Learn-Koa

## cookie
##### 设置cookie 实现二级域名共享cookie
##### 设置path 指定文件路径才可访问cookie
##### 设置httpOnly true表示限定cookie在后端环境访问，可以防止浏览器端的JavaScript脚本通过document.cookie等方式访问Cookie
itying.com 是一级域名
www.itying.com 这是二级域名
mail.itying.com 这是二级域名
mail.itying.com/news /news是文件路径
**domain默认值是当前页面的二级域名**
```js 
ctx.cookies.set("userinfo",12345,{
    maxAge:1000*10*30.
    domain:".itying.com", //指定一级域名，二级域名可共享cookies
    path:"/news", //同域名下只有/new路径可以访问cookies
    httpOnly:true //限定只能在后端服务器访问cookies,不允许前端js脚本访问
})
```
##### 中文域名
<b class="danger">cookies的值无法设置中文字符串</b>

```js 
let base64str = Buffer.from('这是一段中文字符串').toString('base64'); //中文字符串编码成base64
let str = Buffer.from(base64str,'base64').toString() //还原成中文字符串

// 通用cookie设置方法，使用buffer兼容中英文字符串
ctx.cookies.set(Buffer.from('abcder').toString('base64'))
```


## 常用中间件
#### 1. koa-bodyparser
解析HTTP请求的body部分，将解析后的数据存储在ctx.request.body中

```js 
const Koa = require('koa');
const bodyParser = require('koa-bodyparser');

const app = new Koa();
app.use(bodyParser());

app.use(async ctx => {
  // the parsed body will store in ctx.request.body
  // if nothing was parsed, body will be an empty object {}
  ctx.body = ctx.request.body;
});
```

代码示例
```js 
var app = new Koa();
const bodyParser = require('koa-bodyparser');
//配置koa-bodyparser中间件
app.use(bodyParser());
router.get('/', async (ctx) => {  
  let title="你好koa ejs 模板"
  await ctx.render("index.html",{   //注意：await
    "title":title,
  })
});

//接收post传值  提交数据
router.post('/doLogin', async (ctx) => {  
  console.log(ctx.request.body.username);
  console.log(ctx.request.body.pass);
  ctx.body=ctx.request.body
});

//接收put传值 修改数据
router.put('/doEdit', async (ctx) => {   
  console.log(ctx.request.body)
  ctx.body=ctx.request.body
});

//接收delete的值
router.delete('/doDelete', async (ctx) => {   
  console.log(ctx.query)
  ctx.body=ctx.query
});

//启用router路由
app
  .use(router.routes())
  .use(router.allowedMethods());

//监听端口
app.listen(3000,()=>{
    console.log("koa启动完成")
})
```


#### 2. koa-session
session中间件。它提供了一种在用户的每个会话中存储和管理数据的方式。这对于跟踪用户状态，如登录状态、购物车内容或其他用户特定数据非常有用。

```js 
var app = new Koa();
const session = require('koa-session');

//配置session
app.keys = ['this is str']; //用于对cookies的签名， session和cookie会用到

const CONFIG = {
  key: 'koa.itying', /* 设置cookie的名称 */
  maxAge: 1000 * 10 * 30,  /* cookie过期时间*/

  rolling: true, /** 在每次请求时强行设置 cookie，这将重置 cookie 过期时间（默认：false） */
  renew: false, /** 快到期时设置cookie (boolean) renew session when session is nearly expired, so we can always keep user logged in. (default is false)*/
  secure: false, /** (boolean) secure cookie*/
};

router.get('/', async (ctx) => {
  ctx.session.username = "张三"; //设置session key是username，value是"张三"
  ctx.session.userinfo = "李四";
  ctx.body = "首页"
});
router.get('/news', async (ctx) => {
  console.log(ctx.session.username); //获取session中的某个key对应的值
  console.log(ctx.session.userinfo)
  ctx.body = "首页"
});
```
<b class="danger">key: 'koa.itying'</b> 设置后浏览器中的cookie会有个名称为koa.itying的cookie
<b class="danger">app.keys</b> 是 Koa 应用程序的一个属性，用于 <b class="danger">签名和验证 cookie</b> 。这是一个包含一组密钥的数组，这些密钥将用于对 session cookies 进行签名，以保证它们的完整性和安全性。






----
<span class="success">
    test asdfds adasf dfas 
</span>

<span class="alert danger">
    test asdfds adasf dfas 
</span>

<span class="alert info">
    test asdfds adasf dfas 
</span>


<span class="alert success">
    test asdfds adasf dfas 
</span>

<div class="alert warning">python不区分单精度和双精度浮点
数，默认双精度，int也不细分short,long整型)
</div>

<div class="alert asso">python不区分单精度和双精度浮点
数，默认双精度，int也不细分short,long整型)
</div>

<div class="alert doubt">python不区分单精度和双精度浮点
数，默认双精度，int也不细分short,long整型)
</div>