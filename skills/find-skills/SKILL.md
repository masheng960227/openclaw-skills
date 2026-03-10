# Find Skills 技能 (技能搜索与发现)

## 概述
帮助用户高效搜索、发现和评估 OpenClaw 技能，快速找到所需能力。

## 核心功能

### 1️⃣ 技能搜索

#### 本地技能搜索
```bash
# 搜索已安装的技能
skill_search query="关键词" scope="self"

# 搜索公开技能
skill_search query="关键词" scope="public"

# 混合搜索（默认）
skill_search query="关键词" scope="mix"
```

#### ClawHub 搜索
```bash
# 访问 ClawHub 网站
https://clawhub.ai/

# 搜索技能
https://clawhub.ai/search?q=关键词

# 浏览分类
https://clawhub.ai/categories
```

#### GitHub 搜索
```bash
# 搜索 GitHub 上的 OpenClaw 技能
https://github.com/search?q=openclaw+skill+关键词

# 搜索特定组织的技能
https://github.com/search?q=org:openclaw+skill
```

### 2️⃣ 技能评估

#### 安全检查清单
- [ ] 代码审查（是否有恶意代码）
- [ ] 权限检查（是否请求过多权限）
- [ ] 依赖检查（是否有可疑依赖）
- [ ] 社区评价（评论和评分）
- [ ] 维护状态（最后更新时间）

#### 质量评估
| 指标 | 检查项 | 权重 |
|------|--------|------|
| 文档完整性 | 有 README、示例、API 文档 | 20% |
| 代码质量 | 有注释、结构清晰 | 20% |
| 测试覆盖 | 有测试用例 | 15% |
| 社区反馈 | 好评率、下载量 | 25% |
| 维护活跃 | 更新频率、Issue 响应 | 20% |

### 3️⃣ 技能分类

#### 按功能分类
| 类别 | 示例技能 |
|------|----------|
| 🔍 搜索类 | web-search, memory-search, skill-search |
| 📝 写作类 | humanizer, content-generator, copywriter |
| 💻 编程类 | code-review, debugger, git-workflows |
| 📊 数据类 | data-analysis, visualization, excel-tools |
| 🤖 自动化类 | workflow-automation, cron-manager |
| 🎨 设计类 | image-generator, ui-designer |
| 📱 社交类 | social-media-manager, scheduler |
| 🔧 工具类 | file-manager, converter, formatter |

#### 按复杂度分类
| 级别 | 特征 | 适合人群 |
|------|------|----------|
| 🟢 初级 | 单一功能、易上手 | 新手 |
| 🟡 中级 | 多功能、需配置 | 进阶用户 |
| 🔴 高级 | 复杂工作流、需定制 | 专家 |

### 4️⃣ 技能安装

#### 从本地安装
```bash
# 技能已在 ~/openclaw/skills/ 目录
skill_install skillId="skill-name"
```

#### 从 ClawHub 安装
```bash
# 1. 访问技能页面
# 2. 下载 SKILL.md
# 3. 放到 ~/openclaw/skills/skill-name/
# 4. 安装
skill_install skillId="skill-name"
```

#### 从 GitHub 安装
```bash
# 1. Clone 或下载技能
git clone https://github.com/user/skill-name.git ~/openclaw/skills/skill-name

# 2. 安装
skill_install skillId="skill-name"
```

### 5️⃣ 技能管理

#### 列出已安装技能
```bash
# 查看技能目录
ls -la ~/openclaw/skills/

# 查看技能详情
cat ~/openclaw/skills/skill-name/SKILL.md
```

#### 更新技能
```bash
# 1. 检查更新
# 2. 下载新版本
# 3. 替换旧版本
# 4. 重新安装
skill_install skillId="skill-name"
```

#### 卸载技能
```bash
# 删除技能目录
rm -rf ~/openclaw/skills/skill-name

# 注意：某些技能可能需要额外清理
```

## 搜索策略

### 策略 1: 关键词搜索
```
明确需求 → 提取关键词 → 多轮搜索 → 对比结果

示例：
需求："帮我写一个能自动搜索网页的工具"
关键词：["web", "search", "automation"]
搜索：skill_search query="web search automation"
```

### 策略 2: 分类浏览
```
确定类别 → 浏览分类 → 筛选比较 → 选择最佳

示例：
类别：编程辅助
浏览：~/openclaw/skills/ 或 ClawHub 编程类
筛选：查看评分、下载量、更新时间
```

### 策略 3: 社区推荐
```
查看热门 → 阅读评论 → 询问社区 → 验证效果

示例：
热门：ClawHub 排行榜
评论：查看用户反馈
社区：Discord/论坛询问
```

### 策略 4: 需求匹配
```
分析需求 → 列出功能 → 匹配技能 → 验证适用性

需求分析模板：
- 主要任务：[描述]
- 输入：[数据/文件类型]
- 输出：[期望结果]
- 约束：[时间/资源限制]
- 集成：[需要配合的工具]
```

## 实践工作流

