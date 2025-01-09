# 初始化开发工具

开发 Dify 插件需要进行以下准备。

### 前置准备 <a href="#qian-zhi-zhun-bei" id="qian-zhi-zhun-bei"></a>

* Dify 插件脚手架工具
* Python 环境，版本号 ≥ 3.12

#### **1. 安装 Dify 插件脚手架工具**

访问 Dify 插件脚手架 [GitHub 项目地址](https://github.com/langgenius/dify-plugin-daemon/releases)，选择并下载合适的操作系统版本。

本文**以装载 M 系列芯片的 macOS** 为例。在上文的项目地址中下载 `dify-plugin-darwin-arm64` 文件，然后在终端输入前往该文件的所在路径的命令，并给予其执行权限。

```
chmod +x dify-plugin-darwin-arm64
```

运行以下命令检查安装是否成功：

```
./dify-plugin-darwin-arm64 version
```

> 若系统提示 “Apple 无法验证” 错误，请前往设置 → 隐私与安全性 → 安全性，轻点 “仍要打开” 按钮。

运行命令后，终端若返回类似 `v0.0.1-beta.15` 的版本号信息，则说明安装成功。

> 如果想要在系统全局使用 `dify` 命令来运行该脚手架，建议将该二进制文件重命名为 `dify` 并拷贝至 `/usr/local/bin` 系统路径内。
>
> 配置完成后，在终端输入 `dify -v` 命令后将输出版本号。

#### **2. 初始化 Python 环境**

详细说明请参考 [Python 安装教程](https://pythontest.com/python/installing-python-3-11/)，或询问 LLM 获取完整的安装教程。

