# Web安全及防御

# 前言

其实, 前端的安全并没有很多, 不过知道了, 起码后端兄弟不会那么累了。
本文主要讨论以下几种攻击方式 :

- **XSS方式**
- **CSRF方式**
- **点击劫持**

希望大家在阅读完文本之后, 能够很好地回答以下的几个问题:

- **前端的攻击方式有哪些?**
- **什么是XSS方式? XSS攻击有几种类型?如何防范XSS攻击?**
- **什么是CSRF攻击?如何防范CSRF攻击?**
- **如何检测网站是否安全?**

## 1. XSS方式

XSS(Cross-Site-Scripting),跨站脚本攻击是一种代码注入攻击。攻击者在目标网站上注入恶意代码,当被攻击者登录网站时就会执行这些恶意代码, 这些脚本可以读取 cookie ,session tokens , 或者其他敏感的网站消息,对用户进行钓鱼欺诈,甚至发起蠕虫攻击等。

**XSS的本质:恶意代码未经过滤,与网站正常的代码混在一起;浏览器无法分辨哪些脚本是可信的,导致恶意脚本被执行。由于直接在用户的终端执行,恶意代码能够直接获取用户的信息,利用这些信息冒充用户向网站发起攻击**

XSS的分类:

- 存储型
- 反射型
- DOM 型

### 1.1 反射型XSS

当用户点击一个恶意链接, 或者提交一个表单, 或者进入一个恶意网站时, 注入脚本进入被攻击者的网站。Web 服务器将注入脚本,比如一个错误信息, 搜索结果等 , 未进行过滤直接返回到用户的浏览器上。

反射型XSS的攻击步骤:

- 1.攻击者构造出特殊的URL, 其中包含恶意代码
- 2.用户打开带有恶意代码的URL时, 网站服务端将恶意代码从 URL中取出, 拼接在 HTML中返回给浏览器
- 3.用户浏览器接收到响应后解析执行, 混在其中的恶意代码也被执行。
- 4.恶意代码窃取用户数据并发送到攻击者的网站, 或者冒充用户的行为, 调用目标网站接口执行攻击者指定的操作。

反射型 XSS 漏洞常见于 URL 传递参数的功能 , 如网站搜索 , 跳转等。由于需要用户主动打开恶意的 URL 才能生效 , 攻击者往往会结合多种手段诱导用户点击

POST 的内容也可以触发反射型 XSS , 只不过其触发条件比较苛刻 (需要构造表单提交页面 , 并引导用户点击),所以非常少见。

**话不多说 , 我们来举个例子**

![img](https://user-gold-cdn.xitu.io/2020/3/21/170fc3ec07a4518e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```js
//前端代码
// index.html
// 由于必须诱导用户点击,所以可能是这样的
<button id="btn" onclick="test()">
    //这是一张很好看的图片
    <img src="https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=1136880418,2628684495&fm=15&gp=0.jpg" alt="">
</button>
<script>
    var test = function() {
    // 构造出的特殊的 url , 其中包含恶意代码
    let url = `/welcome?type=<script>alert('恶意内容')<\/script>`;
    location.href = url;
}
</script>

// 后端代码
// server.js
const express = require('express');
const app = express();
app.use(express.static(path.join(__dirname)))
app.get('/welcome',function(req, res) {
    // 把恶意代码当做字符串, 传输回浏览器
    res.send(`${req.query.type}`);
    res.end();
})

app.listen(3000, ()=> {
    console.log('server is running at port 4000')
});
```

**如果不希望被前端拿到 cookie, 后端可以设置 httpOnly (不过这个不是 XSS 的解决方案 , 只能降低受损范围)**

#### 如何防范反射型 XSS攻击?

**对字符串进行编码**
对url的查询参数进行转义后再输出到页面

```js
app.get('/welcome',function(req,res) {
    //对查询参数进行编码，避免反射型 XSS攻击
    res.send(`${encodeURIComponent(req.query.type)}`); 
})
```

**总结:简单来说,前端向后端发送GET请求数据,后端返回结果之前,必须先对url查询参数进行编码再输出到页面**

### 1.2 DOM型XSS

DOM型 XSS 攻击 , 实际上就是前端 javaScript 代码不够严谨 , 把不可信的内容插入到了页面 。在使用 .innerHTML , .outerHTML , appendChild , document.write()等 API 时要特别小心 , **不要把不可信的数据作为为 HTML 插入到页面上** , 尽量使用 .innerText , .textContent , setAttribute()等。

#### DOM型XSS 的攻击步骤:

- 1.攻击者构造出特殊数据 , 其中包含恶意代码
- 2.用户浏览器执行恶意代码
- 恶意代码窃取用户数据并发送到攻击者的网站 , 或者冒充用户的行为 , 调用目标网站接口执行攻击者指定的操作。

#### 如何防范 DOM型XSS攻击

防范 DOM 型 XSS攻击的核心就是对输入内容进行转义 (DOM 中的内联事件监听器和链接跳转都能把字符串作为代码运行, 需要对其内容进行检查)

1. 对于 url 链接(例如图片的src属性), 那么直接使用 encodeURIComponent来转义

2. 非 url , 我们可以这样进行编码

   ```js
    function encodeHtml(str) {
        return str.replace(/"/g, '&quot;')
                .replace(/'/g, '&apos;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;');
    }
   ```

DOM型 XSS攻击中 , 取出和执行恶意代码由浏览器端完成 , 属于前端 javaScript 自身的安全漏洞

**再举个例子**

```js
//前端
<form onsubmit="return false">
    <div class="form-group">
        <label for="comments">商品评论</label>
        <input class="form-control" type="text" id="comments" />
    </div>
    <div class="form-group">
        <input class="btn btn-danger" type="button" id="attck" value="会被攻击" />
        <input class="btn btn-primary" type="button" id="security" value="我很安全" />
    </div>
</form>
<form>
    <ul class="list-group"></ul>
</form>
<script src="/node_modules/jquery/dist/jquery.js"></script>
<script>
    // DOM型 XSS 攻击
    $('#attck').click(function () {
        let commend = $('#comments').val();
        console.log(commend);
        if ($.trim(commend)) {
            $('.list-group').append(`<li class="list-group-item">${commend}</li>`);
            $('#comments').val('');
        }
    })
    // HTML 编码
    function encodeHtml(str) {
        return str.replace(/"/g, '&quot;')
            .replace(/'/g, '&apos;')
            .replace(/</g, '&lt;')
        replace(/>/g, '&gt;')
    }

    $('#security').click(function () {
        let commend = $('#comments').val();
        console.log(commend)
        // 对内容进行转义
        // 如果是url 地址 , 可以直接使用 encodeURIComponent 进行转义
        if ($.trim(commend)) {
            $('.list-group').append(`<li class="list-group-item">${encodeHtml(commend)}</li>`);
            $('#comments').val('');
        }
    })
</script>
```

**总结:就是不要相信用户输入的内容,如果非要在页面上插入 HTML,就必须先转义再插入。对于URL可以使用encodeURIComponent,对于非URL可以使用上面的方法**