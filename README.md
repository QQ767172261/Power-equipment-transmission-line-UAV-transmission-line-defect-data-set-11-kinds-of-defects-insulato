输电线路缺陷数据集，6000张作业图片，标注11种缺陷：绝缘子、防振锤、鸟巢等。类别信息下面图片有的。图片有txt和xml格式

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/6b0029011a7b4fecb0f5bb2fc4deab9a.png)


![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/680f99a1292e471a9d2c660b09e14f92.png)


 **输电线路无人机智能巡检数据集** 类别中文对照表， `baliser` 为 **避雷器**：

| **ID** | **英文名称（name）**       | **中文名称**           | **说明** |
|--------|----------------------------|------------------------|----------|
| 1      | baliser_ok                 | 避雷器正常              | 避雷器外观完好，无破损、倾斜或老化，功能正常 |
| 2      | baliser_aok                | 避雷器待确认/警告        | 避雷器存在轻微异常（如轻微倾斜、锈蚀），需进一步核查 |
| 3      | baliser_nok                | 避雷器故障              | 避雷器破损、断裂、脱落或严重老化，失去防雷功能 |
| 4      | insulator_ok               | 绝缘子正常              | 绝缘子无裂纹、破损、闪络或严重污秽 |
| 5      | insulator_nok              | 绝缘子故障              | 绝缘子破裂、掉串、严重污秽或发生电晕放电 |
| 6      | bird_nest                  | 鸟巢                  | 线路杆塔或绝缘子附近有鸟类筑巢，可能引发短路 |
| 7      | stockbridge_ok             | 防振锤正常              | 防振锤安装牢固，无松动、断裂或缺失 |
| 8      | stockbridge_nok            | 防振锤故障              | 防振锤断裂、脱落、变形或严重锈蚀 |
| 9      | spacer_ok                  | 间隔棒正常              | 间隔棒完整且位置正确，保持分裂导线间距 |
| 10     | spacer_nok                 | 间隔棒故障              | 间隔棒断裂、变形或脱落，可能导致导线鞭击 |
| 11     | insulator_unk              | 绝缘子状态未知          | 因遮挡、模糊或角度问题无法判断绝缘子状态 |

### 📄 修正版 YOLO `data.yaml` 配置（推荐使用）
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/f9553e567bec4787a5783c17e560d335.png)

```yaml
# power_line_inspection.yaml
path: ./datasets/power_line_uav
train: ./images/train
val: ./images/val

# 类别数量
nc: 11

# 类别名称（按ID顺序，已修正）
names:
  - baliser_ok          # 避雷器正常
  - baliser_aok         # 避雷器待确认/警告
  - baliser_nok         # 避雷器故障
  - insulator_ok        # 绝缘子正常
  - insulator_nok       # 绝缘子故障
  - bird_nest           # 鸟巢
  - stockbridge_ok      # 防振锤正常
  - stockbridge_nok     # 防振锤故障
  - spacer_ok           # 间隔棒正常
  - spacer_nok          # 间隔棒故障
  - insulator_unk       # 绝缘子状态未知
```

---
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2dd18783b3ef4d7aa0a6aa981107f93e.png)
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/2406996e0a384d31a6a5cb8dd878deed.png)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/231e0ec6170a4e91a1061c3942c49520.jpeg)
1
![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/ee7786f374aa481b99852c8bb53832dc.png)

