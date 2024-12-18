# Web Deploy Action

[简体中文](./README.zh-CN.md) | English

A GitHub Action for deploying Web applications to a remote server via SSH.

## Features
- Automated Web application deployment
- Secure SSH-based deployment
- Version management with timestamped releases
- Symlink-based zero-downtime deployment

## Inputs
| Name | Description | Required | Default |
|------|-------------|----------|---------|
| source_dir | Directory containing files to deploy | Yes | dist |
| deploy_dir | Target directory on the server | Yes | - |
| server_key | SSH private key for server access | Yes | - |
| server_user | SSH username | Yes | - |
| server_host | Server hostname or IP | Yes | - |
| node_version | Node.js version to use | Yes | 18 |

## Usage Example

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

            - name: Deploy to Server
              uses: zenoskongfu/server_deploy@1
              with:
                  source_dir: "dist"
                  deploy_dir: "/www/wwwroot/test-github"
                  server_key: ${{secrets.SERVER_KEY}}
                  server_user: ${{secrets.SERVER_USER_NAME}}
                  server_host: ${{secrets.SERVER_HOST}}
```

### Prerequisites
- Perform necessary operations like installing dependencies and building code
- Use `zenoskongfu/server_deploy` to transfer code to the server

Required parameters for `zenoskongfu/server_deploy`:
- source_dir: Path to build artifacts
- deploy_dir: Deployment path on server
- server_key: SSH private key for server communication
- server_user: Server login account
- server_host: Server IP or domain name

### Result
Build artifacts will be transferred to the server.

### Highlights
`zenoskongfu/server_deploy` uses a 'current' symlink - each deployment creates a new directory, and 'current' points to the newly generated directory.
