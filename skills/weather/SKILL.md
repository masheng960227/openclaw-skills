# Weather 技能 (天气查询)

## 概述
提供实时天气查询、天气预报和气象数据服务。

## 核心功能

### 1️⃣ 实时天气查询
- 当前温度、湿度、风速
- 天气状况（晴、雨、雪等）
- 体感温度
- 空气质量指数

### 2️⃣ 天气预报
- 逐小时预报（24-48 小时）
- 每日预报（7-15 天）
- 降水概率
- 温度变化趋势

### 3️⃣ 气象警报
- 恶劣天气预警
- 空气质量警报
- 紫外线指数

### 4️⃣ 历史天气
- 历史天气数据查询
- 气候统计分析

## 数据源

### 推荐 API 服务

| 服务 | 免费额度 | 特点 |
|------|----------|------|
| OpenWeatherMap | 60 次/分钟 | 数据全面、稳定 |
| WeatherAPI | 100 万次/月 | 免费额度大 |
| AccuWeather | 50 次/天 | 预报准确 |
| 和风天气 | 免费 | 中国数据好 |
| 心知天气 | 免费 | 中文支持好 |

## 配置方法

### OpenWeatherMap 配置
```bash
# 1. 注册获取 API Key
# 访问 https://openweathermap.org/api

# 2. 配置环境变量
export OPENWEATHER_API_KEY="your-api-key"

# 3. 测试连接
curl "https://api.openweathermap.org/data/2.5/weather?q=Beijing&appid=your-api-key&units=metric&lang=zh_cn"
```

### WeatherAPI 配置
```bash
# 1. 注册获取 API Key
# 访问 https://www.weatherapi.com/

# 2. 配置环境变量
export WEATHER_API_KEY="your-api-key"
```

## API 使用示例

### OpenWeatherMap

#### 当前天气
```python
import requests

API_KEY = "your-api-key"
city = "Beijing"

url = f"https://api.openweathermap.org/data/2.5/weather"
params = {
    "q": city,
    "appid": API_KEY,
    "units": "metric",  # 摄氏度
    "lang": "zh_cn"     # 中文
}

response = requests.get(url, params=params)
data = response.json()

# 解析结果
print(f"城市：{data['name']}")
print(f"温度：{data['main']['temp']}°C")
print(f"体感：{data['main']['feels_like']}°C")
print(f"天气：{data['weather'][0]['description']}")
print(f"湿度：{data['main']['humidity']}%")
print(f"风速：{data['wind']['speed']} m/s")
```

#### 天气预报
```python
# 5 天预报（每 3 小时）
url = f"https://api.openweathermap.org/data/2.5/forecast"
params = {
    "q": city,
    "appid": API_KEY,
    "units": "metric",
    "lang": "zh_cn"
}

response = requests.get(url, params=params)
data = response.json()

# 解析预报
for item in data['list'][:8]:  # 前 24 小时
    dt = item['dt_txt']
    temp = item['main']['temp']
    desc = item['weather'][0]['description']
    print(f"{dt}: {temp}°C - {desc}")
```

#### 按坐标查询
```python
# 使用经纬度
params = {
    "lat": 39.9042,
    "lon": 116.4074,
    "appid": API_KEY,
    "units": "metric",
    "lang": "zh_cn"
}
```

### WeatherAPI

#### 当前天气
```python
import requests

API_KEY = "your-api-key"
city = "Beijing"

url = f"http://api.weatherapi.com/v1/current.json"
params = {
    "key": API_KEY,
    "q": city,
    "aqi": "yes",      # 空气质量
    "lang": "zh"       # 中文
}

response = requests.get(url, params=params)
data = response.json()

print(f"城市：{data['location']['name']}")
print(f"温度：{data['current']['temp_c']}°C")
print(f"天气：{data['current']['condition']['text']}")
print(f"湿度：{data['current']['humidity']}%")
print(f"风速：{data['current']['wind_kph']} km/h")
print(f"空气质量：{data['current']['air_quality']}")
```