使用 [Ultralytics YOLOv5 官方仓库](https://github.com/ultralytics/yolov5) 作为基础，提供从环境配置、数据准备、模型训练到推理的全流程代码。

---

## ✅ 一、项目目录结构建议

```bash
power-line-inspection/
├── data/
│   └── power_line.yaml          # 数据集配置文件
├── datasets/
│   └── power_line_uav/
│       ├── images/
│       │   ├── train/
│       │   └── val/
│       └── labels/
│           ├── train/
│           └── val/
├── yolov5/                      # YOLOv5 代码仓库（git clone）
├── train.py                     # 自定义训练脚本（可选）
└── detect.py                    # 推理脚本
```

---

## ✅ 二、安装环境（Python 3.8+）

```bash
# 1. 克隆 YOLOv5 仓库
git clone https://github.com/ultralytics/yolov5.git
cd yolov5

# 2. 安装依赖（含 PyTorch）
pip install -r requirements.txt

# 如果没有GPU，PyTorch会自动安装CPU版本
# 如需指定GPU版本（如CUDA 11.8）：
# pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

---

## ✅ 三、准备数据集配置文件

### 创建 `data/power_line.yaml`

```yaml
# power_line.yaml

# 训练和验证数据路径
train: ../datasets/power_line_uav/images/train
val: ../datasets/power_line_uav/images/val

# 类别数量
nc: 11

# 类别名称（必须与标签ID一一对应）
names:
  - baliser_ok          # 避雷器正常
  - baliser_aok         # 避雷器待确认/警告
  - baliser_nok         # 避雷器故障
  - insulator_ok        # 绝缘子正常
  - insulator_nok       # 绝缘子故障
  - bird_nest           # 鸟巢
  - stockbridge_ok      # 防振锤正常
  - stockbridge_nok     # 防振锤故障
  - spacer_ok           # 间隔棒正常
  - spacer_nok          # 间隔棒故障
  - insulator_unk       # 绝缘子状态未知
```

> ✅ 将该文件保存为 `yolov5/data/power_line.yaml`

---

## ✅ 四、数据集要求说明

- 图像格式：`.jpg`, `.png` 等常见格式
- 标签格式：YOLO 格式 `.txt` 文件，每个文件内容为：
  ```
  class_id center_x center_y width height
  ```
  （所有坐标归一化到 [0,1]）

- 图像与标签文件名需一致，例如：
  - `images/train/img001.jpg`
  - `labels/train/img001.txt`

---

## ✅ 五、启动训练（命令行方式）

```bash
python train.py \
  --img 640 \
  --batch 16 \
  --epochs 100 \
  --data power_line.yaml \
  --weights yolov5s.pt \
  --cfg models/yolov5s.yaml \
  --name power_line_exp \
  --project runs/train \
  --exist-ok \
  --device 0  # 使用GPU 0，若用CPU则改为 --device cpu
```

### 参数说明：

| 参数 | 说明 |
|------|------|
| `--img 640` | 输入图像大小（640x640） |
| `--batch 16` | 批次大小（根据GPU显存调整） |
| `--epochs 100` | 训练轮数 |
| `--data` | 数据配置文件路径 |
| `--weights` | 预训练权重（推荐 yolov5s.pt） |
| `--name` | 实验名称 |
| `--exist-ok` | 允许覆盖已有实验 |

---

## ✅ 六、自定义训练脚本（可选）

### `train.py`（简化版）

```python
import torch
from yolov5 import train

if __name__ == "__main__":
    # 设置训练参数
    train.run(
        imgsz=640,
        batch=16,
        epochs=100,
        data='data/power_line.yaml',
        weights='yolov5s.pt',
        cfg='models/yolov5s.yaml',
        name='power_line_exp',
        project='runs/train',
        exist_ok=True,
        device='0' if torch.cuda.is_available() else 'cpu',
        workers=4,
        optimizer='Adam',  # 可选 SGD, Adam, AdamW
        lr0=0.001          # 初始学习率
    )
```

> 将此脚本放在 `yolov5/` 目录下运行即可。

---

## ✅ 七、训练过程监控

训练过程中，日志和结果保存在：

```
runs/train/power_line_exp/
├── weights/
│   ├── best.pt    # 最佳模型（mAP最高）
│   └── last.pt    # 最终轮次模型
├── results.png    # mAP、loss、precision等曲线
├── labels.jpg     # 带标签的样本可视化
└── opt.yaml       # 训练参数记录
```

- `results.png` 可查看：
  - mAP@0.5:0.95（核心指标）
  - Precision、Recall
  - Box Loss、Objectness Loss

---

## ✅ 八、模型推理（检测新图像）

```bash
python detect.py \
  --weights runs/train/power_line_exp/weights/best.pt \
  --source ../datasets/power_line_uav/images/val \
  --img 640 \
  --conf 0.4 \
  --iou-thres 0.45 \
  --project runs/detect \
  --name power_line_test \
  --exist-ok
```

> 检测结果将保存在 `runs/detect/power_line_test/`，包含带框的图像和置信度。

---

## ✅ 九、评估模型性能

```bash
python val.py \
  --weights runs/train/power_line_exp/weights/best.pt \
  --data data/power_line.yaml \
  --img 640 \
  --task val
```




