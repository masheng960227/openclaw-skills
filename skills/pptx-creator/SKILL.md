# PPTX 技能 (PowerPoint 处理)

## 概述
创建、编辑和美化 PowerPoint 演示文稿的完整技能。

## 核心功能

### 1️⃣ PPTX 创建
- 从模板创建演示文稿
- 自动生成幻灯片
- 批量导入内容
- 支持多种布局

### 2️⃣ PPTX 编辑
- 添加/删除幻灯片
- 修改文本内容
- 插入图片/图表
- 调整布局

### 3️⃣ PPTX 美化
- 自动配色方案
- 字体优化
- 版式调整
- 动画添加

### 4️⃣ 格式转换
- PPTX ↔ PDF
- PPTX ↔ HTML
- PPTX ↔ 图片
- 提取内容

## 安装依赖

### Python 库
```bash
pip install python-pptx
pip install pptx-compose
pip install Pillow
```

### 或使用 uv
```bash
uv add python-pptx
uv add Pillow
```

## 使用示例

### 创建演示文稿
```python
from pptx import Presentation
from pptx.util import Inches, Pt

# 创建新演示文稿
prs = Presentation()

# 添加标题幻灯片
slide_layout = prs.slide_layouts[0]
slide = prs.slides.add_slide(slide_layout)
title = slide.shapes.title
subtitle = slide.placeholders[1]
title.text = "欢迎使用 PPTX 技能"
subtitle.text = "自动创建专业演示文稿"

# 添加内容幻灯片
slide_layout = prs.slide_layouts[1]
slide = prs.slides.add_slide(slide_layout)
title = slide.shapes.title
content = slide.placeholders[1]
title.text = "主要功能"
content.text = "• 自动创建\n• 智能美化\n• 批量处理\n• 格式转换"

# 保存
prs.save('presentation.pptx')
```

### 从 Markdown 创建 PPT
```python
def markdown_to_pptx(md_file, output_pptx):
    """将 Markdown 转换为 PPTX"""
    prs = Presentation()
    
    with open(md_file, 'r', encoding='utf-8') as f:
        content = f.read()
    
    # 按标题分割
    slides = content.split('# ')
    
    for slide_content in slides[1:]:  # 跳过第一个空元素
        lines = slide_content.strip().split('\n')
        title = lines[0].strip()
        
        # 创建幻灯片
        slide_layout = prs.slide_layouts[1]
        slide = prs.slides.add_slide(slide_layout)
        
        # 设置标题
        slide.shapes.title.text = title
        
        # 设置内容
        if len(lines) > 1:
            content_placeholder = slide.placeholders[1]
            text_frame = content_placeholder.text_frame
            text_frame.text = '\n'.join(lines[1:])
    
    prs.save(output_pptx)
    return output_pptx
```

### 添加图片
```python
from pptx.util import Inches

def add_image_to_slide(slide, image_path, left, top, width=None, height=None):
    """添加图片到幻灯片"""
    slide.shapes.add_picture(
        image_path,
        Inches(left),
        Inches(top),
        width=Inches(width) if width else None,
        height=Inches(height) if height else None
    )
```

### 添加图表
```python
from pptx.chart.data import CategoryChartData
from pptx.enum.chart import XL_CHART_TYPE

def add_chart_to_slide(slide, chart_type, data, left, top, width, height):
    """添加图表到幻灯片"""
    chart_data = CategoryChartData()
    chart_data.categories = data['categories']
    chart_data.add_series('数据', data['values'])
    
    x, y, cx, cy = Inches(left), Inches(top), Inches(width), Inches(height)
    slide.shapes.add_chart(
        chart_type, x, y, cx, cy, chart_data
    )
```

## PPT 美化技巧

### 1. 配色方案
```python
# 专业配色方案
COLOR_SCHEMES = {
    'business': ['#2C3E50', '#3498DB', '#E74C3C', '#ECF0F1'],
    'modern': ['#000000', '#FFFFFF', '#FF6B6B', '#4ECDC4'],
    'nature': ['#2D5016', '#8BC34A', '#FFC107', '#795548'],
    'tech': ['#0A192F', '#64FFDA', '#CCD6F6', '#FFAB91']
}

def apply_color_scheme(prs, scheme_name='business'):
    """应用配色方案"""
    colors = COLOR_SCHEMES.get(scheme_name, COLOR_SCHEMES['business'])
    
    for slide in prs.slides:
        for shape in slide.shapes:
            if shape.has_text_frame:
                for paragraph in shape.text_frame.paragraphs:
                    for run in paragraph.runs:
                        run.font.color.rgb = RGBColor(
                            int(colors[0][1:3], 16),
                            int(colors[0][3:5], 16),
                            int(colors[0][5:7], 16)
                        )
```

### 2. 字体优化
```python
def apply_font_style(prs, font_name='Microsoft YaHei', font_size=18):
    """应用统一字体样式"""
    for slide in prs.slides:
        for shape in slide.shapes:
            if shape.has_text_frame:
                for paragraph in shape.text_frame.paragraphs:
                    for run in paragraph.runs:
                        run.font.name = font_name
                        run.font.size = Pt(font_size)
```

### 3. 版式优化
```python
def optimize_layout(slide):
    """优化幻灯片版式"""
    # 调整标题位置
    if slide.shapes.title:
        title = slide.shapes.title
        title.top = Inches(0.5)
        title.height = Inches(1)
    
    # 调整内容边距
    for shape in slide.shapes:
        if shape.has_text_frame and shape != slide.shapes.title:
            tf = shape.text_frame
            tf.margin_left = Inches(0.5)
            tf.margin_right = Inches(0.5)
```

