# 高级任务分配技能 (Advanced Task Delegation)

## 概述
智能任务拆分与分配策略，优化多代理协作效率。

## 核心策略

### 1. 任务分析方法

#### 复杂度评估
```
高复杂度任务 → 拆分为多个中等复杂度子任务
低复杂度任务 → 直接分配给单个代理
```

#### 依赖性分析
```
独立任务 → 并行执行
依赖任务 → 流水线执行
混合任务 → 混合模式
```

### 2. 分配算法

#### 负载均衡
```bash
# 根据任务量动态分配
任务量 / 代理数 = 每个代理的工作量
```

#### 能力匹配
```
研究型任务 → 擅长分析的代理
创作型任务 → 擅长写作的代理
技术型任务 → 擅长编程的代理
```

### 3. 动态调整

#### 进度监控
```bash
# 定期检查子代理状态
subagents action=list

# 查看具体进度
sessions_history sessionKey="agent:main:worker-X" limit=20
```

#### 资源重分配
```bash
# 如果某个代理落后
subagents action=steer target="slow-worker" message="简化输出，优先完成核心部分"

# 如果某个代理提前完成
sessions_spawn task="帮助 slow-worker 完成剩余部分" label="helper" mode="run"
```

## 实践模板

### 模板 1: 研究项目
```bash
# 拆分研究任务
sessions_spawn task="收集背景资料" label="researcher-background" mode="run"
sessions_spawn task="分析最新趋势" label="researcher-trends" mode="run"
sessions_spawn task="整理案例研究" label="researcher-cases" mode="run"

# 等待完成后整合
# (主代理负责汇总)
```

### 模板 2: 内容创作
```bash
# 并行创作不同章节
sessions_spawn task="撰写第 1 章" label="writer-ch1" mode="run"
sessions_spawn task="撰写第 2 章" label="writer-ch2" mode="run"
sessions_spawn task="撰写第 3 章" label="writer-ch3" mode="run"

# 统一风格审核
sessions_spawn task="审核所有章节的风格一致性" label="editor-style" mode="run"
```

### 模板 3: 代码开发
```bash
# 功能模块并行开发
sessions_spawn task="实现用户认证模块" label="dev-auth" mode="run"
sessions_spawn task="实现数据接口模块" label="dev-api" mode="run"
sessions_spawn task="实现前端界面模块" label="dev-ui" mode="run"

# 集成测试
sessions_spawn task="集成测试和 bug 修复" label="test-integration" mode="run"
```

## 决策树

```
开始
├─ 任务是否可拆分？
│  ├─ 否 → 单个代理处理
│  └─ 是 → 继续分析
│
├─ 子任务是否独立？
│  ├─ 是 → 并行模式
│  └─ 否 → 继续分析
│
├─ 依赖关系复杂度？
│  ├─ 简单线性 → 流水线模式
│  └─ 复杂网状 → 混合模式 + 主代理协调
│
└─ 预计耗时？
   ├─ < 5 分钟 → 直接执行
   ├─ 5-30 分钟 → 子代理执行
   └─ > 30 分钟 → 多层子代理 + 定期检查点
```

## 常见陷阱与解决方案

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| 子代理输出不一致 | 指令模糊 | 提供详细模板和示例 |
| 进度差异大 | 任务难度不均 | 动态调整任务分配 |
| 结果难以整合 | 缺少统一标准 | 提前定义输出格式 |
| 资源浪费 | 子代理过多 | 限制并发数，分批处理 |

## 性能优化

### 批处理
```bash
# 大量相似任务 → 分批处理
# 每批 3-5 个子代理
for batch in batches:
    spawn_subagents(batch)
    wait_for_completion()
    process_results()
```

### 缓存复用
```bash
# 共享上下文减少重复工作
# 使用 attachAs 挂载共享数据
sessions_spawn task="处理数据" attachAs={"mountPath": "/shared"} mode="run"
```

## 评估指标

- **效率提升**: (串行时间 - 并行时间) / 串行时间
- **质量一致性**: 子代理输出质量方差
- **资源利用率**: 实际 token 使用 / 预算 token
- **完成时间**: 从开始到整合完成的总时间