### 工作流 1: 快速查找技能
```bash
# 1. 明确需求
需求 = "需要一个能帮我搜索记忆的技能"

# 2. 本地搜索
skill_search query="memory search" scope="self"

# 3. 如无结果，搜索公开技能
skill_search query="memory search" scope="public"

# 4. 如无结果，搜索 ClawHub
# 访问 https://clawhub.ai/search?q=memory+search

# 5. 评估并安装
skill_install skillId="memory-tools"
```

### 工作流 2: 技能对比
```bash
# 找到多个候选技能后

对比维度：
1. 功能覆盖度 - 是否满足所有需求
2. 易用性 - 配置和使用难度
3. 性能 - 执行速度和资源消耗
4. 兼容性 - 与其他技能的配合
5. 维护状态 - 更新频率和社区支持

决策矩阵：
技能 A: 功能★★★★ 易用★★★ 性能★★★★ 维护★★★
技能 B: 功能★★★ 易用★★★★ 性能★★★ 维护★★★★
选择：根据优先级决定
```

### 工作流 3: 技能验证
```bash
# 安装后验证

# 1. 阅读文档
cat ~/openclaw/skills/skill-name/SKILL.md

# 2. 检查依赖
# 查看是否有外部依赖需要安装

# 3. 测试基本功能
# 运行简单测试用例

# 4. 检查日志
# 查看是否有错误或警告

# 5. 评估效果
# 是否达到预期目标
```

## 搜索技巧

### 技巧 1: 同义词扩展
```
原始词：search
扩展：["search", "find", "lookup", "query", "discover"]

搜索时尝试多个同义词提高命中率
```

### 技巧 2: 组合搜索
```
单一搜索：skill_search query="web"
组合搜索：skill_search query="web search automation"

组合更精准，但可能漏掉相关技能
建议：先组合后单一
```

### 技巧 3: 中英文混合
```
中文搜索：skill_search query="网页搜索"
英文搜索：skill_search query="web search"

很多技能使用英文命名，建议都试试
```

### 技巧 4: 模糊匹配
```
精确：skill_search query="memory-search"
模糊：skill_search query="memory"

模糊搜索可能找到更多相关技能
```

## 技能评估模板

### 评估报告
```markdown
# 技能评估报告

## 基本信息
- 名称：[skill-name]
- 版本：[version]
- 来源：[ClawHub/GitHub/本地]
- 作者：[author]

## 功能评估
- 核心功能：[描述]
- 附加功能：[描述]
- 功能完整性：★★★★☆

## 质量评估
- 文档质量：★★★★☆
- 代码质量：★★★★☆
- 测试覆盖：★★★☆☆

## 安全评估
- 代码审查：[通过/需注意]
- 权限检查：[最小/适中/过多]
- 依赖检查：[安全/需注意]

## 社区评价
- 评分：[4.5/5]
- 下载量：[1000+]
- 评论：[正面/中性/负面]

## 推荐指数
综合评分：★★★★☆
推荐程度：强烈推荐/推荐/谨慎使用/不推荐

## 使用建议
[具体使用建议和注意事项]
```

## 常见问题

### Q1: 找不到需要的技能怎么办？
```
A: 
1. 尝试不同关键词和同义词
2. 搜索更宽泛的类别
3. 在 ClawHub 社区提问
4. 考虑自己创建技能
```

### Q2: 如何判断技能是否安全？
```
A:
1. 检查代码（特别是网络请求和文件操作）
2. 查看权限请求是否合理
3. 阅读用户评论和反馈
4. 在隔离环境先测试
5. 检查作者信誉
```

### Q3: 技能冲突怎么办？
```
A:
1. 检查技能依赖是否冲突
2. 查看是否有相同功能的技能
3. 尝试禁用其中一个
4. 联系技能作者寻求支持
```

### Q4: 技能更新后不兼容怎么办？
```
A:
1. 查看更新日志了解变更
2. 回退到旧版本
3. 调整配置适应新版本
4. 联系作者反馈问题
```

## 工具命令速查

```bash
# 搜索技能
skill_search query="关键词" scope="mix"

# 安装技能
skill_install skillId="skill-name"

# 查看技能目录
ls -la ~/openclaw/skills/

# 查看技能详情
cat ~/openclaw/skills/skill-name/SKILL.md

# 发布技能（自己的）
skill_publish skillId="skill-name"

# 取消发布
skill_unpublish skillId="skill-name"
```

## 资源链接

| 资源 | 链接 |
|------|------|
| ClawHub | https://clawhub.ai/ |
| OpenClaw 文档 | https://docs.openclaw.ai/ |
| GitHub 技能搜索 | https://github.com/search?q=openclaw+skill |
| 技能开发指南 | ~/openclaw/docs/skill-development.md |

## 最佳实践

1. **先搜索后创建**: 避免重复造轮子
2. **评估再安装**: 检查安全性和质量
3. **小步测试**: 先测试基本功能
4. **记录使用**: 记录哪些技能好用
5. **社区贡献**: 分享自己创建的技能

---

**提示**: 定期浏览 ClawHub 发现新技能，保持技能库更新！
