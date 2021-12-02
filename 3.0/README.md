<p>
  <a href="https://github.com/helsingborg-stad/municipio-deploy">
    <img src="../images/hbg-github-logo-combo.png" alt="Logo" width="300">
  </a>
</p>

# 3.0 deploy action
This action will setup the build machine with php and node install all composer packages and run `php ./build.php`.  
The `build.php` is expected to perform a complete build of the site.  
All outcome from the build script is the pushed to a host with rsync based on below parameters.  
Lastly it will clear all nginx, object and blade cache.  

## Parameters

| Parameter name              | Description                                                                  | Required |
|-----------------------------|------------------------------------------------------------------------------|----------|
| deploy-host                 | Host domain or ip                                                            | true     |
| deploy-host-path            | Host deployment path                                                         | true     |
| deploy-host-backup-path     | Host rsync backup path                                                       | true     |
| deploy-host-user            | Host deploy ssh user name                                                    | true     |
| deploy-host-user-key        | Host deploy ssh user key                                                     | true     |
| deploy-host-web-server-user | Host web server user                                                         | true     |
| github-token                | github token for github npm package usage, use built in secrets.GITHUB_TOKEN | true     |

## Secrets
All parameters are sensitive information and should ONLY use [Github secrets.](https://docs.github.com/en/actions/security-guides/encrypted-secrets).  
Secrets can be added on organisation level spanning all repositories in the org or on a repository level where Github actions in the current repository will be the only one able to read it.

## Usage
In your Municipio 3.0 site repository:
- Create a file in `.github/workflows/` named ex. `deploy-branchname.yml`.
- Use the following template.
```
name: Build and deploy beta.

on:
  push:
    branches: [ branchname ]
    paths-ignore:
      - .github/**

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - uses: helsingborg-stad/municipio-deploy/3.0@master
      with:
        deploy-host: ${{ secrets.DEPLOY_REMOTE_HOST_DOMAIN_SE }}
        deploy-host-path: ${{ secrets.DEPLOY_REMOTE_PATH_DOMAIN_SE }}
        deploy-host-backup-path: ${{ secrets.DEPLOY_REMOTE_BACKUP_DIR_DOMAIN_SE }}
        deploy-host-user: ${{ secrets.DEPLOY_REMOTE_USER }}
        deploy-host-user-key: ${{ secrets.DEPLOY_KEY }}
        deploy-host-web-server-user: ${{ secrets.WEB_SERVER_USER }}
        github-token: ${{ secrets.GITHUB_TOKEN }}
  ```
- Make sure branchname is matching the branch you would want to deploy.
