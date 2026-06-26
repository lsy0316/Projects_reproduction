# CIRNet 论文复现与实验记录

本仓库为 CIRNet 论文与代码的复现项目，主要用于学习和研究 RGB-D 显著目标检测（RGB-D Salient Object Detection, RGB-D SOD）方向。

本项目基于论文 *CIR-Net: Cross-modality Interaction and Refinement for RGB-D Salient Object Detection* 及其开源代码进行复现，完成了模型代码整理、预训练权重测试、数据集推理、评价指标计算与实验结果记录。

## 1. 项目简介

RGB-D 显著目标检测旨在利用 RGB 图像和深度信息（Depth）共同预测图像中的显著目标区域。与传统 RGB SOD 相比，深度信息提供了额外的几何结构信息，有助于更好地区分目标和背景。

CIRNet 是针对 RGB-D SOD 任务提出的经典模型之一，核心思想是：

- **Cross-modality Interaction（跨模态交互）**：在不同层级进行 RGB 和 Depth 模态的特征交互；
- **Refinement（细化）**：通过交叉模态细化模块逐步优化显著图。

本仓库主要用于个人科研训练和论文复现，为后续研究 RGB-D SOD、跨模态融合等方向打基础。

## 2. 论文信息

| 项目 | 内容 |
|------|------|
| 论文名称 | CIR-Net: Cross-modality Interaction and Refinement for RGB-D Salient Object Detection |
| 模型名称 | CIRNet |
| 任务方向 | RGB-D Salient Object Detection |
| 发表期刊 | IEEE Transactions on Image Processing (TIP) |
| 发表年份 | 2022 |
| 作者 | Runmin Cong, Qinwei Lin, Chen Zhang, Chongyi Li, Xiaochun Cao, Qingming Huang, Yao Zhao |

## 3. 项目结构

```
CIRNet/
├── backbone/                    # 骨干网络
│   ├── ResNet.py               # ResNet 骨干
│   └── vgg.py                  # VGG 骨干
├── model/                      # 模型定义
│   ├── CIRNet_Res50.py         # ResNet50 版本
│   └── CIRNet_vgg16.py         # VGG16 版本
├── module/                     # 功能模块
│   ├── BaseBlock.py            # 基础模块
│   ├── Decoder.py              # 解码器
│   └── cmWR.py                 # 交叉模态细化模块
├── dataLoader.py               # 数据加载
├── options.py                  # 参数配置
├── utils.py                    # 工具函数
├── CIRNet_train.py             # 训练脚本
├── CIRNet_test.py              # 测试脚本
└── README.md
```

## 4. 环境配置

建议使用 Conda 创建独立环境。

```bash
conda create -n cirnet python=3.7
conda activate cirnet
```

安装 PyTorch：

```bash
pip install torch==1.10.1 torchvision==0.11.2
```

安装其他依赖：

```bash
pip install numpy opencv-python pillow tqdm scipy scikit-image
pip install py-sod-metrics
```

如果服务器环境缺少 OpenCV 相关依赖，可安装：

```bash
sudo apt-get update
sudo apt-get install libgl1
```

## 5. 权重文件说明

由于 GitHub 单文件大小限制，以下权重文件不上传到本仓库：

- CIRNet_R50.pth（ResNet50 骨干预训练权重）
- CIRNet_VGG16.pth（VGG16 骨干预训练权重）

请手动下载预训练权重，并放置在项目根目录下。

下载链接：
- ResNet50 版本：[Baidu Cloud](https://pan.baidu.com/s/1QUoGbqgaZhalwJxoDOpL8A)（密码: 1234）
- VGG16 版本：[Baidu Cloud](https://pan.baidu.com/s/1tP3XFXhmAjC2Q3I8lC7TwQ)（密码: 1234）

同时建议在 `.gitignore` 中加入：

```
*.pth
*.pt
test_maps/
__pycache__/
.DS_Store
```

## 6. 数据集准备

测试时使用常见 RGB-D SOD 数据集，包括：

```
data/
├── train/                      # 训练数据 (RGB + Depth)
└── test/                       # 测试数据
    ├── STEREO797/
    ├── NLPR/
    ├── NJUD/
    ├── DUT/
    ├── LFSD/
    └── SIP/
```

下载链接：
- 训练数据：[Baidu Cloud](https://pan.baidu.com/s/1NFt3eSpdNA99DuP9O5zpHA)（密码:1234）
- 测试数据：[Baidu Cloud](https://pan.baidu.com/s/1KVCLaXLrMZDUZDpYBd_SJA)（密码:1234）

## 7. 模型测试

运行测试脚本：

```bash
# ResNet50 版本
python CIRNet_test.py --backbone R50 --test_model CIRNet_R50.pth

# VGG16 版本
python CIRNet_test.py --backbone VGG16 --test_model CIRNet_VGG16.pth
```

测试完成后，预测结果通常会保存在 `test_maps` 文件夹中。

## 8. 模型训练

运行训练脚本：

```bash
# ResNet50 版本
python CIRNet_train.py --backbone R50

# VGG16 版本
python CIRNet_train.py --backbone VGG16
```

## 9. 指标评估

本项目使用以下 RGB-D SOD 常见评价指标：

- **S-measure**：结构相似性度量
- **E-measure**：基于局部估计的相似性度量
- **F-measure**：精确率和召回率的调和平均
- **Weighted F-measure**：加权 F-measure
- **MAE**：平均绝对误差

评价工具可使用 [py-sod-metrics](https://github.com/lartpang/py-sod-metrics)：

```bash
pip install py-sod-metrics
```

## 10. 复现工作内容

本项目主要完成了以下工作：

1. 阅读并整理 CIRNet 论文核心思想
2. 复现并运行 CIRNet 官方代码
3. 配置 PyTorch、OpenCV、py-sod-metrics 等实验环境
4. 下载并整理 RGB-D SOD 测试数据集（STEREO797、NLPR、NJUD、DUT、LFSD、SIP）
5. 使用预训练权重完成模型推理
6. 编写并整理测试脚本
7. 记录并分析模型在不同数据集上的实验指标
8. 为后续 RGB-D SOD 方向的模型改进与论文研究提供实验基础

## 11. Citation

If you use our CIR-Net, please cite our paper:

```
@article{crm/tip22/CIRNet,
  title={{CIR-Net}: Cross-modality interaction and refinement for {RGB-D} salient object detection},
  author={Cong, Runmin and Lin, Qinwei and Zhang, Chen and Li, Chongyi and Cao, Xiaochun and Huang, Qingming and Zhao, Yao },
  journal={IEEE Trans. Image Process. },
  volume={31},
  pages={6800-6815},
  year={2022},
}
```

## 12. License

本仓库仅用于学习与科研交流。

如需进行商业使用、论文发表或二次开发，请遵循原论文和官方仓库的相关许可说明。