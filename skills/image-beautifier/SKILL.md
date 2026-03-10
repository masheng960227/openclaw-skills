# Image Beautifier 技能 (图片美化)

## 概述
使用 AI 和图像处理技术美化和优化图片质量。

## 核心功能

### 1️⃣ 基础美化
- 亮度/对比度调整
- 色彩校正
- 锐化处理
- 降噪

### 2️⃣ AI 增强
- 超分辨率放大
- 细节增强
- 老照片修复
- 人脸美化

### 3️⃣ 滤镜效果
- 艺术滤镜
- 风格转换
- 色调调整
- 特效添加

### 4️⃣ 批量处理
- 批量美化
- 统一风格
- 自动优化
- 格式转换

## 安装依赖

### Python 库
```bash
pip install Pillow
pip install opencv-python
pip install numpy
pip install rembg  # 背景移除
pip install basicsr  # 超分辨率
```

### 或使用 uv
```bash
uv add Pillow opencv-python numpy rembg
```

## 使用示例

### 基础美化
```python
from PIL import Image, ImageEnhance

def beautify_image(input_path, output_path):
    """基础图片美化"""
    img = Image.open(input_path)
    
    # 亮度调整 (+20%)
    enhancer = ImageEnhance.Brightness(img)
    img = enhancer.enhance(1.2)
    
    # 对比度调整 (+30%)
    enhancer = ImageEnhance.Contrast(img)
    img = enhancer.enhance(1.3)
    
    # 色彩饱和度 (+25%)
    enhancer = ImageEnhance.Color(img)
    img = enhancer.enhance(1.25)
    
    # 锐化处理
    enhancer = ImageEnhance.Sharpness(img)
    img = enhancer.enhance(1.5)
    
    img.save(output_path, quality=95)
    return output_path
```

### 自动优化
```python
def auto_optimize(image_path, output_path):
    """自动优化图片"""
    from PIL import ImageOps
    
    img = Image.open(image_path)
    
    # 自动色彩平衡
    img = ImageOps.autocontrast(img)
    
    # 自动调整大小 (保持比例)
    img.thumbnail((1920, 1080), Image.Resampling.LANCZOS)
    
    # 保存优化
    img.save(
        output_path,
        optimize=True,
        quality=85,
        progressive=True
    )
    
    return output_path
```

### 背景移除
```python
from rembg import remove

def remove_background(input_path, output_path):
    """移除图片背景"""
    with open(input_path, 'rb') as i:
        input_image = i.read()
    
    output_image = remove(input_image)
    
    with open(output_path, 'wb') as o:
        o.write(output_image)
    
    return output_path
```

### 超分辨率放大
```python
from basicsr.archs.rrdbnet_arch import RRDBNet
from realesrgan import RealESRGANer

def upscale_image(input_path, output_path, scale=4):
    """超分辨率放大图片"""
    # 初始化模型
    model = RRDBNet(num_in_ch=3, num_out_ch=3, num_feat=64, num_block=23, num_grow_ch=32, scale=4)
    
    upsampler = RealESRGANer(
        scale=4,
        model_path='experiments/pretrained_models/RealESRGAN_x4plus.pth',
        model=model,
        tile=0,
        tile_pad=10,
        pre_pad=0,
        half=True
    )
    
    # 放大图片
    img = cv2.imread(input_path, cv2.IMREAD_UNCHANGED)
    output, _ = upsampler.enhance(img, outscale=scale)
    
    # 保存
    cv2.imwrite(output_path, output)
    return output_path
```

### 人像美化
```python
import cv2
import numpy as np

def beautify_portrait(input_path, output_path):
    """人像美化"""
    img = cv2.imread(input_path)
    
    # 磨皮 (双边滤波)
    smoothed = cv2.bilateralFilter(img, 9, 75, 75)
    
    # 美白 (调整亮度)
    lab = cv2.cvtColor(smoothed, cv2.COLOR_BGR2LAB)
    l, a, b = cv2.split(lab)
    l = cv2.addWeighted(l, 1.1, l, 0, 0)
    lab = cv2.merge([l, a, b])
    whitened = cv2.cvtColor(lab, cv2.COLOR_LAB2BGR)
    
    # 锐化
    kernel = np.array([[-1,-1,-1], [-1,9,-1], [-1,-1,-1]])
    sharpened = cv2.filter2D(whitened, -1, kernel)
    
    cv2.imwrite(output_path, sharpened)
    return output_path
```

## 滤镜效果

### 复古滤镜
```python
def vintage_filter(image_path, output_path):
    """复古滤镜"""
    img = Image.open(image_path)
    
    # 调整色调
    img = img.convert('Sepia')
    
    # 添加颗粒感
    enhancer = ImageEnhance.Sharpness(img)
    img = enhancer.enhance(0.8)
    
    # 降低饱和度
    enhancer = ImageEnhance.Color(img)
    img = enhancer.enhance(0.7)
    
    img.save(output_path)
    return output_path
```

### 黑白艺术
```python
def black_white_art(image_path, output_path):
    """黑白艺术效果"""
    img = Image.open(image_path).convert('L')
    
    # 增强对比度
    enhancer = ImageEnhance.Contrast(img)
    img = enhancer.enhance(1.5)
    
    img.save(output_path)
    return output_path
```

