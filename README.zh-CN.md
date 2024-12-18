# Web项目 部署 Action

简体中文 | [English](./README.md)

一个用于通过 SSH 将 Web 应用程序部署到远程服务器的 GitHub Action。

## 特性
- 自动化 Web 应用程序部署
- 基于 SSH 的安全部署
- 带时间戳的版本管理
- 基于软链接的零停机部署

## 输入参数
| 名称 | 描述 | 是否必需 | 默认值 |
|------|------|----------|--------|
| source_dir | 要部署的文件所在目录 | 是 | dist |
| deploy_dir | 服务器上的目标目录 | 是 | - |
| server_key | 服务器 SSH 私钥 | 是 | - |
| server_user | SSH 用户名 | 是 | - |
| server_host | 服务器主机名或 IP | 是 | - |
| node_version | Node.js 版本 | 是 | 18 |

## 使用示例

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

            - name: 部署到服务器
              uses: zenoskongfu/server_deploy@0.0.1
              with:
                  source_dir: "dist"
                  deploy_dir: "/www/wwwroot/test-github"
                  server_key: ${{secrets.SERVER_KEY}}
                  server_user: ${{secrets.SERVER_USER_NAME}}
                  server_host: ${{secrets.SERVER_HOST}}
```

### 前置条件
- 先做一些必要操作，如安装依赖、代码构建等
- 然后使用 `zenoskongfu/server_deploy` 开始将代码传输给服务器

`zenoskongfu/server_deploy` 需要的参数是：
- source_dir: 构建产物的路径
- deploy_dir: 部署到服务器的路径
- server_key: 与服务器通信的 SSH 私钥
- server_user: 服务器的登录账号
- server_host: 服务器的 IP 或域名

### 效果
构建产物会被传输至服务器。

### 亮点
`zenoskongfu/server_deploy` 采用软链接 current，每次部署新目录，都会将 current 指向新生成的目录。