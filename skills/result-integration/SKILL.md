# 结果整合技能 (Result Integration)

## 概述
高效整合多代理输出，确保一致性和完整性。

## 整合策略

### 1. 输出标准化

#### 统一格式模板
```markdown
# [任务名称]

## 执行摘要
- 核心发现/成果
- 关键数据点

## 详细内容
[具体内容]

## 附件/参考
[相关链接或文件]
```

#### 数据结构化
```json
{
  "task": "任务名称",
  "agent": "执行代理",
  "status": "完成状态",
  "output": "主要输出",
  "metrics": {"质量": "评分", "耗时": "分钟"},
  "dependencies": ["依赖的任务"]
}
```

### 2. 质量检查清单

- [ ] 格式一致性
- [ ] 数据准确性
- [ ] 逻辑连贯性
- [ ] 无重复内容
- [ ] 无矛盾信息
- [ ] 完整性验证

### 3. 冲突解决

#### 信息冲突
```
发现冲突 → 追溯来源 → 验证准确性 → 选择可靠版本 → 记录决策
```

#### 风格冲突
```
风格不一致 → 确定主风格 → 统一调整 → 保留特色内容
```

## 整合方法

### 方法 1: 直接合并
适用：独立模块、无重叠内容

```bash
# 收集所有输出
output1 = get_result("agent-1")
output2 = get_result("agent-2")
output3 = get_result("agent-3")

# 按顺序合并
final = output1 + "\n\n" + output2 + "\n\n" + output3
```

### 方法 2: 智能融合
适用：有重叠、需要去重

```bash
# 提取关键点
points1 = extract_key_points(output1)
points2 = extract_key_points(output2)

# 合并去重
merged = deduplicate_merge(points1, points2)

# 重新组织
final = reorganize(merged)
```

### 方法 3: 分层整合
适用：复杂项目、多层级内容

```
Level 1: 执行摘要 (主代理编写)
Level 2: 各模块详情 (子代理输出)
Level 3: 附录数据 (原始输出)
```

## 实践工作流

### 工作流 1: 研究报告整合
```bash
# 1. 收集各子代理报告
reports = []
for agent in ["researcher-1", "researcher-2", "researcher-3"]:
    history = sessions_history(sessionKey=f"agent:main:{agent}")
    reports.append(extract_content(history))

# 2. 提取关键发现
findings = []
for report in reports:
    findings.extend(extract_findings(report))

# 3. 去重和分类
unique_findings = categorize_deduplicate(findings)

# 4. 编写综合报告
final_report = write_report(
    executive_summary=summarize(unique_findings),
    sections=organize_by_topic(unique_findings),
    appendices=reports  # 原始报告作为附录
)
```

### 工作流 2: 代码整合
```bash
# 1. 收集各模块代码
modules = {}
for agent in ["dev-auth", "dev-api", "dev-ui"]:
    code = get_code_from_session(f"agent:main:{agent}")
    modules[agent] = code

# 2. 检查接口兼容性
compatibility_issues = check_interfaces(modules)

# 3. 解决冲突
if compatibility_issues:
    fix_conflicts(modules, compatibility_issues)

# 4. 集成测试
test_results = run_integration_tests(modules)

# 5. 生成最终代码库
final_repo = assemble_repository(modules, test_results)
```

### 工作流 3: 内容创作整合
```bash
# 1. 收集各章节
chapters = []
for i in range(1, 4):
    chapter = get_chapter(f"writer-ch{i}")
    chapters.append(chapter)

# 2. 风格统一
style_guide = extract_style(chapters[0])
for chapter in chapters[1:]:
    chapter = align_style(chapter, style_guide)

# 3. 过渡优化
for i in range(len(chapters)-1):
    transition = create_transition(chapters[i], chapters[i+1])
    chapters[i] += transition

# 4. 整体审阅
final_manuscript = holistic_review(chapters)
```

## 工具函数

### 提取内容
```python
def extract_content(session_history):
    # 从会话历史中提取主要输出
    # 过滤掉工具调用和中间对话
    pass
```

### 去重合并
```python
def deduplicate_merge(items, similarity_threshold=0.8):
    # 使用语义相似度检测重复
    # 保留信息量最大的版本
    pass
```

### 质量评分
```python
def quality_score(content, criteria):
    # 根据预设标准评分
    # 返回 0-100 分数
    pass
```

## 常见挑战

| 挑战 | 解决方案 |
|------|----------|
| 输出格式不统一 | 提前提供模板，后期统一转换 |
| 信息重复 | 语义去重 + 人工审核 |
| 质量参差不齐 | 设置最低标准，不达标重做 |
| 逻辑不连贯 | 主代理负责过渡和连接 |
| 术语不一致 | 维护术语表，统一替换 |

## 最佳实践

1. **提前规划**: 在分配任务前定义输出格式
2. **中期检查**: 在 50% 进度时进行质量检查
3. **版本控制**: 保留各版本以便追溯
4. **文档化**: 记录整合决策和理由
5. **反馈循环**: 将整合问题反馈给子代理改进

## 自动化程度

| 任务类型 | 自动化比例 | 人工介入点 |
|----------|------------|------------|
| 数据汇总 | 90% | 异常值处理 |
| 文本合并 | 70% | 风格调整 |
| 代码集成 | 60% | 架构决策 |
| 创意内容 | 40% | 整体审阅 |
