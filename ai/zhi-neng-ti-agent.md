# 智能体(Agent)

#### 4种设计范式

1. Reflection反思链
2. ToolUse 调用工具/api/函数/生成代码
3. planning 规划 由AI去拆解规划流程，选择工具调用，输出结果。
4. Multi-agent（多智能体协作） 不同功能的智能体堆叠，嵌套。流程编排Workflows

#### 实际体验

> workflows的方式整体效率不高，不如插件的方式

workflows的方式，主要是解决复杂任务。非复杂任务不要往workflows上面靠。需要花费精力去做效率优化。
