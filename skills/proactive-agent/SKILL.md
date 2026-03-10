# Proactive Agent 技能 (主动代理)

## 概述
让 OpenClaw Agent 从被动响应变为主动发现并汇报问题的标准化框架。

## 核心功能

### 1️⃣ 主动监控
- 定期检查系统状态
- 监控关键指标
- 发现异常主动报告

### 2️⃣ 定时任务
- 按计划执行检查
- 自动生成报告
- 推送重要更新

### 3️⃣ 智能提醒
- 基于阈值告警
- 优先级分类
- 上下文相关通知

### 4️⃣ 自动化汇报
- 日报/周报生成
- 关键事件汇总
- 趋势分析

## 配置方法

### 基础配置
```json
{
  "proactive-agent": {
    "enabled": true,
    "checkInterval": 3600,
    "reportTime": "09:00",
    "channels": ["feishu", "email"],
    "thresholds": {
      "cpu": 80,
      "memory": 85,
      "disk": 90
    }
  }
}
```

### Cron 任务配置
```json
{
  "name": "系统健康检查",
  "schedule": {
    "kind": "cron",
    "expr": "0 */6 * * *"
  },
  "payload": {
    "kind": "agentTurn",
    "message": "检查系统健康状态并汇报"
  },
  "sessionTarget": "isolated"
}
```

## 使用场景

### 场景 1: 系统监控
```bash
# 每 6 小时检查一次
0 */6 * * * 检查 CPU、内存、磁盘使用率
```

### 场景 2: 日志分析
```bash
# 每天分析错误日志
0 9 * * * 分析昨日错误日志并汇总
```

### 场景 3: 数据备份检查
```bash
# 每周检查备份状态
0 10 * * 1 检查备份完整性
```

### 场景 4: API 健康检查
```bash
# 每 30 分钟检查 API 状态
*/30 * * * * 检查关键 API 可用性
```

## 主动汇报模板

### 日报模板
```markdown
# 系统日报 - {{date}}

## 概览
- 运行时间：{{uptime}}
- 整体状态：{{status}}

## 关键指标
| 指标 | 当前值 | 阈值 | 状态 |
|------|--------|------|------|
| CPU | {{cpu}}% | 80% | ✅ |
| 内存 | {{mem}}% | 85% | ⚠️ |
| 磁盘 | {{disk}}% | 90% | ✅ |

## 今日事件
- ✅ 正常事件列表
- ⚠️ 警告事件列表
- ❌ 错误事件列表

## 建议
- 需要关注的项
- 建议的操作
```

### 告警模板
```markdown
## ⚠️ 告警通知

**级别**: {{level}}
**时间**: {{timestamp}}
**来源**: {{source}}

**问题**: {{description}}

**当前值**: {{current_value}}
**阈值**: {{threshold}}

**建议操作**:
1. {{action1}}
2. {{action2}}
```

## 实现示例

### 示例 1: 健康检查
```bash
#!/bin/bash
# health-check.sh

# CPU 使用率
cpu=$(top -l 1 | grep "CPU usage" | awk '{print $3}' | cut -d'%' -f1)

# 内存使用率
mem=$(vm_stat | awk '/Pages active/ {print $3}' | cut -d'.' -f1)

# 磁盘使用率
disk=$(df -h / | awk 'NR==2 {print $5}' | cut -d'%' -f1)

# 检查阈值
if (( $(echo "$cpu > 80" | bc -l) )); then
    echo "⚠️ CPU 使用率过高：${cpu}%"
fi

if (( $(echo "$mem > 85" | bc -l) )); then
    echo "⚠️ 内存使用率过高：${mem}%"
fi

if (( $(echo "$disk > 90" | bc -l) )); then
    echo "⚠️ 磁盘空间不足：${disk}%"
fi
```

### 示例 2: 日志分析
```bash
#!/bin/bash
# log-analyzer.sh

# 统计错误数量
error_count=$(grep -c "ERROR" /var/log/app.log)

# 统计警告数量
warn_count=$(grep -c "WARN" /var/log/app.log)

# 提取最新错误
latest_errors=$(grep "ERROR" /var/log/app.log | tail -10)

# 生成报告
echo "# 日志分析报告"
echo "错误数：$error_count"
echo "警告数：$warn_count"
echo ""
echo "最新错误:"
echo "$latest_errors"
```

## 最佳实践

### 1. 合理设置检查频率
```
高频检查 (1-5 分钟): 关键服务可用性
中频检查 (15-60 分钟): 系统资源使用
低频检查 (1-24 小时): 日志分析、备份检查
```

### 2. 设置合理阈值
```
警告阈值：80% (提醒注意)
严重阈值：90% (需要立即处理)
```

### 3. 避免告警疲劳
```
- 合并相关告警
- 设置静默时间
- 分级通知策略
```

### 4. 保持上下文
```
- 记录历史数据
- 提供趋势分析
- 关联相关事件
```

## 集成方式

### 飞书通知
```bash
curl -X POST -H "Content-Type: application/json" \
  -d '{"msg_type":"interactive","card":{...}}' \
  "https://open.feishu.cn/open-apis/bot/v2/hook/YOUR_WEBHOOK"
```

### 邮件通知
```bash
echo "报告内容" | mail -s "系统日报" admin@example.com
```

### Slack 通知
```bash
curl -X POST -H 'Content-type: application/json' \
  --data '{"text":"告警内容"}' \
  https://hooks.slack.com/services/YOUR/WEBHOOK/URL
```

## 监控指标

### 系统指标
- CPU 使用率
- 内存使用率
- 磁盘空间
- 网络流量
- 进程数量

### 应用指标
- API 响应时间
- 错误率
- 请求量
- 队列长度
- 数据库连接数

### 业务指标
- 用户活跃度
- 交易量
- 转化率
- 异常行为

## 故障排除

### 问题 1: 告警过多
```
解决:
1. 调整阈值
2. 合并相似告警
3. 设置告警抑制
```

### 问题 2: 漏报
```
解决:
1. 检查监控覆盖
2. 增加检查频率
3. 添加冗余监控
```

### 问题 3: 误报
```
解决:
1. 校准阈值
2. 添加确认机制
3. 学习历史模式
```

## 扩展功能

### 自动修复
```bash
# 检测到服务停止自动重启
if ! pgrep -x "myservice" > /dev/null; then
    systemctl start myservice
    echo "服务已自动重启"
fi
```

### 趋势预测
```bash
# 基于历史数据预测磁盘满的时间
disk_usage=$(df / | awk 'NR==2 {print $5}' | cut -d'%' -f1)
days_left=$(( (100 - disk_usage) / 1 ))  # 假设每天增长 1%
echo "预计磁盘将在 $days_left 天后满"
```

### 根因分析
```bash
# 关联分析多个指标
if [ $cpu_high ] && [ $mem_high ]; then
    echo "可能存在内存泄漏导致 CPU 升高"
fi
```

---

**提示**: Proactive Agent 的核心是从"被动响应"转向"主动发现"，提前预防问题发生！
