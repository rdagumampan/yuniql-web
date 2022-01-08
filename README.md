# yuniql-web

Hugo-based web project for https://yuniql.io

## How to test

```shell
choco install hugo

git clone https://github.com/rdagumampan/yuniql-web.git c:/temp/yuniql-web
cd c:/temp/yuniql-web

hugo server -D
```

## How to publish

```shell
hugo
```

## How to publish in yuniql.io
See https://ci.appveyor.com/project/rdagumampan/yuniql-web

```shell
choco install hugo

cd c:\projects\yuniql-web
hugo

cd c:\projects\yuniql-web\public
git init
git remote add origin https://github.com/rdagumampan/yuniql.io.git
git remote -v
git add . -A
git commit -m "Published from CI/CD pipeline in AppVeyor"
git push origin master --force
```
