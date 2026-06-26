# 论文复现项目

本仓库包含多个计算机视觉论文复现项目，主要用于学习和研究方向包括伪装目标检测（COD）和 RGB-D 显著目标检测（RGB-D SOD）。

## 项目背景

论文复现是深度学习研究中的重要环节，通过复现经典论文的代码和实验，可以：
1. 深入理解模型架构和算法原理
2. 掌握实验设计和评估方法
3. 为后续研究工作打下基础

## 复现项目列表

| 项目 | 任务 | 论文 | 会议/期刊 | 年份 |
|------|------|------|----------|------|
| SINet-V2 | 伪装目标检测 (COD) | Concealed Object Detection | TPAMI | 2022 |
| CIRNet | RGB-D 显著目标检测 | CIR-Net: Cross-modality Interaction and Refinement | TIP | 2022 |

---

## 复现过程总览

### 通用复现步骤

每个项目的复现流程大致相同：

```
1. 环境配置 → 2. 代码理解 → 3. 数据准备 → 4. 模型测试 → 5. 指标评估 → 6. 结果分析
```

### 1. 环境配置

#### 创建 Conda 环境

```bash
# SINet-V2 环境
conda create -n sinetv2 python=3.10
conda activate sinetv2

# CIRNet 环境
conda create -n cirnet python=3.7
conda activate cirnet
```

#### 安装 PyTorch

```bash
# 根据 CUDA 版本选择合适的 PyTorch
pip install torch torchvision torchaudio

# 或指定版本
pip install torch==1.10.1 torchvision==0.11.2
```

#### 安装其他依赖

```bash
pip install numpy opencv-python pillow tqdm scipy scikit-image
pip install py-sod-metrics  # 用于评价指标计算
```

### 2. 权重文件准备

由于 GitHub 文件大小限制，预训练权重需要手动下载：

| 项目 | 权重文件 | 下载链接 |
|------|---------|---------|
| SINet-V2 | Net_epoch_best.pth | [Google Drive](https://drive.google.com/file/d/1D3RKQ8Nzd0ArV_c47StVKEuaoYTwnclR/view) |
| SINet-V2 | Res2Net 权重 | [Google Drive](https://drive.google.com/file/d/1QumnqSY_2wa-81-Ti0X1-jQzaGDIfa9r/view) |
| CIRNet | CIRNet_R50.pth | [Baidu Cloud](https://pan.baidu.com/s/1QUoGbqgaZhalwJxoDOpL8A) (密码: 1234) |
| CIRNet | CIRNet_VGG16.pth | [Baidu Cloud](https://pan.baidu.com/s/1tP3XFXhmAjC2Q3I8lC7TwQ) (密码: 1234) |

### 3. 数据集准备

#### SINet-V2 数据集

```
Dataset/
├── TestDataset/
│   ├── CAMO/          # 测试集
│   ├── CHAMELEON/     # 测试集
│   └── COD10K/        # 测试集
└── TrainDataset/      # 训练集
```

下载链接：[Google Drive](https://drive.google.com/file/d/1V0iSEdYJrT0Y_DHZfVGMg6TySFRNTy4o/view)

#### CIRNet 数据集

```
data/
├── train/             # 训练数据 (RGB + Depth)
└── test/              # 测试数据
    ├── STEREO797/
    ├── NLPR/
    ├── NJUD/
    ├── DUT/
    ├── LFSD/
    └── SIP/
```

下载链接：[Baidu Cloud](https://pan.baidu.com/s/1KVCLaXLrMZDUZDpYBd_SJA) (密码: 1234)

### 4. 模型测试

#### SINet-V2

```bash
cd SINet_V2/SINet-V2-main
python MyTesting.py
```

#### CIRNet

```bash
cd CIRNet/CIRNet_TIP2022-main
python CIRNet_test.py --backbone R50 --test_model CIRNet_R50.pth
```

### 5. 指标评估

常用评价指标：

| 指标 | 说明 |
|------|------|
| S-measure | 结构相似性度量，评估预测图与 GT 的结构一致性 |
| E-measure | 基于局部估计的相似性度量 |
| F-measure | 精确率和召回率的调和平均 |
| Weighted F-measure | 加权 F-measure，更关注难分样本 |
| MAE | 平均绝对误差，衡量像素级预测误差 |

使用 py-sod-metrics 库进行评估：

```python
from py_sod_metrics import Smeasure, Emeasure, Fmeasure, MAE

# 计算各项指标
...
```

---

## 各项目详细复现过程

### SINet-V2 复现过程

详细复现步骤请参考：[SINet-V2 README](./SINet_V2/SINet-V2-main/README.md)

**核心要点：**
1. 模型核心思想：Search（搜索）→ Identification（识别）
2. 关键模块：邻居连接解码器(NCD)、组反转注意力(GRA)
3. 支持 PyTorch 和 Jittor 两种框架
4. 在多个 COD 数据集上达到 SOTA 性能

### CIRNet 复现过程

详细复现步骤请参考：[CIRNet README](./CIRNet/CIRNet_TIP2022-main/README.md)

**核心要点：**
1. 模型核心思想：Cross-modality Interaction（跨模态交互）→ Refinement（细化）
2. 支持 ResNet50 和 VGG16 两种骨干网络
3. 融合 RGB 和 Depth 两种模态信息
4. 在 6 个 RGB-D SOD 数据集上进行测试

---

## 项目结构

```
Projects_reproduction/
├── README.md                    # 本文件
├── SINet_V2/
│   └── SINet-V2-main/
│       ├── README.md            # SINet-V2 复现详细说明
│       ├── lib/                 # 网络模块
│       ├── utils/               # 工具函数
│       ├── jittor_lib/          # Jittor 版本
│       ├── Dataset/             # 数据集目录
│       ├── snapshot/            # 权重文件
│       ├── MyTesting.py         # 测试脚本
│       └── MyTrain_Val.py       # 训练脚本
└── CIRNet/
    └── CIRNet_TIP2022-main/
        ├── README.md            # CIRNet 复现详细说明
        ├── backbone/            # 骨干网络
        ├── model/               # 模型定义
        ├── module/              # 功能模块
        ├── dataLoader.py        # 数据加载
        ├── CIRNet_train.py      # 训练脚本
        └── CIRNet_test.py       # 测试脚本
```

---

## 复现心得

### 遇到的问题与解决方案

1. **环境配置问题**
   - PyTorch 版本与 CUDA 版本不匹配 → 根据 CUDA 版本安装对应 PyTorch
   - OpenCV 依赖缺失 → `sudo apt-get install libgl1`

2. **数据集路径问题**
   - 不同项目数据集结构不同 → 仔细阅读官方文档，统一目录结构

3. **权重文件下载**
   - Google Drive 下载慢 → 使用代理或寻找镜像源
   - 百度云链接失效 → 联系作者获取新链接

4. **指标计算**
   - 使用 py-sod-metrics 库简化评估流程

### 学习收获

1. 深入理解 COD 和 RGB-D SOD 任务的定义和挑战
2. 掌握 PyTorch 项目结构和调试方法
3. 学习多模态融合和注意力机制的设计思想
4. 为后续研究打下基础

---

## 许可证

各项目遵循各自的许可证，具体信息请参考各项目目录下的 LICENSE 文件。

本仓库仅用于学习与科研交流。

---

## 致谢

感谢以下论文作者的开源贡献：
- SINet-V2: https://github.com/GewelsJI/SINet-V2
- CIRNet: 原作者提供的开源代码