### 电影色调
```python
def cinematic_look(image_path, output_path):
    """电影色调"""
    img = Image.open(image_path)
    
    # 调整色温 (偏暖)
    img = img.convert('RGB')
    r, g, b = img.split()
    r = r.point(lambda i: i * 1.1)
    b = b.point(lambda i: i * 0.9)
    img = Image.merge('RGB', (r, g, b))
    
    # 添加暗角效果
    enhancer = ImageEnhance.Brightness(img)
    img = enhancer.enhance(0.95)
    
    img.save(output_path)
    return output_path
```

## 批量处理

### 批量美化
```python
import os
from pathlib import Path

def batch_beautify(input_folder, output_folder):
    """批量美化图片"""
    os.makedirs(output_folder, exist_ok=True)
    
    for filename in os.listdir(input_folder):
        if filename.lower().endswith(('.png', '.jpg', '.jpeg')):
            input_path = os.path.join(input_folder, filename)
            output_path = os.path.join(output_folder, filename)
            
            try:
                beautify_image(input_path, output_path)
                print(f"✓ {filename}")
            except Exception as e:
                print(f"✗ {filename}: {e}")
```

### 统一风格
```python
def unify_style(image_paths, reference_image, output_folder):
    """统一图片风格"""
    os.makedirs(output_folder, exist_ok=True)
    
    # 分析参考图片
    ref_img = Image.open(reference_image)
    ref_hist = ref_img.histogram()
    
    for i, img_path in enumerate(image_paths):
        img = Image.open(img_path)
        
        # 直方图匹配
        matched = ImageOps.equalize(img)
        
        # 保存
        output_path = os.path.join(output_folder, f'style_{i}.png')
        matched.save(output_path)
```

## 图片优化

### 压缩优化
```python
def optimize_for_web(input_path, output_path, max_size=500):
    """优化图片用于网页"""
    img = Image.open(input_path)
    
    # 调整大小
    img.thumbnail((max_size, max_size), Image.Resampling.LANCZOS)
    
    # 转换为 RGB (移除 alpha 通道)
    if img.mode in ('RGBA', 'LA', 'P'):
        img = img.convert('RGB')
    
    # 保存优化
    img.save(
        output_path,
        'JPEG',
        quality=80,
        optimize=True,
        progressive=True
    )
    
    return output_path
```

### 格式转换
```python
def convert_format(input_path, output_format='PNG'):
    """转换图片格式"""
    img = Image.open(input_path)
    
    # 生成输出路径
    base = os.path.splitext(input_path)[0]
    output_path = f"{base}.{output_format.lower()}"
    
    # 转换
    if output_format.upper() == 'PNG' and img.mode in ('RGBA', 'LA'):
        img.save(output_path, 'PNG')
    else:
        img = img.convert('RGB')
        img.save(output_path, output_format.upper())
    
    return output_path
```

## AI 增强服务

### 调用 API
```python
import requests

def ai_enhance_api(image_path, output_path, api_key):
    """使用 AI API 增强图片"""
    
    # 示例：使用 Replicate API
    url = "https://api.replicate.com/v1/predictions"
    headers = {
        "Authorization": f"Token {api_key}",
        "Content-Type": "application/json"
    }
    
    with open(image_path, 'rb') as f:
        image_data = f.read()
    
    payload = {
        "version": "model-version-id",
        "input": {
            "image": image_data,
            "scale": 4
        }
    }
    
    response = requests.post(url, json=payload, headers=headers)
    result = response.json()
    
    # 等待处理完成并下载
    # ...
    
    return output_path
```

## 实用工具

### 图片信息
```python
def get_image_info(image_path):
    """获取图片信息"""
    from PIL import Image
    
    img = Image.open(image_path)
    
    info = {
        'format': img.format,
        'mode': img.mode,
        'size': img.size,
        'width': img.width,
        'height': img.height,
        'file_size': os.path.getsize(image_path)
    }
    
    return info
```

### EXIF 处理
```python
from PIL.ExifTags import TAGS

def extract_exif(image_path):
    """提取 EXIF 信息"""
    img = Image.open(image_path)
    exif_data = {}
    
    info = img._getexif()
    if info:
        for tag_id in info:
            tag = TAGS.get(tag_id, tag_id)
            data = info.get(tag_id)
            exif_data[tag] = data
    
    return exif_data
```

### 水印添加
```python
def add_watermark(image_path, output_path, watermark_text):
    """添加水印"""
    img = Image.open(image_path).convert('RGBA')
    
    # 创建透明图层
    txt_layer = Image.new('RGBA', img.size, (255, 255, 255, 0))
    
    # 创建字体
    from PIL import ImageDraw, ImageFont
    draw = ImageDraw.Draw(txt_layer)
    font = ImageFont.truetype("arial.ttf", 36)
    
    # 绘制水印
    draw.text(
        (img.width - 200, img.height - 50),
        watermark_text,
        font=font,
        fill=(255, 255, 255, 128)
    )
    
    # 合并
    watermarked = Image.alpha_composite(img, txt_layer)
    watermarked.convert('RGB').save(output_path)
    
    return output_path
```

## 最佳实践

### 1. 图片质量
```
✅ 使用无损格式保存中间结果
✅ 最终输出根据用途选择格式
✅ 保留原始文件备份
```

### 2. 性能优化
```
✅ 批量处理使用多线程
✅ 大图分块处理
✅ 缓存处理结果
```

### 3. 色彩管理
```
✅ 使用 sRGB 色彩空间
✅ 注意伽马校正
✅ 考虑目标设备
```

### 4. 文件大小
```
✅ 网页图片 < 500KB
✅ 打印图片保持高质量
✅ 社交媒体按平台优化
```

---

**提示**: 图片美化的关键是适度，避免过度处理导致不自然！
