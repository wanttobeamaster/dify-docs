# 打包插件文件并发布

完成插件开发后，你可以将插件项目打包为一个本地文件并分享给他人。获取插件文件后即可安装至 Dify Workspace 内。本文将为你介绍如何将插件项目打包为本地文件，以及如何通过本地文件安装插件。

### 前置准备

打包插件需要使用 Dify 插件开发脚手架工具，你可以前往 [Github 项目地址](https://github.com/langgenius/dify-plugin-daemon/releases)，选择并下载适用于你的操作系统版本。&#x20;

本文**以装载 M 系列芯片的 macOS** 为例，下载 `dify-plugin-darwin-arm64` 文件，然后在终端输入前往该文件的所在路径的命令，并给予其执行权限。

```bash
chmod +x dify-plugin-darwin-arm64
```

为了便于全局使用该脚手架工具，建议将该二进制文件重命名为 `dify` 并拷贝至 `/usr/local/bin` 系统路径内。

配置完成后，在终端输入 `dify -v` 命令查看是否能够输出版本号信息。

### 打包插件

插件项目开发完成后，请确保已完成[远程连接测试](../developing-plugins/extension.md#tiao-shi-cha-jian)。打包插件时需前往插件项目的上一级目录，然后运行以下插件打包命令：

```bash
cd ../
dify plugin package ./your_plugin_project
```

运行命令后，将在当前路径下生成以 `.difypkg` 后缀结尾的文件。

![](https://assets-docs.dify.ai/2024/12/98e09c04273eace8fe6e5ac976443cca.png)

### 安装插件

访问 Dify 插件管理页，轻点右上角的**安装插件** → **通过本地文件**安装，或将插件文件拖拽至页面空白处安装插件。

![](https://assets-docs.dify.ai/2024/12/8c31c4025a070f23455799f942b91a57.png)

### 发布插件

你可以将该插件文件分享给他人，或上传至互联网供他人下载。



