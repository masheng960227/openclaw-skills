# 合作技能 (Collaboration Basic)

## 概述
基础的多代理协作技能，支持任务分配、并行执行和结果整合。

## 核心功能

### 1. 子代理管理
- 列出可用子代理：`subagents action=list`
- 生成新子代理：`sessions_spawn runtime=subagent`
- 控制子代理：`subagents action=steer/kill`

### 2. 任务分配模式

#### 并行模式
```bash
# 同时启动多个子代理处理不同任务
sessions_spawn task="任务 A" label="worker-1" mode="run"
sessions_spawn task="任务 B" label="worker-2" mode="run"
```

#### 流水线模式
```bash
# 子代理 1 处理 -> 结果传递给子代理 2
sessions_spawn task="第一步" label="stage-1" mode="run"
# 等待完成后
sessions_spawn task="第二步：使用第一步结果" label="stage-2" mode="run"
```

### 3. 通信机制

#### 会话间消息
```bash
# 发送消息到另一个会话
sessions_send sessionKey="agent:main:worker-1" message="请处理这个数据"
```

#### 子代理控制
```bash
# 指导子代理
subagents action=steer target="worker-1" message="调整方向：优先处理重要部分"
```

### 4. 结果整合

#### 收集子代理结果
```bash
# 列出所有子代理
subagents action=list

# 获取每个子代理的会话历史
sessions_history sessionKey="agent:main:worker-1" limit=50
```

## 最佳实践

### 任务拆分原则
1. **独立性**：子任务应尽可能独立
2. **明确性**：每个子任务有清晰的输入输出
3. **可合并**：结果易于整合

### 资源管理
- 控制并发子代理数量（建议 ≤5）
- 设置合理的超时时间
- 及时清理完成的子代理

### 错误处理
- 每个子代理设置超时
- 监控子代理状态
- 准备回退方案

## 示例工作流

```bash
# 1. 分析任务，拆分为子任务
# 2. 启动子代理
sessions_spawn task="研究主题 A" label="researcher-A" mode="run"
sessions_spawn task="研究主题 B" label="researcher-B" mode="run"

# 3. 监控进度
subagents action=list

# 4. 收集结果
sessions_history sessionKey="agent:main:researcher-A"
sessions_history sessionKey="agent:main:researcher-B"

# 5. 整合输出
# (主代理负责整合)

# 6. 清理
subagents action=kill target="researcher-A"
subagents action=kill target="researcher-B"
```

## 适用场景

- ✅ 多主题并行研究
- ✅ 大规模数据处理
- ✅ 多步骤工作流
- ✅ 代码审查/测试
- ❌ 需要深度上下文的任务
- ❌ 高度依赖主观判断的任务

## 限制

- 子代理继承主代理的工作空间
- 子代理之间不直接通信（需通过主代理）
- 每个子代理消耗独立的 token 配额