#### 天气预报
```python
# 3 天预报
url = f"http://api.weatherapi.com/v1/forecast.json"
params = {
    "key": API_KEY,
    "q": city,
    "days": 3,
    "aqi": "yes",
    "lang": "zh"
}

response = requests.get(url, params=params)
data = response.json()

# 解析每日预报
for day in data['forecast']['forecastday']:
    date = day['date']
    max_temp = day['day']['maxtemp_c']
    min_temp = day['day']['mintemp_c']
    condition = day['day']['condition']['text']
    rain_chance = day['day']['daily_chance_of_rain']
    
    print(f"{date}: {min_temp}~{max_temp}°C - {condition} (降雨概率：{rain_chance}%)")
```

## 实践模板

### 模板 1: 每日天气简报
```python
def daily_weather_brief(city="Beijing"):
    """生成每日天气简报"""
    
    # 获取当前天气
    current = get_current_weather(city)
    
    # 获取预报
    forecast = get_forecast(city, days=3)
    
    # 生成简报
    brief = f"""
📍 {city} 天气简报

🌡️ 当前天气
温度：{current['temp']}°C (体感 {current['feels_like']}°C)
天气：{current['condition']}
湿度：{current['humidity']}%
风速：{current['wind']} m/s

📅 未来 3 天预报
"""
    
    for day in forecast:
        brief += f"{day['date']}: {day['min']}~{day['max']}°C {day['condition']}\n"
    
    # 生活建议
    brief += "\n💡 生活建议\n"
    if current['temp'] < 10:
        brief += "• 天气较冷，注意保暖\n"
    elif current['temp'] > 30:
        brief += "• 天气炎热，注意防暑\n"
    
    if current['condition'] in ['雨', '小雨', '大雨']:
        brief += "• 有雨，记得带伞\n"
    
    if current['aqi'] > 100:
        brief += "• 空气质量较差，减少户外活动\n"
    
    return brief
```

### 模板 2: 出行天气建议
```python
def travel_weather_advice(city, days=7):
    """出行李象建议"""
    
    forecast = get_forecast(city, days)
    
    advice = f"🧳 {city} 出行天气建议\n\n"
    
    # 分析天气趋势
    good_days = []
    bad_days = []
    
    for day in forecast:
        if day['condition'] in ['晴', '多云'] and day['rain_chance'] < 20:
            good_days.append(day['date'])
        else:
            bad_days.append(day['date'])
    
    advice += f"✅ 适合出行：{', '.join(good_days)}\n"
    advice += f"⚠️ 注意天气：{', '.join(bad_days)}\n\n"
    
    # 打包建议
    temps = [day['max'] for day in forecast]
    avg_temp = sum(temps) / len(temps)
    
    if avg_temp < 15:
        advice += "🧥 建议携带：厚外套、保暖衣物\n"
    elif avg_temp < 25:
        advice += "👕 建议携带：长袖、薄外套\n"
    else:
        advice += "🩳 建议携带：短袖、防晒衣\n"
    
    # 检查是否有雨
    if any(day['rain_chance'] > 50 for day in forecast):
        advice += "☂️ 必带：雨伞/雨衣\n"
    
    advice += "🕶️ 建议携带：防晒霜、墨镜\n"
    
    return advice
```

### 模板 3: 天气警报监控
```python
def weather_alert_monitor(cities):
    """监控多个城市的天气警报"""
    
    alerts = []
    
    for city in cities:
        weather = get_current_weather(city)
        
        # 检查极端天气
        if weather['temp'] > 35:
            alerts.append(f"🔴 {city}: 高温预警 ({weather['temp']}°C)")
        elif weather['temp'] < -10:
            alerts.append(f"🔵 {city}: 低温预警 ({weather['temp']}°C)")
        
        if weather['wind'] > 20:
            alerts.append(f"💨 {city}: 大风预警 ({weather['wind']} m/s)")
        
        if weather.get('aqi', 0) > 150:
            alerts.append(f"😷 {city}: 空气污染预警 (AQI: {weather['aqi']})")
        
        if weather['condition'] in ['暴雨', '大暴雨']:
            alerts.append(f"🌧️ {city}: 暴雨预警")
    
    if alerts:
        return "⚠️ 天气警报\n\n" + "\n".join(alerts)
    else:
        return "✅ 所有城市天气正常"
```

