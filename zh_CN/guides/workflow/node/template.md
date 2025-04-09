# 模板转换

### 定义

允许借助 Jinja2 的 Python 模板语言灵活地进行数据转换、文本处理等。

### 什么是 Jinja？

> Jinja is a fast, expressive, extensible templating engine.
>
> Jinja 是一个快速、表达力强、可扩展的模板引擎。

—— [https://jinja.palletsprojects.com/en/3.1.x/](https://jinja.palletsprojects.com/en/3.1.x/)

### 场景

模板节点允许你借助 Jinja2 这一强大的 Python 模板语言，在工作流内实现轻量、灵活的数据转换，适用于文本处理、JSON 转换等情景。例如灵活地格式化并合并来自前面步骤的变量，创建出单一的文本输出。这非常适合于将多个数据源的信息汇总成一个特定格式，满足后续步骤的需求。

**示例1：** 将多个输入（文章标题、介绍、内容）拼接为完整文本

<figure><img src="../../../.gitbook/assets/image (209).png" alt="" width="375"><figcaption><p>拼接文本</p></figcaption></figure>

**示例2：** 将知识检索节点获取的信息及其相关的元数据，整理成一个结构化的 Markdown 格式

```Plain
{% raw %}
{% for item in chunks %}
### Chunk {{ loop.index }}. 
### Similarity: {{ item.metadata.score | default('N/A') }}

#### {{ item.title }}

##### Content
{{ item.content | replace('\n', '\n\n') }}

---
{% endfor %}
{% endraw %}
```

<figure><img src="../../../.gitbook/assets/image (210).png" alt=""><figcaption><p>知识检索节点输出转换为 Markdown</p></figcaption></figure>

你可以参考 Jinja 的[官方文档](https://jinja.palletsprojects.com/en/3.1.x/templates/)，创建更为复杂的模板来执行各种任务。
