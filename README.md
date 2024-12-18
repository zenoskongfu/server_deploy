## 介绍

这是一个部署前端网页到远程服务器的Action

**支持任意远程服务器**
- 阿里云
- 腾讯云
- 华为云
- ...

> 为什么支持任意远程服务器，两个原因
> - 部署前端项目很简单，只需要传递文件给服务就可以
> - 与服务器交互的方式是ssh，所以支持ssh的服务器都可以

## 用法

```yml
on:
    push:
        branches: [develop]

jobs:
    develop-deploy:
        runs-on: ubuntu-latest
        environment: development
        steps:
            - name: Checkout Code
              uses: actions/checkout@v4.2.2
            
            #...
            #...

            - name: Test Action
              uses: zenoskongfu/server_deploy@0.0.1
              with:
                  source_dir: "dist"
                  deploy_dir: "/www/wwwroot/test-github"
                  server_key: ${{secrets.SERVER_KEY}}
                  server_user: ${{secrets.SERVER_USER_NAME}}
                  server_host: ${{secrets.SERVER_HOST}}

```
- 先做一些必要操作，安装依赖，代码构建等等
- 然后使用`zenoskongfu/server_deploy`开始将代码传输给服务器
  
`zenoskongfu/server_deploy`需要的参数是：
- source_dir: 构建产物的路径
- deploy_dir: 部署到服务器的路径
- server_key: 与服务器通信的SSH私钥
- server_user: 服务器的登录账号
- server_host: 服务器的IP或域名


### 效果

构建产物会被传输至服务器

### 亮点

`zenoskongfu/server_deploy`采用软链接current，每次部署新目录，都会将current指向新生成的目录
