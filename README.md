# OpenClaw Skills Collection 🦞

我的 OpenClaw 技能合集 - 精心打造的 AI 助手能力扩展

---

## 📚 技能列表

### 🤝 协作与任务管理

| 技能 | 说明 |
|------|------|
| **collaboration-basic** | 基础多代理协作，子代理管理和任务分配 |
| **advanced-delegation** | 高级任务分配，智能拆分和负载均衡 |
| **result-integration** | 结果整合，输出标准化和质量检查 |
| **communication-protocol** | 多代理通信协议，消息格式和错误处理 |

### 🔍 搜索与信息查询

| 技能 | 说明 |
|------|------|
| **tavily-search** | Tavily AI 搜索引擎，智能网页搜索 |
| **find-skills** | 技能搜索与发现，快速找到所需能力 |
| **weather** | 天气查询，实时天气和预报 |
| **memos-memory-guide** | MemOS 本地记忆系统，搜索和使用历史对话 |

### 💻 开发与工具

| 技能 | 说明 |
|------|------|
| **git-fundamentals** | Git 版本控制基础，从入门到进阶 |
| **github-management** | GitHub 仓库管理，PR/Issue/Actions |
| **humanizer** | 文本人性化，AI 文本转人类风格 |

---

## 🚀 快速开始

### 安装技能

```bash
# 克隆技能仓库
git clone https://github.com/masheng960227/openclaw-skills.git

# 复制技能到 OpenClaw 目录
cp -r openclaw-skills/skills/* ~/openclaw/skills/

# 重启 OpenClaw
openclaw gateway restart
```

### 使用技能

技能安装后会自动加载，可以通过对话调用：

```
帮我搜索最新的 AI 新闻          # 使用 tavily-search
北京今天天气怎么样              # 使用 weather
教我 Git 基础命令               # 使用 git-fundamentals
把这段文字改得更自然            # 使用 humanizer
```

---

## 📖 技能详情

### collaboration-basic
基础的多代理协作技能，支持：
- 子代理管理（创建、监控、控制）
- 任务分配（并行模式、流水线模式）
- 通信机制（会话间消息、子代理控制）
- 结果整合

### advanced-delegation
智能任务拆分与分配：
- 任务分析方法（复杂度、依赖性）
- 分配算法（负载均衡、能力匹配）
- 动态调整（进度监控、资源重分配）
- 实践模板（研究、创作、开发）

### result-integration
高效整合多代理输出：
- 输出标准化（格式模板、数据结构）
- 质量检查（清单、冲突解决）
- 整合方法（直接合并、智能融合、分层整合）

### tavily-search
Tavily AI 搜索引擎：
- 基础搜索和高级选项
- 内容提取
- 搜索模式（通用、新闻、研究）
- 实践模板

### weather
天气查询服务：
- 实时天气（温度、湿度、风速）
- 天气预报（逐小时、每日）
- 生活指数（穿衣、运动、洗车）
- 多城市对比

### git-fundamentals
Git 版本控制完整指南：
- 基础命令（配置、初始化、状态）
- 文件操作（add、commit、delete）
- 分支管理（创建、切换、合并）
- 远程仓库（push、pull、fetch）
- 撤销操作（restore、reset、revert）
- 高级操作（rebase、reflog、stash）

### github-management
GitHub 仓库管理：
- 仓库创建和配置
- 分支策略
- Pull Request 流程
- Issue 管理
- GitHub Actions (CI/CD)
- 版本发布

### humanizer
AI 文本人性化：
- 风格转换（正式→自然）
- 人性化技巧（句式、情感、个人化）
- AI 检测规避
- 程度控制（0-100%）

### memos-memory-guide
MemOS 本地记忆系统：
- 自动记忆召回
- 长期对话搜索
- 任务上下文恢复
- 技能指南复用

---

## 🎯 使用场景

### 个人效率
- 📰 每日天气简报
- 🔍 信息搜索和整理
- 📝 内容创作和润色

### 团队协作
- 🤖 多代理并行任务
- 📊 大规模数据处理
- 💻 代码审查和测试

### 开发工作流
- 🐙 GitHub 仓库管理
- 📦 版本控制和发布
- ⚙️ CI/CD 自动化

---

## 📁 目录结构

```
openclaw-skills/
├── README.md
└── skills/
    ├── collaboration-basic/
    │   └── SKILL.md
    ├── advanced-delegation/
    │   └── SKILL.md
    ├── result-integration/
    │   └── SKILL.md
    ├── communication-protocol/
    │   └── SKILL.md
    ├── tavily-search/
    │   └── SKILL.md
    ├── find-skills/
    │   └── SKILL.md
    ├── weather/
    │   └── SKILL.md
    ├── git-fundamentals/
    │   └── SKILL.md
    ├── github-management/
    │   └── SKILL.md
    └── humanizer/
        └── SKILL.md
```

---

## 🛠️ 技能开发

### 创建新技能

1. 创建技能目录
```bash
mkdir -p ~/openclaw/skills/my-skill
```

2. 编写 SKILL.md
```markdown
# My Skill Name

## 概述
技能描述...

## 核心功能
- 功能 1
- 功能 2

## 使用方法
...
```

3. 安装技能
```bash
skill_install skillId="my-skill"
```

### 技能模板

```markdown
# 技能名称

## 概述
简要描述技能功能

## 核心功能
列出主要功能

## 使用方法
提供使用示例

## 配置说明
如需配置，说明步骤

## 最佳实践
使用建议和注意事项
```

---

## 📊 技能统计

| 类别 | 数量 |
|------|------|
| 协作管理 | 4 |
| 搜索查询 | 3 |
| 开发工具 | 3 |
| 记忆系统 | 1 |
| **总计** | **11** |

---

## 🔄 更新日志

### v1.1.0 (2026-03-10)
- ✨ 添加 memos-memory-guide 记忆系统技能
- 📦 技能总数达到 11 个

### v1.0.0 (2026-03-10)
- ✨ 初始版本
- 📦 包含 10 个核心技能
- 🎯 覆盖协作、搜索、开发三大领域

---

## 📝 许可证

MIT License - 可自由使用和修改

---

## 🙏 致谢

感谢 OpenClaw 团队提供的强大平台！

---

## 📬 联系方式

- GitHub: [@masheng960227](https://github.com/masheng960227)
- 仓库：[openclaw-skills](https://github.com/masheng960227/openclaw-skills)

---

**Happy Coding! 🚀**
