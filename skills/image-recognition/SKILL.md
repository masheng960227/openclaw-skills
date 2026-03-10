# Image Recognition & Analysis 技能 (图片识别与分析)

## 概述
使用 AI 进行图片内容识别、文字提取 (OCR)、物体检测和分析。

## 核心功能

### 1️⃣ OCR 文字识别
- 提取图片中的文字
- 支持多语言
- 手写体识别
- 表格文字提取

### 2️⃣ 物体检测
- 识别图片中的物体
- 物体位置标注
- 多物体识别
- 场景理解

### 3️⃣ 图片分析
- 颜色分析
- 构图评估
- 质量评分
- 内容描述

### 4️⃣ 人脸识别
- 人脸检测
- 表情分析
- 年龄估计
- 人脸比对

## 配置方法

### Google Cloud Vision API
```json
{
  "image-recognition": {
    "provider": "google-vision",
    "apiKey": "YOUR_GOOGLE_CLOUD_API_KEY"
  }
}
```

### Azure Computer Vision
```json
{
  "image-recognition": {
    "provider": "azure-vision",
    "apiKey": "YOUR_AZURE_API_KEY",
    "endpoint": "https://YOUR_RESOURCE.cognitiveservices.azure.com/"
  }
}
```

### AWS Rekognition
```json
{
  "image-recognition": {
    "provider": "aws-rekognition",
    "accessKeyId": "YOUR_AWS_ACCESS_KEY",
    "secretAccessKey": "YOUR_AWS_SECRET_KEY",
    "region": "us-east-1"
  }
}
```

## 使用示例

### OCR 文字提取
```bash
# 提取图片文字
python scripts/ocr.py --image photo.jpg --output text.txt

# 多语言识别
python scripts/ocr.py --image document.png --lang zh-CN,en

# 批量处理
python scripts/ocr.py --dir ./images --output ./output
```

### 物体检测
```bash
# 检测物体
python scripts/detect.py --image scene.jpg

# 输出标注图片
python scripts/detect.py --image photo.jpg --output annotated.jpg

# JSON 格式结果
python scripts/detect.py --image test.jpg --format json
```

### 图片分析
```bash
# 完整分析
python scripts/analyze.py --image photo.jpg

# 仅颜色分析
python scripts/analyze.py --image photo.jpg --features color

# 质量评估
python scripts/analyze.py --image photo.jpg --features quality
```

### 人脸识别
```bash
# 检测人脸
python scripts/face.py --image group.jpg

# 表情分析
python scripts/face.py --image portrait.jpg --analyze emotion

# 人脸比对
python scripts/face.py --compare face1.jpg face2.jpg
```

## API 对比

| 服务 | OCR | 物体检测 | 人脸 | 价格 |
|------|-----|----------|------|------|
| **Google Vision** | ✅ | ✅ | ✅ | $1.5/1000 |
| **Azure Vision** | ✅ | ✅ | ✅ | $1.0/1000 |
| **AWS Rekognition** | ✅ | ✅ | ✅ | $1.0/1000 |
| **百度 AI** | ✅ | ✅ | ✅ | 免费额度 |
| **腾讯优图** | ✅ | ✅ | ✅ | 免费额度 |

## 实践场景

### 场景 1: 文档数字化
```bash
# 扫描文档转文字
python scripts/ocr.py --scan document.pdf --output document.txt

# 保持格式
python scripts/ocr.py --scan document.pdf --format markdown
```

### 场景 2: 商品识别
```bash
# 识别商品
python scripts/detect.py --image product.jpg --category retail

# 提取价格标签
python scripts/ocr.py --image shelf.jpg --pattern price
```

### 场景 3: 照片管理
```bash
# 自动标签
python scripts/analyze.py --dir ./photos --tag

# 人脸识别分组
python scripts/face.py --dir ./photos --group
```

### 场景 4: 内容审核
```bash
# 检测不当内容
python scripts/moderate.py --image upload.jpg

# 批量审核
python scripts/moderate.py --dir ./uploads --report
```

## 代码示例

### Python OCR 示例
```python
from google.cloud import vision

def extract_text(image_path):
    client = vision.ImageAnnotatorClient()
    
    with open(image_path, 'rb') as image_file:
        content = image_file.read()
    
    image = vision.Image(content=content)
    response = client.text_detection(image=image)
    texts = response.text_annotations
    
    if texts:
        return texts[0].description
    return ""

# 使用
text = extract_text("document.jpg")
print(text)
```