### 4. 添加动画
```python
from pptx.enum.text import MSO_ANIMATION

def add_animation(slide, animation_type='fade'):
    """添加动画效果"""
    for shape in slide.shapes:
        if shape.has_text_frame:
            for paragraph in shape.text_frame.paragraphs:
                paragraph.animation_type = MSO_ANIMATION.FADE
```

## 批量处理

### 批量创建
```python
def batch_create_pptx(data_list, template=None):
    """批量创建 PPTX"""
    for i, data in enumerate(data_list):
        if template:
            prs = Presentation(template)
        else:
            prs = Presentation()
        
        # 填充内容
        fill_presentation(prs, data)
        
        # 保存
        prs.save(f'presentation_{i+1}.pptx')
```

### 批量美化
```python
def batch_beautify(folder_path):
    """批量美化 PPTX 文件"""
    import os
    
    for filename in os.listdir(folder_path):
        if filename.endswith('.pptx'):
            filepath = os.path.join(folder_path, filename)
            prs = Presentation(filepath)
            
            # 应用美化
            apply_color_scheme(prs, 'modern')
            apply_font_style(prs)
            
            # 保存
            prs.save(filepath)
```

## 格式转换

### PPTX 转 PDF
```python
import comtypes.client

def pptx_to_pdf(pptx_path, pdf_path):
    """PPTX 转 PDF (Windows)"""
    powerpoint = comtypes.client.CreateObject("Powerpoint.Application")
    powerpoint.Visible = 1
    
    deck = powerpoint.Presentations.Open(pptx_path)
    deck.SaveAs(pdf_path, 32)  # 32 = ppSaveAsPDF
    deck.Close()
    powerpoint.Quit()
```

### PPTX 转图片
```python
def pptx_to_images(pptx_path, output_folder):
    """PPTX 转图片"""
    import os
    from pptx import Presentation
    
    prs = Presentation(pptx_path)
    os.makedirs(output_folder, exist_ok=True)
    
    for i, slide in enumerate(prs.slides):
        # 创建单页 PPT
        temp_prs = Presentation()
        temp_prs.slides.add_slide(slide.slide_layout)
        temp_prs.save(f'temp_{i}.pptx')
        
        # 转换为图片 (需要其他库支持)
        # 或使用在线 API
        
        os.remove(f'temp_{i}.pptx')
```

## 实用模板

### 商务汇报模板
```python
def create_business_report(title, sections, output='report.pptx'):
    """创建商务汇报 PPT"""
    prs = Presentation()
    
    # 封面
    slide = prs.slides.add_slide(prs.slide_layouts[0])
    slide.shapes.title.text = title
    slide.placeholders[1].text = f"汇报日期：{datetime.now().strftime('%Y-%m-%d')}"
    
    # 目录
    slide = prs.slides.add_slide(prs.slide_layouts[1])
    slide.shapes.title.text = "目录"
    content = slide.placeholders[1]
    for i, section in enumerate(sections, 1):
        content.text += f"{i}. {section}\n"
    
    # 内容页
    for section in sections:
        slide = prs.slides.add_slide(prs.slide_layouts[1])
        slide.shapes.title.text = section
        slide.placeholders[1].text = f"{section} 内容..."
    
    # 结束页
    slide = prs.slides.add_slide(prs.slide_layouts[1])
    slide.shapes.title.text = "谢谢"
    slide.placeholders[1].text = "Q&A"
    
    # 美化
    apply_color_scheme(prs, 'business')
    apply_font_style(prs)
    
    prs.save(output)
    return output
```

### 项目计划模板
```python
def create_project_plan(project_name, timeline, team, output='plan.pptx'):
    """创建项目计划 PPT"""
    prs = Presentation()
    
    # 项目概述
    slide = prs.slides.add_slide(prs.slide_layouts[0])
    slide.shapes.title.text = project_name
    slide.placeholders[1].text = f"项目负责人：{team['leader']}\n开始日期：{timeline['start']}"
    
    # 项目目标
    slide = prs.slides.add_slide(prs.slide_layouts[1])
    slide.shapes.title.text = "项目目标"
    content = slide.placeholders[1]
    content.text = '\n'.join([f"• {goal}" for goal in timeline['goals']])
    
    # 时间线
    slide = prs.slides.add_slide(prs.slide_layouts[1])
    slide.shapes.title.text = "项目时间线"
    # 可以添加图表
    
    # 团队介绍
    slide = prs.slides.add_slide(prs.slide_layouts[1])
    slide.shapes.title.text = "团队成员"
    content = slide.placeholders[1]
    content.text = '\n'.join([f"• {member}" for member in team['members']])
    
    prs.save(output)
    return output
```

## 最佳实践

### 1. 内容组织
```
✅ 每页一个核心观点
✅ 使用简洁的标题
✅ 要点式呈现
✅ 避免大段文字
```

### 2. 视觉设计
```
✅ 统一配色方案
✅ 一致字体样式
✅ 适当留白
✅ 高质量图片
```

### 3. 动画使用
```
✅ 适度使用动画
✅ 保持简洁
✅ 避免过度效果
✅ 确保流畅性
```

### 4. 文件管理
```
✅ 使用有意义的文件名
✅ 定期保存备份
✅ 压缩图片减小体积
✅ 嵌入字体确保兼容性
```

---

**提示**: PPT 美化的核心是一致性和简洁性，避免过度设计！
