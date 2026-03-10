# Tavily Search 技能 (Tavily Web Search)

## 概述
使用 Tavily AI 搜索引擎进行智能网页搜索，专为 AI 助手优化的搜索结果。

## 什么是 Tavily

Tavily 是一个专为 AI 助手设计的搜索引擎 API，提供：
- 🎯 **精准结果**: AI 优化的搜索结果
- 📝 **内容提取**: 自动提取网页正文
- 🔒 **安全可靠**: 过滤低质量内容
- ⚡ **快速响应**: 优化的 API 性能

## 核心功能

### 1️⃣ 基础搜索

#### API 配置
```bash
# 获取 Tavily API Key
# 访问 https://app.tavily.com/ 注册获取

# 配置到环境变量
export TAVILY_API_KEY="your-api-key"

# 或在 OpenClaw 配置中
openclaw configure --section web
```

#### 基础搜索
```python
from tavily import TavilyClient

tavily = TavilyClient(api_key="your-api-key")

# 基础搜索
response = tavily.search(query="AI 发展趋势 2024")

# 结果
{
    "query": "AI 发展趋势 2024",
    "results": [
        {
            "title": "2024 年 AI 发展十大趋势",
            "url": "https://example.com/ai-trends-2024",
            "content": "文章内容摘要...",
            "score": 0.95
        }
    ]
}
```

### 2️⃣ 高级搜索选项

#### 搜索深度
```python
# 基础搜索（快速）
response = tavily.search(
    query="关键词",
    search_depth="basic"  # 或 "advanced"
)

# advanced: 更深入搜索，更多结果
```

#### 结果数量
```python
response = tavily.search(
    query="关键词",
    max_results=5  # 返回结果数量 (默认 5)
)
```

#### 内容长度
```python
response = tavily.search(
    query="关键词",
    max_tokens=1000  # 每个结果的最大 token 数
)
```

#### 包含图片
```python
response = tavily.search(
    query="关键词",
    include_images=True
)
```

#### 答案生成
```python
response = tavily.search(
    query="关键词",
    include_answer=True  # 包含 AI 生成的答案
)
```

### 3️⃣ 搜索模式

#### 通用搜索
```python
response = tavily.search(
    query="最新 AI 新闻",
    topic="general"
)
```

#### 新闻搜索
```python
response = tavily.search(
    query="科技新闻",
    topic="news"
)
```

#### 研究搜索
```python
response = tavily.search(
    query="机器学习论文",
    topic="research"
)
```

### 4️⃣ 内容提取

#### 提取网页内容
```python
from tavily import TavilyClient

tavily = TavilyClient(api_key="your-api-key")

# 提取特定 URL 内容
response = tavily.extract(
    urls=["https://example.com/article"]
)

# 结果
{
    "results": [
        {
            "url": "https://example.com/article",
            "content": "完整文章内容...",
            "title": "文章标题"
        }
    ]
}
```

#### 批量提取
```python
response = tavily.extract(
    urls=[
        "https://example.com/article1",
        "https://example.com/article2",
        "https://example.com/article3"
    ]
)
```

## 使用场景

### 场景 1: 实时信息搜索
```python
# 搜索最新新闻
response = tavily.search(
    query="2024 年 3 月 科技新闻",
    topic="news",
    max_results=10
)

# 整理结果
news_summary = []
for result in response['results']:
    news_summary.append(f"- {result['title']}: {result['content'][:200]}")

print("\n".join(news_summary))
```

### 场景 2: 研究资料收集
```python
# 深度研究搜索
response = tavily.search(
    query="机器学习 最新进展",
    search_depth="advanced",
    max_results=15,
    include_answer=True
)

# 包含 AI 生成的答案
print(f"AI 总结：{response['answer']}")

# 详细结果
for result in response['results']:
    print(f"\n{result['title']}")
    print(f"来源：{result['url']}")
    print(f"内容：{result['content']}")
```

### 场景 3: 竞品分析
```python
# 搜索竞品信息
competitors = ["公司 A", "公司 B", "公司 C"]

for competitor in competitors:
    response = tavily.search(
        query=f"{competitor} 最新动态",
        max_results=5
    )
    
    print(f"\n=== {competitor} ===")
    for result in response['results']:
        print(f"- {result['title']}")
```

### 场景 4: 事实核查
```python
# 验证信息
claim = "某公司发布了新产品"

response = tavily.search(
    query=f"{claim} 真实性",
    search_depth="advanced",
    max_results=10
)

# 交叉验证多个来源
sources = []
for result in response['results']:
    sources.append({
        'title': result['title'],
        'url': result['url'],
        'content': result['content']
    })

# 分析一致性
```

### 场景 5: 内容创作辅助
```python
# 收集素材
response = tavily.search(
    query="AI 写作技巧",
    max_results=10,
    include_answer=True
)

# 整理要点
key_points = []
for result in response['results']:
    # 提取关键信息
    key_points.append(result['content'])

# 用于创作参考
```