### 模板 4: 周末活动建议
```python
def weekend_activity_suggestion(city):
    """根据天气推荐周末活动"""
    
    forecast = get_forecast(city, days=2)
    
    suggestions = []
    
    for day in forecast:
        date = day['date']
        condition = day['condition']
        temp = day['max']
        rain = day['rain_chance']
        
        suggestions.append(f"\n📅 {date}")
        
        if condition in ['晴', '多云'] and rain < 30:
            if temp > 20:
                suggestions.append("✅ 推荐：郊游、野餐、骑行")
                suggestions.append("✅ 推荐：户外摄影、徒步")
            else:
                suggestions.append("✅ 推荐：登山、慢跑")
        else:
            suggestions.append("🏠 推荐：博物馆、电影院、室内运动")
            suggestions.append("🏠 推荐：咖啡厅、书店、购物中心")
    
    return "周末活动建议\n" + "\n".join(suggestions)
```

## 命令行工具

### 快速查询脚本
```bash
#!/bin/bash
# weather.sh

API_KEY="your-api-key"
CITY="${1:-Beijing}"

echo "🌤️  $CITY 天气"
echo "=============="

# 当前天气
curl -s "https://api.openweathermap.org/data/2.5/weather?q=$CITY&appid=$API_KEY&units=metric&lang=zh_cn" | \
jq -r '"温度：\(.main.temp)°C\n天气：\(.weather[0].description)\n湿度：\(.main.humidity)%\n风速：\(.wind.speed) m/s"'

echo ""
echo "📅 未来预报"
echo "=========="

# 预报
curl -s "https://api.openweathermap.org/data/2.5/forecast?q=$CITY&appid=$API_KEY&units=metric&lang=zh_cn" | \
jq -r '.list[:8][] | "\(.dt_txt): \(.main.temp)°C - \(.weather[0].description)"'
```

### 使用方法
```bash
# 查询北京天气
./weather.sh Beijing

# 查询上海天气
./weather.sh Shanghai

# 默认查询北京
./weather.sh
```

## 定时任务

### 每日天气推送
```bash
# 添加 cron 任务
crontab -e

# 每天早上 7 点推送天气
0 7 * * * /path/to/weather.sh Beijing | send_notification
```

### OpenClaw Cron 配置
```json
{
  "name": "每日天气推送",
  "schedule": {
    "kind": "cron",
    "expr": "0 7 * * *"
  },
  "payload": {
    "kind": "systemEvent",
    "text": "请推送北京今日天气"
  },
  "sessionTarget": "main"
}
```

## 数据可视化

### 温度趋势图
```python
import matplotlib.pyplot as plt

def plot_temperature_trend(city, days=7):
    """绘制温度趋势图"""
    
    forecast = get_forecast(city, days)
    
    dates = [day['date'] for day in forecast]
    max_temps = [day['max'] for day in forecast]
    min_temps = [day['min'] for day in forecast]
    
    plt.figure(figsize=(10, 5))
    plt.plot(dates, max_temps, 'r-', label='最高温', marker='o')
    plt.plot(dates, min_temps, 'b-', label='最低温', marker='o')
    plt.fill_between(dates, min_temps, max_temps, alpha=0.3)
    
    plt.title(f'{city} 温度趋势')
    plt.xlabel('日期')
    plt.ylabel('温度 (°C)')
    plt.legend()
    plt.grid(True, alpha=0.3)
    plt.xticks(rotation=45)
    plt.tight_layout()
    plt.savefig(f'{city}_temperature.png')
    plt.show()
```

