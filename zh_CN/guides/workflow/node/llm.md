# LLM

### 定义

调用大语言模型的能力，处理用户在 “开始” 节点中输入的信息（自然语言、上传的文件或图片），给出有效的回应信息。

<figure><img src="../../../.gitbook/assets/image (71).png" alt=""><figcaption><p>LLM 节点</p></figcaption></figure>

***

### 应用场景

LLM 节点是 Chatflow/Workflow 的核心节点。该节点能够利用大语言模型的对话/生成/分类/处理等能力，根据给定的提示词处理广泛的任务类型，并能够在工作流的不同环节使用。

* **意图识别**，在客服对话情景中，对用户问题进行意图识别和分类，导向下游不同的流程。
* **文本生成**，在文章生成情景中，作为内容生成的节点，根据主题、关键词生成符合的文本内容。
* **内容分类**，在邮件批处理情景中，对邮件的类型进行自动化分类，如咨询/投诉/垃圾邮件。
* **文本转换**，在文本翻译情景中，将用户提供的文本内容翻译成指定语言。
* **代码生成**，在辅助编程情景中，根据用户的要求生成指定的业务代码，编写测试用例。
* **RAG**，在知识库问答情景中，将检索到的相关知识和用户问题重新组织回复问题。
* **图片理解**，使用具备 vision 能力的 LLM，理解与问答图像内的信息。
* **文件分析**，在文件处理场景中，使用 LLM 识别并分析文件包含的信息。

选择合适的模型，编写提示词，你可以在 Chatflow/Workflow 中构建出强大、可靠的解决方案。

***

### 配置示例

在应用编辑页中，点击鼠标右键或轻点上一节点末尾的 + 号，添加节点并选择 LLM。

<figure><img src="../../../.gitbook/assets/image (251).png" alt=""><figcaption><p>LLM 节点配置-选择模型</p></figcaption></figure>

**配置步骤：**

1. **选择模型**，Dify 提供了全球主流模型的[支持](../../../getting-started/readme/model-providers.md)，包括 OpenAI 的 GPT 系列、Anthropic 的 Claude 系列、Google 的 Gemini 系列等，选择一个模型取决于其推理能力、成本、响应速度、上下文窗口等因素，你需要根据场景需求和任务类型选择合适的模型。

{% hint style="info" %}
如果你是初次使用 Dify ，在 LLM 节点选择模型之前，需要在 **系统设置—模型供应商** 内提前完成[模型配置](../../model-configuration/)。
{% endhint %}

2. **配置模型参数**，模型参数用于控制模型的生成结果，例如温度、TopP，最大标记、回复格式等，为了方便选择系统同时提供了 3 套预设参数：创意，平衡和精确。如果你对以上参数并不熟悉，建议选择默认设置。若希望应用具备图片分析能力，请选择具备视觉能力的模型。
3. **填写上下文（可选），** 上下文可以理解为向 LLM 提供的背景信息，常用于填写[知识检索](knowledge-retrieval.md)的输出变量。
4. **编写提示词**，LLM 节点提供了一个易用的提示词编排页面，选择聊天模型或补全模型，会显示不同的提示词编排结构。如果选择聊天模型（Chat model），你可以自定义系统提示词（SYSTEM）/用户（USER）/ 助手（ASSISTANT）三部分内容。

<figure><img src="../../../.gitbook/assets/zh-node-llm.png" alt="" width="352"><figcaption><p>编写提示词</p></figcaption></figure>

如果在编写系统提示词（SYSTEM）时没有好的思路，也可以使用提示生成器功能，借助 AI 能力快速生成适合实际业务场景的提示词。

![提示生成器](../../../.gitbook/assets/zh-node-llm-prompt-generator.png)

