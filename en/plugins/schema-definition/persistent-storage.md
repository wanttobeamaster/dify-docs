# Persistent Storage

如果单独审视插件中的 Tool 及 Endpoint，不难发现大多数情况下其只能完成单轮交互，请求后返回数据，任务结束。

如果有需要长期储存的数据，如实现持久化的记忆，需要插件具备持久化存储能力。**持久化储机制能够让插件具备在相同 Workspace 持久存储数据的能力**，目前通过提供 KV 数据库满足存储需求，未来可能会根据实际的使用情况推出更灵活更强大的储存接口。

If you look at the Tool and Endpoint in the plug-in alone, it is not difficult to find that in most cases it can only complete a single round of interaction, the request returns the data, and the task ends.&#x20;

If there is a need for long-term storage of data, such as the implementation of persistent memory, the plug-in needs to have persistent storage capabilities. **Persistent storage mechanism allows plugins to have the ability to store data persistently in the same Workspace** , currently through the provision of KV database to meet the storage needs , the future may be based on the actual use of the introduction of more flexible and more powerful storage endpoints .

### Storage Key

#### **Entrance**

```python
    self.session.storage
```

#### Endpoints

```python
    def set(self, key: str, val: bytes) -> None:
        pass
```

You can notice that a bytes is passed in, so you can actually store files in it.

### Get Key

#### **Entrance**

```python
    self.session.storage
```

#### **Endpoint**

```python
    def get(self, key: str) -> bytes:
        pass
```

### Delete Key

#### **Entrance**

```python
    self.session.storage
```

#### **Endpoint**

```python
    def delete(self, key: str) -> None:
        pass
```

\
