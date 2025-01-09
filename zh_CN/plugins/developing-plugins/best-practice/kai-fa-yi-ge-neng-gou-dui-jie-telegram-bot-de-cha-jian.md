# 开发一个能够对接 Telegram Bot 的插件

### 项目背景

将 LLM 服务对接至常用的实时聊天平台（IM）一直都是热门的最佳实践话题。Dify 插件生态致力于支持更简单、更易用的接入方式。本文将以 Telegram 为例，详细介绍如何开发一个接入 Telegram Bot 的插件。

[Telegram](https://telegram.org/) 是一个自由开放的实时通信平台，它提供了丰富的 API，其中一个易用的功能是 Webhook，一个基于事件的机制。我们将利用该机制创建 Telegram Bot 插件，原理如下图所示：

![](https://assets-docs.dify.ai/2024/12/08d0cc0074efe3b81b15ade2d888e785.png)

**原理简介：**

1.  **用户使用 Telegram Bot**

    当用户在 Telegram 中发出一条消息的时候，Telegram 会发出一个 HTTP 请求到 Dify 插件。
2.  **消息转发至 Telegram Bot 插件**

    好比邮件系统需要一位收件人的邮箱，用户在使用 Telegram bot 时，需要将消息转发回 Dify 应用进行处理。此时可以通过 Telegram 的 API 配置一个 Telegram Webhook 的地址，并将其填入至插件内，建议连接。
3.  **插件在接受到消息后，返回至某个 Dify 应用**

    插件处理 Telegram 的请求，分析用户输入的是什么，并调用 Dify 中的 App 来获取回复内容。
4.  **Dify 应用回应后，将消息返回至 Telegram Bot 并回答用户**

    获取 Dify 应用的回复后，通过插件将消息原路返回至 Telegram Bot，使得用户能够在使用 Telegram 时直接与 Dify 应用互动

### 前置准备 <a href="#qian-zhi-zhun-bei" id="qian-zhi-zhun-bei"></a>

* 申请 Telegram Bot
* Dify 插件脚手架工具
* Python 环境，版本号 ≥ 3.10

#### 申请 Telegram Bot

根据 [@BotFather](https://t.me/BotFather) 指引，创建一个新的机器人。详细的创建过程请参考 [Telegram 官方文档](https://core.telegram.org/bots/tutorial)。

创建完成后将获取 HTTP API Token，供后续步骤使用。

![Telegram api token](https://assets-docs.dify.ai/2024/12/668783d0362200257b2cb5385ecbacff.png)

**安装 Dify 插件脚手架工具**

详细说明请参考[初始化开发工具](../tool-initialization.md)。

**初始化 Python 环境**

详细说明请参考 [Python 安装教程](https://pythontest.com/python/installing-python-3-11/)，或询问 LLM 获取完整的安装教程。

### 开发插件

现在开始实际的插件编码工作。在开始之前，请确保你已经阅读过[快速开始：开发 Extension 类型插件](../extension.md)，或已动手开发过一次 Dify 插件。

#### 初始化项目

运行以下命令初始化插件开发项目：

```bash
./dify-plugin-darwin-arm64 plugin init
```

按照提示填写项目的基础信息，选择 `extension` 模板，并且在权限部分授予 `Apps` 和 `Endpoints` 两个权限。

如需了解更多关于插件反向调用 Dify 平台能力，请参考[反向调用：App](../../api-documentation/fan-xiang-diao-yong-dify-fu-wu/app.md)。

![Plugins permission](https://assets-docs.dify.ai/2024/12/d89a6282c5584fc43a9cadeddf09c0de.png)

#### 1. 编辑配置表单

在这个插件中，需要指定使用哪个 Dify 的 App 进行回复，并且在回复的时候需要使用到 Telegram 的 bot token，因此需要在插件表单中加上这两个字段。

修改 group 路径下的 yaml 文件，例如 `group/your-project.yaml。`表单配置文件的名称由创建插件时填写的基础信息决定，你可以修改对应的 yaml 文件。

**示例代码：**

`your-project.yaml`

```yaml
settings:
  - name: bot_token
    type: secret-input
    required: true
    label:
      en_US: Bot Token
      zh_Hans: 机器人 Token
      pt_BR: Token do Bot
      ja_JP: ボットトークン
  - name: app
    type: app-selector
    scope: chat
    required: true
    label:
      en_US: App
      zh_Hans: 应用
      pt_BR: App
      ja_JP: アプリ
    placeholder:
      en_US: the app you want to use to answer telegram messages
      zh_Hans: 你想要用来回答 Telegram 消息的应用
      pt_BR: o app que você deseja usar para responder mensagens do Telegram
      ja_JP: あなたが Telegram メッセージに回答するために使用するアプリ
endpoints:
  - endpoints/your_project.yaml
```

代码数据结构说明：

```
  - name: app
    type: app-selector
    scope: chat
```

*   type 字段指定为 app-selector 字段

    用户在使用插件时可以访问某个 Dify 应用并进行消息转发。
*   scope 字段指定为 chat 字段

    只能使用 `agent` 、`chatbot` 、`chatflow` 等类型的 app。

最后修改 `endpoints/your_path.yaml` 文件中的请求路径和请求方式，需要将 method 修改为 POST 方式。

**示例代码：**

`endpoints/your_path.yaml`

```yaml
path: "/your_project/message"
method: "POST"
extra:
  python:
    source: "endpoints/your_project.py"
```

#### 2. 编辑功能代码

修改 `endpoints/your_project.py`文件，并在其中添加下面的代码：

```python
import json
import traceback
import requests
from typing import Mapping
from werkzeug import Request, Response
from dify_plugin import Endpoint

class TelegramWebhook(Endpoint):
    def _invoke(self, r: Request, values: Mapping, settings: Mapping) -> Response:
        """
        Invokes the endpoint with the given request.
        """
        data = r.get_json()

        message = data.get("message", {})
        chat = message.get("chat", {})
        chat_id = chat.get("id")
        if not chat or not message:
            return Response(status=200, response="ok")

        message_id = message.get("message_id")
        bot_token = settings.get("bot_token", "")
        chat_type = chat.get("type")

        reply_message = {
            "method": "sendMessage",
            "chat_id": chat_id,
            "reply_to_message_id": message_id,
            "text": message.get("text"),
        }
        
        return Response(
            status=200,
            response=json.dumps(reply_message),
            content_type="application/json",
        )
```

为了便于测试，插件功能目前仅能重复用户输入的内容，暂不调用 Dify app。

#### 2.1 调试插件

前往 Dify 平台，获取 Dify 插件远程调试的连接地址和密钥。

<figure><img src="https://assets-docs.dify.ai/2024/11/1cf15bc59ea10eb67513c8bdca557111.png" alt=""><figcaption></figcaption></figure>

回到插件项目，复制 `.env.example` 文件并重命名为 `.env`。

```bash
INSTALL_METHOD=remote
REMOTE_INSTALL_HOST=remote-url
REMOTE_INSTALL_PORT=5003
REMOTE_INSTALL_KEY=****-****-****-****-****
```

运行 `python -m main` 命令启动插件。在插件页即可看到该插件已被安装至 Workspace 内。其他团队成员也可以访问该插件。

```bash
python -m main
```

#### 设置插件 Endpoint

在 Dify 的插件管理页中找到自动安装的测试插件，新建一个 Endpoint，填写名称、Bot token、选择需要连接的 app。

![测试插件](https://assets-docs.dify.ai/2024/12/93f1bc3d52635ff5bf177331caaf7afa.png)

复制插件内的 URL，将其与下面的命令组成测试请求命令。

其中 `<hook_url>` 填写为刚刚复制的地址，`<bot_token>` 填写在[前置准备](kai-fa-yi-ge-neng-gou-dui-jie-telegram-bot-de-cha-jian.md#qian-zhi-zhun-bei)中获取的 Telegram HTTP API Token。

```bash
curl -F "url=<hook_url>" https://api.telegram.org/bot<bot_token>/setWebhook
```

执行完以后应该可以看到如下返回结果：

![curl response](https://assets-docs.dify.ai/2024/12/4e2a924cfed25ea88d6692d55fac39e3.png)

向 Telegram 的 bot 发一条消息，可以发现它会原封不动地重复我们发送的消息，说明插件已正确和 Telegram bot 建立连接。

![](https://assets-docs.dify.ai/2024/12/a7c4f708fd0c11734f3359c1e1b6ad5a.png)

#### 2.2 使用 Dify App 进行回复

修改功能代码，添加使用 Dify App 回复的代码：

```python
import json
import traceback
from typing import Mapping
from werkzeug import Request, Response
from dify_plugin import Endpoint

class TelegramWebhook(Endpoint):
    def _invoke(self, r: Request, values: Mapping, settings: Mapping) -> Response:
        """
        Invokes the endpoint with the given request.
        """
        data = r.get_json()

        message = data.get("message", {})
        chat = message.get("chat", {})
        chat_id = chat.get("id")
        if not chat or not message:
            return Response(status=200, response="ok")

        message_id = message.get("message_id")
        bot_token = settings.get("bot_token", "")
        chat_type = chat.get("type")

        try:
            response = self.session.app.chat.invoke(
                app_id=settings["app"]["app_id"],
                query=message.get("text", ""),
                inputs={},
                response_mode="blocking",
            )
            return Response(
                status=200,
                response=json.dumps({
                    "method": "sendMessage",
                    "chat_id": chat_id,
                    "reply_to_message_id": message_id,
                    "text": response.get("answer", ""),
                }),
                content_type="text/plain",
            )
        except Exception as e:
            err = traceback.format_exc()
            return Response(
                status=200,
                response="Sorry, I'm having trouble processing your request. Please try again later." + str(err),
                content_type="text/plain",
            )
```

代码使用了 `self.session.app.chat.invoke` 调用 Dify 平台内的 App，并传递了 `app_id` 和 `query` 等信息，最后将 response 的内容返回至 Telegram Bot。

重启插件并重新调试，可以发现 Telegram Bot 已正确输出 Dify App 的答复消息。

![](https://assets-docs.dify.ai/2024/12/5987709c373903925ba8f639606aa554.png)

### 打包插件

确认插件能够正常运行后，可以通过以下命令行工具打包并命名插件。运行以后你可以在当前文件夹发现 `telegram.difypkg` 文件，该文件为最终的插件包。

```bash
dify plugin package ./telegram
```

恭喜，你已完成一个插件的完整开发、测试打包过程！

### 发布插件

现在可以将它上传至 [Dify Marketplace 仓库](https://github.com/langgenius/dify-plugins) 来发布你的插件了！不过在发布前，请确保你的插件遵循了[插件发布规范](https://langgenius.feishu.cn/wiki/OuRdwGwGwiIOdPkJqTncH5e2nNb)。

### 参考阅读

如果你想要查看完整 Dify 插件的项目代码，请前往 [Github 代码仓库](https://github.com/langgenius/dify-official-plugins)。除此之外，你可以看到其它插件的完整代码与具体细节。

如果想要了解更多插件，请参考以下内容。

**快速开始：**

* [开发 Extension 类型插件](../extension.md)
* [开发 Model 类型插件](broken-reference)
* [Bundle 类型插件：将多个插件打包](../bundle.md)

**插件接口文档：**

* [Manifest](../../api-documentation/manifest.md) 结构
* [Endpoint](../../api-documentation/endpoint.md) 详细定义
* [反向调用 Dify 能力](../../api-documentation/fan-xiang-diao-yong-dify-fu-wu/)
* [工具](../../api-documentation/tool.md)
* [模型](../../api-documentation/model/)



