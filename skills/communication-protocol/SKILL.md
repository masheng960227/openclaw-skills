# 多代理通信协议 (Multi-Agent Communication Protocol)

## 概述
标准化的代理间通信规范，确保高效准确的信息传递。

## 通信模式

### 1. 主从模式 (Hub-Spoke)
```
     主代理
    /  |  \
   /   |   \
代理 1  代理 2  代理 3
```
- **优点**: 集中控制、易于协调
- **缺点**: 主代理成为瓶颈
- **适用**: 复杂任务、需要强协调

### 2. 流水线模式 (Pipeline)
```
代理 1 → 代理 2 → 代理 3 → 输出
```
- **优点**: 清晰的数据流、责任明确
- **缺点**: 串行依赖、速度受限
- **适用**: 顺序依赖的任务

### 3. 广播模式 (Broadcast)
```
主代理 → [所有子代理]
```
- **优点**: 信息同步快
- **缺点**: 可能造成信息过载
- **适用**: 全局通知、统一指令

### 4. 点对点模式 (Peer-to-Peer)
```
代理 1 ↔ 代理 2
代理 2 ↔ 代理 3
```
- **优点**: 直接高效
- **缺点**: 需要代理间直接通信能力
- **适用**: 高度协作的子任务

## 消息格式标准

### 指令消息
```json
{
  "type": "instruction",
  "from": "main-agent",
  "to": "worker-1",
  "priority": "high|normal|low",
  "content": {
    "task": "具体任务描述",
    "context": "相关背景信息",
    "constraints": ["限制条件 1", "限制条件 2"],
    "deadline": "预期完成时间",
    "output_format": "期望的输出格式"
  },
  "timestamp": "ISO8601 时间戳"
}
```

### 状态更新
```json
{
  "type": "status_update",
  "from": "worker-1",
  "to": "main-agent",
  "content": {
    "progress": "0-100%",
    "current_activity": "正在进行的活动",
    "blockers": ["遇到的障碍"],
    "eta": "预计完成时间",
    "needs_help": true/false
  }
}
```

### 结果提交
```json
{
  "type": "result",
  "from": "worker-1",
  "to": "main-agent",
  "content": {
    "task_id": "任务标识",
    "status": "success|partial|failed",
    "output": "主要输出内容",
    "metadata": {
      "time_taken": "耗时",
      "quality_score": "自评分",
      "confidence": "置信度"
    },
    "attachments": ["相关文件"]
  }
}
```

### 请求协助
```json
{
  "type": "help_request",
  "from": "worker-1",
  "to": "main-agent",
  "content": {
    "issue": "遇到的问题",
    "attempted_solutions": ["已尝试的方案"],
    "suggested_help": ["建议的协助方式"],
    "urgency": "critical|high|normal"
  }
}
```

## 通信实践

### 实践 1: 清晰的任务指令
```bash
# ❌ 模糊指令
sessions_send sessionKey="agent:main:worker-1" message="研究一下 AI"

# ✅ 清晰指令
sessions_send sessionKey="agent:main:worker-1" message="""
任务：研究 2024 年 AI 发展趋势
范围：重点关注大语言模型和多模态 AI
输出格式：
  - 3-5 个关键趋势
  - 每个趋势配 1-2 个案例
  - 总字数 800-1000 字
截止时间：30 分钟内
"""
```

### 实践 2: 定期状态同步
```bash
# 主代理主动询问
subagents action=steer target="worker-1" message="请汇报当前进度（百分比）和是否遇到障碍"

# 子代理主动汇报
# (在任务进行到 25%、50%、75% 时主动发送状态)
```

### 实践 3: 高效的问题升级
```bash
# 子代理遇到问题时
# 1. 先尝试自主解决（设定尝试次数限制）
# 2. 记录已尝试的方案
# 3. 清晰描述问题
# 4. 提出具体需要的帮助

subagents action=steer target="main" message="""
【求助】数据获取失败
已尝试：
  1. 更换数据源
  2. 调整查询参数
需要：
  - 备选数据源建议
  - 或调整任务范围
"""
```

## 通信优化技巧

### 1. 批量通信
```bash
# ❌ 多次单独发送
send("更新 A")
send("更新 B")
send("更新 C")

# ✅ 一次批量发送
send("""
更新汇总：
- A: [内容]
- B: [内容]
- C: [内容]
""")
```

### 2. 优先级标记
```bash
# 紧急事项
subagents action=steer target="worker-1" message="[紧急] 请立即停止当前任务，处理..."

# 常规事项
subagents action=steer target="worker-1" message="[常规] 有空时请处理..."
```

### 3. 上下文保持
```bash
# 在长对话中保持上下文
# 每次通信包含必要背景
message = f"""
[任务背景：{task_context}]
[当前阶段：{current_stage}]
[具体问题：{specific_question}]
"""
```

## 错误处理协议

### 通信失败
```
检测到失败 → 重试 (最多 3 次) → 降级方案 → 记录日志
```

### 理解偏差
```
发现偏差 → 澄清确认 → 调整指令 → 继续执行
```

### 超时处理
```
超过预期时间 → 询问状态 → 评估继续/中止 → 资源回收
```

## 安全与隐私

### 信息隔离
- 敏感数据不通过子代理传递
- 子代理只能访问必要的工作空间部分
- 使用临时凭证而非永久凭证

### 审计日志
```bash
# 记录所有重要通信
log_communication(from, to, timestamp, summary)
```

## 性能指标

| 指标 | 目标 | 测量方法 |
|------|------|----------|
| 消息延迟 | < 5 秒 | 发送 - 接收时间差 |
| 指令清晰度 | > 90% 理解率 | 子代理确认率 |
| 状态更新频率 | 每 10 分钟 | 自动监控 |
| 问题解决时间 | < 15 分钟 | 从求助到解决 |

## 模板库

### 任务分配模板
```
【任务分配】
任务名称：{name}
背景：{context}
目标：{goal}
约束：{constraints}
输出要求：{output_format}
截止时间：{deadline}
确认收到请回复"收到"
```

### 进度询问模板
```
【进度检查】
任务：{task_name}
当前进度：{progress}%
是否遇到障碍：{yes/no}
如有障碍，请描述：{details}
预计完成时间：{eta}
```

### 结果收集模板
```
【结果提交】
任务：{task_name}
完成状态：{status}
主要输出：{output}
质量自评：{score}/100
备注：{notes}
```
