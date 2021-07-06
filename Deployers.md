# Web 应用托管平台 Vercel、Netlify、Deno Deploy 体验报告

本文主要记录三个 web 应用 CI、托管平台的使用体验、功能定位。平台分别为：

+ Vercel
+ Netlify
+ Deno Deploy

## 产品功能

**Vercel** 与 **Netlify** 产品定位相似，主要提供前端应用托管和 **Serverless** 函数托管功能，此外的附加功能包括：

+ 生产部署与预览部署配置

+ 域名绑定

+ 命令行工具

+ 自定义构建

**Deno Deploy** 由于仍处于 Beta 阶段，且主要面向 Deno 运行时，功能相对单一，提供函数部署和域名绑定功能。

## 部署结果预览

+ [Vercel App](https://vue-ifwo0n7db-huang825172.vercel.app) / [Vercel Api](https://vue-ifwo0n7db-huang825172.vercel.app/api/hello?name=User)

+ [Netlify App](https://zen-kirch-989415.netlify.app) / [Netlify Api](https://zen-kirch-989415.netlify.app/.netlify/functions/hello-world)

+ [Deno Deploy Api](https://clean-salmon-50.deno.dev)

## 使用体验

**Vercel** 与 **Netlify** 平台的部署流程类似：登陆平台账号，选择模板或从仓库创建项目，获取 Git 服务授权，创建或读取仓库，服务构建与部署。这两个平台提供了大部分常用前端框架的项目模板。

由于前端构建流程一致，各平台的前端应用部署、边缘分发功能可以共享代码仓库，但 Serverless 函数的设计则各不相同。

![image-20210706221249836](C:\Users\youngehuang\AppData\Roaming\Typora\typora-user-images\image-20210706221249836.png)

**Vercel** 平台只要求在 `/api` 目录下放置函数脚本文件，**Netlify** 则要求使用命令行工具连接平台项目并创建云函数文件结构，云函数的输入输出设计也有差异。

```javascript
// Vercel Serverless Helloworld
module.exports = (req, res) => res.status(200).send("Hello World")
```

```javascript
// Netlify Serverless Helloworld
exports.handler = async function (event, context) {
    return {
        statusCode: 200,
        body: JSON.stringify({ message: "Hello World" })
    }
}
```

```javascript
// Deno Serverless Helloworld
addEventListener("fetch", (event) => {
  event.respondWith(
    new Response("Hello world", {
      status: 200,
      headers: { "content-type": "text/plain" },
    }),
  );
});
```

除了输入输出方式的不同，**Vercel** 提供了构建脚本配置项，可以在构建期实现一些灵活操作，而 **Netlify** 提供了 *后台函数*  用于异步处理耗时操作。

+ Railway
+ Render
+ Heroku
+ Webify