## 实践模板

### 模板 1: 新闻摘要
```python
def get_news_summary(topic, days=7):
    """获取指定主题的新闻摘要"""
    response = tavily.search(
        query=f"{topic} 最新新闻",
        topic="news",
        max_results=10
    )
    
    summary = f"# {topic} 新闻摘要\n\n"
    for i, result in enumerate(response['results'], 1):
        summary += f"## {i}. {result['title']}\n"
        summary += f"来源：{result['url']}\n"
        summary += f"摘要：{result['content'][:300]}...\n\n"
    
    return summary
```

### 模板 2: 研究报告
```python
def create_research_report(topic):
    """创建主题研究报告"""
    # 深度搜索
    response = tavily.search(
        query=f"{topic} 全面分析",
        search_depth="advanced",
        max_results=15,
        include_answer=True
    )
    
    report = f"# {topic} 研究报告\n\n"
    report += f"## AI 总结\n{response['answer']}\n\n"
    report += "## 详细资料\n\n"
    
    for result in response['results']:
        report += f"### {result['title']}\n"
        report += f"来源：{result['url']}\n"
        report += f"{result['content']}\n\n"
    
    return report
```

### 模板 3: 竞品监控
```python
def monitor_competitors(competitors_list):
    """监控竞品动态"""
    reports = {}
    
    for competitor in competitors_list:
        response = tavily.search(
            query=f"{competitor} 最新动态 产品 融资",
            max_results=5
        )
        
        reports[competitor] = {
            'updates': [
                {
                    'title': r['title'],
                    'url': r['url'],
                    'date': 'recent'
                }
                for r in response['results']
            ]
        }
    
    return reports
```

## 配置选项

### 完整配置示例
```python
response = tavily.search(
    query="你的搜索词",
    search_depth="advanced",    # basic 或 advanced
    topic="general",            # general, news, research
    max_results=10,             # 1-20
    max_tokens=1000,            # 每个结果的 token 数
    include_answer=True,        # 包含 AI 答案
    include_images=False,       # 包含图片
    include_raw_content=False,  # 包含原始 HTML
    days=7                      # 仅搜索最近 N 天
)
```

### 参数说明
| 参数 | 类型 | 说明 | 默认值 |
|------|------|------|--------|
| query | string | 搜索关键词 | 必填 |
| search_depth | string | 搜索深度 | "basic" |
| topic | string | 搜索主题 | "general" |
| max_results | int | 结果数量 | 5 |
| max_tokens | int | 每结果 token 数 | 1000 |
| include_answer | bool | 包含 AI 答案 | False |
| include_images | bool | 包含图片 | False |
| days | int | 最近 N 天 | 无限制 |

## 错误处理

### 常见错误
```python
from tavily import APIError, RateLimitError

try:
    response = tavily.search(query="关键词")
except APIError as e:
    print(f"API 错误：{e}")
except RateLimitError as e:
    print(f"请求超限：{e}")
except Exception as e:
    print(f"其他错误：{e}")
```

### 重试机制
```python
import time
from tavily import RateLimitError

def search_with_retry(query, max_retries=3):
    for i in range(max_retries):
        try:
            return tavily.search(query=query)
        except RateLimitError:
            if i < max_retries - 1:
                time.sleep(2 ** i)  # 指数退避
            else:
                raise
```

## 最佳实践

### 1. 优化搜索词
```
❌ 模糊：AI
✅ 精准：2024 年 AI 发展趋势 大语言模型

❌ 太长：我想知道关于人工智能在医疗领域的应用...
✅ 简洁：AI 医疗应用 2024
```

### 2. 合理使用搜索深度
```
日常查询 → basic (快速)
深入研究 → advanced (全面)
```

### 3. 结果验证
```python
# 交叉验证重要信息
response1 = tavily.search(query="声明内容")
response2 = tavily.search(query="声明内容 真实性")

# 比较多个来源
```

### 4. 缓存结果
```python
import hashlib
import json

cache = {}

def cached_search(query):
    key = hashlib.md5(query.encode()).hexdigest()
    if key in cache:
        return cache[key]
    
    result = tavily.search(query=query)
    cache[key] = result
    return result
```

## 定价信息

| 计划 | 价格 | 每月搜索次数 |
|------|------|-------------|
| Free | $0 | 1,000 次 |
| Starter | $29 | 10,000 次 |
| Pro | $99 | 100,000 次 |
| Enterprise | 定制 | 无限 |

*价格可能变动，请查阅官网最新信息*

## 替代方案

如果 Tavily 不可用，可以考虑：
- Google Search API
- Bing Search API
- DuckDuckGo API
- Serper API

## 安全注意事项

1. **API Key 保护**: 不要硬编码在代码中
2. **速率限制**: 遵守 API 调用限制
3. **内容审核**: 验证搜索结果的可靠性
4. **隐私保护**: 不搜索敏感个人信息

---

**提示**: Tavily 专为 AI 助手优化，搜索结果更适合 LLM 处理！
