# 这，就是 JSON Server ！

## 一、先搭建 Web 服务器

[https://nodejs.org/](https://nodejs.org/zh-cn)

[https://www.npmjs.com/package/json-server](https://www.npmjs.com/package/json-server)

[https://github.com/typicode/json-server](https://github.com/typicode/json-server)

**安装**

`npm install json-server`

**创建 db.json**

```json
{
  "posts": [
    { "id": "1", "title": "a title", "views": 100 },
    { "id": "2", "title": "another title", "views": 200 }
  ],
  "comments": [
    { "id": "1", "text": "a comment about post 1", "postId": "1" },
    { "id": "2", "text": "another comment about post 1", "postId": "1" }
  ],
  "profile": {
    "name": "typicode"
  }
}
```

**启动 Web 服务器**

`npx json-server db.json`

**全局安装**

`npm install -g json-server`

## 二、能查询才算好汉

**一般查询**

[http://localhost:3000/posts](http://localhost:3000/posts)

[http://localhost:3000/comments](http://localhost:3000/comments)

[http://localhost:3000/profile](http://localhost:3000/profile)

**根据 ID**

[http://localhost:3000/posts/1](http://localhost:3000/posts/1)

**分页**

[http://localhost:3000/posts?_page=1&_per_page=2](http://localhost:3000/posts?_page=1&_per_page=2)

**排序**

[http://localhost:3000/posts?_sort=id,-views](http://localhost:3000/posts?_sort=id,views)

**嵌入数据**

[http://localhost:3000/posts?_embed=comments](http://localhost:3000/posts?_embed=comments) （postId 格式）

[http://localhost:3000/comments?_embed=post](http://localhost:3000/comments?_embed=post)

## 三、添加数据 多多益善

[https://www.postman.com/](https://www.postman.com/) （Body raw text、json 都可以 json更规范）

```
POST http://localhost:3000/posts
不提供ID 随机ID
{"title": "a title", "views": 100}   
提供ID 指定ID
{"id": "6", "title": "a title", "views": 100}   
ID 是数字类型也是一样，希望以后能加上这个功能。
```

## 四、先改脾气 再改数据

**PUT /posts/:id 更新全部数据，需要提供完整数据**

```
PUT http://localhost:3000/posts/6
{"views": 30}   
PUT http://localhost:3000/posts/6
{"title": "another title", "views": 30}  
PUT http://localhost:3000/profile
{"name": "zhouhuajian","age": 30,"nickname": "Joe"}
```

**PATCH /posts/:id 更新部分数据**

```
PATCH http://localhost:3000/posts/5
{"views": 30}   
```

## 五、删删数据更健康

DELETE /posts/:id

```
DELETE http://localhost:3000/posts/6
DELETE http://localhost:3000/posts/bd1e
```

不能删 profile

```
DELETE http://localhost:3000/profile (Not Found，文档也没提供，说明暂时没这功能。)
```

直接改 db.json

## 六、聊聊其他事情

**静态资源 HTML/CSS/JS**

放到 ./public 不需要重启 JSON Server

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>首页</title>
</head>

<body>
  <h1>首页</h1>
  <a href="/login">登录</a>
  <h3>帖子列表</h3>
  <ul></ul>
  <script>
    fetch('/posts')
      .then(res => res.json())
      .then(posts => {
        let html = ''
        for (const post of posts) {
          html += `<li>${post.title}</li>`
        }
        document.querySelector('ul').innerHTML = html
      })
  </script>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>登录页面</title>
</head>

<body>
  <h1>登录页面</h1>
</body>

</html>
```

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>商城页面</title>
</head>

<body>
  <h1>商城页面</h1>
</body>

</html>
```

**指定静态资源位置**

`npx json-server --static C:\Users\zhouhuajian\Desktop\frontend db.json`

还可以指定多个目录，./public 默认生效

```
> npx json-server --help 
Usage: json-server [options] <file>

Options:
  -p, --port <port>  Port (default: 3000)
  -h, --host <host>  Host (default: localhost)
  -s, --static <dir> Static files directory (multiple allowed)
  --help             Show this message
  --version          Show version number
```

**修改默认端口**

`npx json-server --port 3001 db.json`