## 多城市对比

### 城市天气对比
```python
def compare_cities(cities):
    """对比多个城市天气"""
    
    print(f"{'城市':<10} {'温度':<8} {'天气':<10} {'湿度':<6} {'风速':<6}")
    print("-" * 50)
    
    for city in cities:
        weather = get_current_weather(city)
        print(f"{city:<10} {weather['temp']:>5}°C  {weather['condition']:<10} {weather['humidity']:>5}% {weather['wind']:>5}m/s")
```

## 生活指数

### 计算生活指数
```python
def calculate_life_index(weather):
    """计算生活指数"""
    
    indices = {}
    
    # 穿衣指数
    temp = weather['temp']
    if temp < 5:
        indices['穿衣'] = "厚外套、羽绒服"
    elif temp < 15:
        indices['穿衣'] = "外套、毛衣"
    elif temp < 25:
        indices['穿衣'] = "长袖、薄外套"
    else:
        indices['穿衣'] = "短袖、薄衣"
    
    # 运动指数
    if weather['condition'] in ['晴', '多云'] and weather['wind'] < 10:
        indices['运动'] = "适宜户外运动"
    else:
        indices['运动'] = "建议室内运动"
    
    # 洗车指数
    if weather['condition'] in ['雨', '小雨', '大雨']:
        indices['洗车'] = "不宜洗车"
    else:
        indices['洗车'] = "适宜洗车"
    
    # 紫外线指数
    if weather['condition'] in ['晴'] and temp > 25:
        indices['防晒'] = "需要防晒"
    else:
        indices['防晒'] = "一般防护"
    
    return indices
```

## 错误处理

### 异常处理
```python
import requests
from requests.exceptions import RequestException

def safe_get_weather(city):
    """安全获取天气"""
    
    try:
        response = requests.get(
            f"https://api.openweathermap.org/data/2.5/weather",
            params={
                "q": city,
                "appid": API_KEY,
                "units": "metric",
                "lang": "zh_cn"
            },
            timeout=10
        )
        
        if response.status_code == 404:
            return {"error": f"找不到城市：{city}"}
        elif response.status_code == 401:
            return {"error": "API Key 无效"}
        elif response.status_code == 429:
            return {"error": "请求超限，请稍后重试"}
        
        response.raise_for_status()
        return response.json()
        
    except RequestException as e:
        return {"error": f"网络错误：{str(e)}"}
```

## 最佳实践

### 1. 缓存结果
```python
from functools import lru_cache
import time

@lru_cache(maxsize=100)
def get_weather_cached(city):
    """带缓存的天气查询"""
    return get_current_weather(city)

# 每 30 分钟更新一次缓存
```

### 2. 批量查询优化
```python
# 避免频繁调用 API
# 一次性获取多个城市数据
def batch_weather_query(cities):
    results = {}
    for city in cities:
        results[city] = get_weather_cached(city)
        time.sleep(0.1)  # 避免触发限流
    return results
```

### 3. 降级方案
```python
# 主 API 失败时使用备用 API
def get_weather_with_fallback(city):
    try:
        return get_weather_openweathermap(city)
    except:
        try:
            return get_weather_weatherapi(city)
        except:
            return get_weather_backup(city)
```

## 注意事项

⚠️ **API 限制**
- 注意免费额度的调用限制
- 实现缓存减少调用次数
- 准备备用 API 源

⚠️ **数据准确性**
- 天气预报存在误差
- 极端天气可能更新延迟
- 重要决策请咨询官方气象部门

⚠️ **隐私保护**
- 不要存储用户位置历史
- 使用 HTTPS 加密传输
- 遵守数据保护法规

---

**提示**: 建议配置多个 API 源作为备用，确保服务稳定性！