在提示词编辑器中，你可以通过输入 **“/”** 或者 **“{”** 呼出 **变量插入菜单**，将 **特殊变量块** 或者 **上游节点变量** 插入到提示词中作为上下文内容。

<figure><img src="../../../.gitbook/assets/image (253).png" alt="" width="366"><figcaption><p>呼出变量插入菜单</p></figcaption></figure>

5. **高级设置**，可以开关记忆功能并设置记忆窗口、开关 Vision 功能或者使用 Jinja-2 模板语言来进行更复杂的提示词等。

***

### 特殊变量说明

**上下文变量**

上下文变量是一种特殊变量类型，用于向 LLM 提供背景信息，常用于在知识检索场景下使用。详细说明请参考[知识检索节点](knowledge-retrieval.md)。

**图片变量**

具备视觉能力的 LLM 可以通过变量读取应用使用者所上传的图片。开启 VISION 后，选择图片文件的输出变量完成设置。

<figure><img src="../../../.gitbook/assets/image (371).png" alt=""><figcaption><p>视觉上传功能</p></figcaption></figure>

**文件变量**

部分 LLMs（例如 [Claude 3.5 Sonnet](https://docs.anthropic.com/en/docs/build-with-claude/pdf-support)）已支持直接处理并分析文件内容，因此系统提示词已允许输入文件变量。为了避免潜在异常，应用开发者在使用该文件变量前需前往 LLM 官网确认 LLM 支持何种文件类型。

![](https://assets-docs.dify.ai/2024/11/05b3d4a78038bc7afbb157078e3b2b26.png)

> 阅读[文件上传](https://docs.dify.ai/zh-hans/guides/workflow/file-upload)了解如何搭建具备文件上传功能的 Chatflow/Workflow 应用。

**会话历史**

为了在文本补全类模型（例如 gpt-3.5-turbo-Instruct）内实现聊天型应用的对话记忆，Dify 在原[提示词专家模式（已下线）](../../../learn-more/extended-reading/prompt-engineering/prompt-engineering-1/)内设计了会话历史变量，该变量沿用至 Chatflow 的 LLM 节点内，用于在提示词中插入 AI 与用户之间的聊天历史，帮助 LLM 理解对话上文。

{% hint style="info" %}
会话历史变量应用并不广泛，仅在 Chatflow 中选择文本补全类模型时可以插入使用。
{% endhint %}

<figure><img src="../../../.gitbook/assets/image (255).png" alt=""><figcaption><p>插入会话历史变量</p></figcaption></figure>

**模型参数**

模型的参数会影响模型的输出效果。不同模型的参数会有所区别。下图为`gpt-4`的参数列表。

<figure><img src="../../../.gitbook/assets/截屏2024-10-18 10.45.17.png" alt="" width="368"><figcaption></figcaption></figure>

主要的参数名词解释如下：

* **温度：** 通常是0-1的一个值，控制随机性。温度越接近0，结果越确定和重复，温度越接近1，结果越随机。
* **Top P：** 控制结果的多样性。模型根据概率从候选词中选择，确保累积概率不超过预设的阈值P。
* **存在惩罚：** 用于减少重复生成同一实体或信息，通过对已经生成的内容施加惩罚，使模型倾向于生成新的或不同的内容。参数值增加时，对于已经生成过的内容，模型在后续生成中被施加更大的惩罚，生成重复内容的可能性越低。
* **频率惩罚：** 对过于频繁出现的词或短语施加惩罚，通过降低这些词的生成概率。随着参数值的增加，对频繁出现的词或短语施加更大的惩罚。较高的参数值会减少这些词的出现频率，从而增加文本的词汇多样性。

如果你不理解这些参数是什么，可以选择**加载预设**，从创意、平衡、精确三种预设中选择。

<figure><img src="../../../.gitbook/assets/截屏2024-10-18 11.08.59.png" alt="" width="365"><figcaption></figcaption></figure>

***

### 高级功能

**记忆：** 开启记忆后问题分类器的每次输入将包含对话中的聊天历史，以帮助 LLM 理解上文，提高对话交互中的问题理解能力。

**记忆窗口：** 记忆窗口关闭时，系统会根据模型上下文窗口动态过滤聊天历史的传递数量；打开时用户可以精确控制聊天历史的传递数量（对数）。

**对话角色名设置：** 由于模型在训练阶段的差异，不同模型对于角色名的指令遵循程度不同，如 Human/Assistant，Human/AI，人类/助手等等。为适配多模型的提示响应效果，系统提供了对话角色名的设置，修改对话角色名将会修改会话历史的角色前缀。

**Jinja-2 模板：** LLM 的提示词编辑器内支持 Jinja-2 模板语言，允许你借助 Jinja2 这一强大的 Python 模板语言，实现轻量级数据转换和逻辑处理，参考[官方文档](https://jinja.palletsprojects.com/en/3.1.x/templates/)。

**错误重试**：针对节点发生的部分异常情况，通常情况下再次重试运行节点即可解决。开启错误重试功能后，节点将在发生错误的时候按照预设策略进行自动重试。你可以调整最大重试次数和每次重试间隔以设置重试策略。

- 最大重试次数为 10 次
- 最大重试间隔时间为 5000 ms

![](https://assets-docs.dify.ai/2024/12/dfb43c1cbbf02cdd36f7d20973a5529b.png)

**异常处理**：提供多样化的节点错误处理策略，能够在当前节点发生错误时抛出故障信息而不中断主流程；或通过备用路径继续完成任务。详细说明请参考[异常处理](https://docs.dify.ai/guides/workflow/error-handling)。

***

### 使用案例

* **读取知识库内容**

想要让工作流应用具备读取 [“知识库”](../../knowledge-base/) 内容的能力，例如搭建智能客服应用，请参考以下步骤：

1. 在 LLM 节点上游添加知识库检索节点；
2. 将知识检索节点的 **输出变量** `result` 填写至 LLM 节点中的 **上下文变量** 内；
3. 将 **上下文变量** 插入至应用提示词内，赋予 LLM 读取知识库内的文本能力。

<figure><img src="../../../.gitbook/assets/image (256).png" alt=""><figcaption><p>上下文变量</p></figcaption></figure>

[知识检索节点](knowledge-retrieval.md)输出的变量 `result` 还包含了分段引用信息，你可以通过 [**引用与归属**](../../knowledge-base/retrieval-test-and-citation.md#id-2-yin-yong-yu-gui-shu) 功能查看信息来源。

{% hint style="info" %}
上游节点的普通变量同样可以填写至上下文变量内，例如开始节点的字符串类型变量，但 **引用与归属** 功能将会失效。
{% endhint %}

* **读取文档文件**

想要让工作流应用具备读取读取文档内容的能力，例如搭建 ChatPDF 应用，可以参考以下步骤：

* 在 “开始” 节点内添加文件变量；
* 在 LLM 节点上游添加文档提取器节点，将文件变量作为输入变量；
* 将文档提取器节点的 **输出变量** `text` 填写至 LLM 节点中的提示词内。

如需了解更多，请参考[文件上传](../file-upload.md)。

<figure><img src="../../../.gitbook/assets/image (2) (2).png" alt=""><figcaption><p>填写系统提示词</p></figcaption></figure>

* **异常处理**

LLM 节点处理信息时有可能会遇到输入文本超过 Token 限制，未填写关键参数等错误。应用开发者可以参考以下步骤配置异常分支，在节点出现异常时启用应对方案，而避免中断整个流程。

1. 在 LLM 节点启用 “异常处理”
2. 选择异常处理方案并进行配置

如需了解更多应对异常的处理办法，请参考[异常处理](https://docs.dify.ai/guides/workflow/error-handling)。

![Error handling](https://assets-docs.dify.ai/2024/12/48c666959a491aa87c2232c444794dc5.png)



