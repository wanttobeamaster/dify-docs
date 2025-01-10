---
hidden: true
---

# Agent 策略插件

Agent 策略插件能够帮助内部的 LLM 执行推理或决策逻辑，包括工具选择、调用和结果处理。

本文将演示如何创建一个具备工具调用（Function Calling）能力，自动获取当前准确时间的插件。Agent 策略插件能够辅助应用生成更加精确的信息。

### 前置准备

* Dify 插件脚手架工具
* Python 环境，版本号 ≥ 3.12

关于如何准备插件开发的脚手架工具，详细说明请参考[初始化开发工具](tool-initialization.md)。

### &#x20;1. 初始化插件模板

运行以下命令，初始化 Agent 插件开发模板。

```
dify plugin init
```

并按照提示填写对应信息。

```
➜  Dify Plugins Developing dify plugin init
Edit profile of the plugin
Plugin name (press Enter to next step): # 填写插件的名称
Author (press Enter to next step): Author name # 填写插件作者
Description (press Enter to next step): Description # 填写插件的描述

Select the language you want to use for plugin development, and press Enter to con
BTW, you need Python 3.12+ to develop the Plugin if you choose Python.
-> python # 选择 Python 环境
  go (not supported yet)
  
  
```



初始化插件模板后将生成一个代码文件夹，包含了插件开发所需的完整资源。熟悉 Agent 策略插件的整体代码结构有助于插件的开发过程，代码结构如下：

```
├── GUIDE.md               # User guide and documentation
├── PRIVACY.md            # Privacy policy and data handling guidelines
├── README.md             # Project overview and setup instructions
├── _assets/             # Static assets directory
│   └── icon.svg         # Agent strategy provider icon/logo
├── main.py              # Main application entry point
├── manifest.yaml        # Basic plugin configuration
├── provider/           # Provider configurations directory
│   └── basic_agent.yaml # Your agent provider settings
├── requirements.txt    # Python dependencies list
└── strategies/         # Strategy implementation directory
    ├── basic_agent.py  # Basic agent strategy implementation
    └── basic_agent.yaml # Basic agent strategy configuration
```

### 2. 开发插件功能

Agent 策略插件的主要执行逻辑与功能实现文件是 `strategies/basic_agent.py`。首先可以定义 Agent 插件的所需参数，例如赋予其调用 LLM 模型，使用工具的能力。此处定义最基础的 `model`, `tools`, `query`, `maximum_iterations` 四项参数。

```yaml
identity:
  name: basic_agent # the name of the agent_strategy
  author: novice # the author of the agent_strategy
  label:
    en_US: BasicAgent # the engilish label of the agent_strategy
description:
  en_US: BasicAgent # the english description of the agent_strategy
parameters:
  - name: model # the name of the model parameter
    type: model-selector # model-type
    scope: tool-call&llm # the scope of the parameter
    required: true
    label:
      en_US: Model
      zh_Hans: 模型
      pt_BR: Model
  - name: tools # the name of the tools parameter
    type: array[tools] # the type of tool parameter
    required: true
    label:
      en_US: Tools list
      zh_Hans: 工具列表
      pt_BR: Tools list
  - name: query # the name of the query parameter
    type: string # the type of query parameter
    required: true
    label:
      en_US: Query
      zh_Hans: 查询
      pt_BR: Query
  - name: maximum_iterations
    type: number
    required: false
    default: 5
    label:
      en_US: Maxium Iterations
      zh_Hans: 最大迭代次数
      pt_BR: Maxium Iterations
    max: 50 # if you set the max and min value, the display of the parameter will be a slider
    min: 1
extra:
  python:
    source: strategies/basic_agent.py

```









