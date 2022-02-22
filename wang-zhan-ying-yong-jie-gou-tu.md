# 网站应用结构图
<script src="https://cdn.jsdelivr.net/npm/mermaid/dist/mermaid.min.js"></script>
<script>mermaid.initialize({startOnLoad:true});
</script>
```mermaid
graph TD
A1[用户1] -->|访问| B(负载均衡)
A2[用户2] -->|访问| B(负载均衡)
B -->|条件C1| D[模块D]
B -->|条件C2| E[模块E]
B -->|条件C3| F[模块F]
```
