# Agent

**Agent Strategy Overview**&#x20;

An Agent Strategy is an extensible template that defines standard input content and output formats. By developing specific Agent strategy interface functionality, you can implement various Agent strategies such as CoT (Chain of Thought) / ToT (Tree of Thought) / GoT (Graph of Thought) / BoT (Backbone of Thought), and achieve complex strategies like [Semantic Kernel](https://learn.microsoft.com/en-us/semantic-kernel/overview/).

### **Adding Fields in Manifest**&#x20;

To add Agent strategies in a plugin, add the `plugins.agent_strategies` field in the manifest.yaml file and define the Agent provider. Example code:

```yaml
version: 0.0.2
type: plugin
author: "langgenius"
name: "agent"
plugins:
  agent_strategies:
    - "provider/agent.yaml"
```

Some unrelated fields in the manifest file are omitted. For detailed Manifest format, refer to [Manifest](manifest.md).

### **Defining the Agent Provider**&#x20;

Create an agent.yaml file with basic Agent provider information:

```yaml
identity:
  author: langgenius
  name: agent
  label:
    en_US: Agent
    zh_Hans: Agent
    pt_BR: Agent
  description:
    en_US: Agent
    zh_Hans: Agent
    pt_BR: Agent
  icon: icon.svg
strategies:
  - strategies/function_calling.yaml
```

### **Defining and Implementing Agent Strategy**

#### **Definition**&#x20;

Create a function\_calling.yaml file to define the Agent strategy code:

```yaml
identity:
  name: function_calling
  author: Dify
  label:
    en_US: FunctionCalling
    zh_Hans: FunctionCalling
    pt_BR: FunctionCalling
description:
  en_US: Function Calling is a basic strategy for agent, model will use the tools provided to perform the task.
parameters:
  - name: model
    type: model-selector
    scope: tool-call&llm
    required: true
    label:
      en_US: Model
  - name: tools
    type: array[tools]
    required: true
    label:
      en_US: Tools list
  - name: query
    type: string
    required: true
    label:
      en_US: Query
  - name: max_iterations
    type: number
    required: false
    default: 5
    label:
      en_US: Max Iterations
    max: 50
    min: 1
extra:
  python:
    source: strategies/function_calling.py
```

The code format is similar to the [Tool](tool.md) standard format and defines four parameters: `model`, `tools`, `query`, and `max_iterations` to implement the most basic Agent strategy. This means that users can:

* Select which model to use
* Choose which tools to utilize
* Configure the maximum number of iterations
* Input a query to start executing the Agent

All these parameters work together to define how the Agent will process tasks and interact with the selected tools and models.

**Log**

To view the Agent's thinking process, besides normal message returns, you can use specialized interfaces to display the entire Agent thought process in a tree structure.

**Creating Logs**

* This interface creates and returns an `AgentLogMessage`, which represents a node in the log tree.
* If a parent is passed in, it indicates this node has a parent node.
* The default status is "Success". However, if you want to better display the task execution process, you can first set the status to "start" to show a "in progress" log, then update the log status to "Success" after the task is completed. This way, users can clearly see the entire process from start to finish.
* The label will be used as the log title shown to users.

```python
def create_log_message(
    self,
    label: str,
    data: Mapping[str, Any],
    status: AgentInvokeMessage.LogMessage.LogStatus = AgentInvokeMessage.LogMessage.LogStatus.SUCCESS,
    parent: AgentInvokeMessage | None = None,
) -> AgentInvokeMessage
```

**Completing Logs**

You can use the log completion endpoint to change the status if you previously set "start" as the initial status.

```python
def finish_log_message(
    self,
    log: AgentInvokeMessage,
    status: AgentInvokeMessage.LogMessage.LogStatus = AgentInvokeMessage.LogMessage.LogStatus.SUCCESS,
    error: Optional[str] = None,
) -> AgentInvokeMessage
```

**Example Implementation**

This example demonstrates a simple two-step execution process: first outputting a "Thinking" status log, then completing the actual task processing.

```python
class FunctionCallingAgentStrategy(AgentStrategy):
    def _invoke(self, parameters: dict[str, Any]) -> Generator[AgentInvokeMessage]:
        thinking_log = self.create_log_message(
            data={"Query": parameters.get("query")},
            label="Thinking",
            status=AgentInvokeMessage.LogMessage.LogStatus.START,
        )

        yield thinking_log

        llm_response = self.session.model.llm.invoke(
            model_config=LLMModelConfig(
                provider="openai",
                model="gpt-4o-mini",
                mode="chat",
                completion_params={},
            ),
            prompt_messages=[
                SystemPromptMessage(content="you are a helpful assistant"),
                UserPromptMessage(content=parameters.get("query")),
            ],
            stream=False,
            tools=[],
        )

        thinking_log = self.finish_log_message(log=thinking_log)
        yield thinking_log
        yield self.create_text_message(text=llm_response.message.content)
```