### 物体检测示例
```python
import requests

def detect_objects(image_path, api_key):
    url = "https://api.azure.com/vision/v3.2/analyze"
    headers = {"Ocp-Apim-Subscription-Key": api_key}
    params = {"visualFeatures": "Objects"}
    
    with open(image_path, 'rb') as f:
        image_data = f.read()
    
    response = requests.post(
        url,
        headers=headers,
        params=params,
        data=image_data
    )
    
    result = response.json()
    return result.get('objects', [])

# 使用
objects = detect_objects("scene.jpg", "your-api-key")
for obj in objects:
    print(f"{obj['object']}: {obj['confidence']:.2f}")
```

### 图片描述生成
```python
def generate_caption(image_path, api_key):
    url = "https://api.azure.com/vision/v3.2/analyze"
    headers = {"Ocp-Apim-Subscription-Key": api_key}
    params = {"visualFeatures": "Description"}
    
    with open(image_path, 'rb') as f:
        image_data = f.read()
    
    response = requests.post(
        url,
        headers=headers,
        params=params,
        data=image_data
    )
    
    result = response.json()
    captions = result.get('description', {}).get('captions', [])
    
    if captions:
        return captions[0]['text']
    return "No description available"

# 使用
caption = generate_caption("photo.jpg", "your-api-key")
print(f"图片描述：{caption}")
```

## 本地方案

### Tesseract OCR (免费)
```bash
# 安装
brew install tesseract

# 使用
tesseract image.png output
tesseract image.png output -l chi_sim+eng  # 中英文
```

### YOLO 物体检测 (免费)
```bash
# 安装
pip install ultralytics

# 使用
from ultralytics import YOLO
model = YOLO('yolov8n.pt')
results = model('image.jpg')
```

### OpenCV 基础分析
```python
import cv2
import numpy as np

# 读取图片
img = cv2.imread('photo.jpg')

# 颜色直方图
hist = cv2.calcHist([img], [0, 1, 2], None, [8, 8, 8], [0, 256, 0, 256, 0, 256])

# 边缘检测
edges = cv2.Canny(img, 100, 200)

# 人脸检测
face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_default.xml')
faces = face_cascade.detectMultiScale(cv2.cvtColor(img, cv2.COLOR_BGR2GRAY))
```

## 性能优化

### 批量处理
```python
from concurrent.futures import ThreadPoolExecutor

def process_batch(images, max_workers=10):
    with ThreadPoolExecutor(max_workers=max_workers) as executor:
        results = list(executor.map(process_image, images))
    return results
```

### 缓存结果
```python
import hashlib
import json

def cached_analysis(image_path, cache_dir='./cache'):
    # 生成图片 hash
    with open(image_path, 'rb') as f:
        image_hash = hashlib.md5(f.read()).hexdigest()
    
    cache_file = f"{cache_dir}/{image_hash}.json"
    
    # 检查缓存
    if os.path.exists(cache_file):
        with open(cache_file, 'r') as f:
            return json.load(f)
    
    # 执行分析
    result = analyze_image(image_path)
    
    # 保存缓存
    os.makedirs(cache_dir, exist_ok=True)
    with open(cache_file, 'w') as f:
        json.dump(result, f)
    
    return result
```

### 图片预处理
```python
def preprocess_for_ocr(image_path):
    img = cv2.imread(image_path)
    gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    
    # 去噪
    denoised = cv2.fastNlMeansDenoising(gray)
    
    # 二值化
    _, binary = cv2.threshold(denoised, 0, 255, cv2.THRESH_BINARY + cv2.THRESH_OTSU)
    
    # 保存
    cv2.imwrite('processed.png', binary)
    return 'processed.png'
```

## 最佳实践

### 1. 图片质量
```
✅ 高分辨率 (至少 300 DPI for OCR)
✅ 良好光照
✅ 清晰对焦
✅ 适当对比度
```

### 2. 格式选择
```
OCR: PNG, TIFF (无损)
检测：JPEG (平衡)
分析：根据需求选择
```

### 3. 成本优化
```
- 本地预处理减少 API 调用
- 批量处理享受折扣
- 缓存结果避免重复
- 选择合适分辨率
```

### 4. 隐私保护
```
- 敏感图片本地处理
- 传输使用 HTTPS
- 及时删除临时文件
- 遵守数据保护法规
```

## 故障排除

### OCR 准确率低
```
解决:
1. 提高图片分辨率
2. 增强对比度
3. 校正倾斜
4. 选择正确语言包
```

### 检测漏检
```
解决:
1. 调整置信度阈值
2. 使用更大模型
3. 多尺度检测
4. 图像增强
```

### API 超时
```
解决:
1. 压缩图片大小
2. 增加超时时间
3. 分批处理
4. 使用本地方案
```

---

**提示**: 根据具体需求选择合适的方案 - 云端 API 精度高，本地方案隐私好且免费！
