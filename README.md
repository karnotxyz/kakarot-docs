# 网站

该网站使用现代静态网站生成器[Docusaurus 3](https://docusaurus.io/)制作。

### 安装

```
$ pnpm install
```

### 本地开发

```
$ pnpm run start
```

该命令启动本地开发服务器并打开浏览器窗口。
大多数更改都是实时反映的，而无需重新启动服务器。

### 构建

```
$ pnpm build
```

该命令生成静态内容到 `build`目录中，并且可以使用任何静态内容托管服务提供服务。

### 开发

使用SSH:

```
$ USE_SSH=true pnpm deploy
```

不使用SSH:

```
$ GIT_USER=<Your GitHub username> pnpm deploy
```

如果您正在使用GitHub Pages进行托管，则此命令是一种方便的方法建立网站并推送到`gh-pages`分支。