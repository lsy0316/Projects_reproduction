# SINet-V2 论文复现与实验记录

本仓库为 SINet-V2 论文与代码的复现项目，主要用于学习和研究伪装目标检测 / 隐蔽目标检测（Camouflaged Object Detection / Concealed Object Detection, COD）方向。

本项目基于论文 *Concealed Object Detection* 及其开源代码进行复现，完成了模型代码整理、预训练权重测试、数据集推理、评价指标计算与实验结果记录。

## 1. 项目简介

伪装目标检测旨在从复杂背景中分割出与环境高度相似的目标。与普通目标检测或显著性目标检测不同，伪装目标通常在颜色、纹理、边缘和形状上与背景高度融合，因此检测难度更高。

SINet-V2 是针对 COD 任务提出的经典模型之一，核心思想来自生物捕食过程中的：

- **Search**：先搜索可能存在伪装目标的区域；
- **Identification**：再对目标区域进行精细识别与分割。

本仓库主要用于个人科研训练和论文复现，为后续研究伪装目标检测、跨模态伪装目标检测、RGB-D COD、CoCOD 等方向打基础。

## 2. 论文信息

| 项目 | 内容 |
|------|------|
| 论文名称 | Concealed Object Detection |
| 模型名称 | SINet-V2 |
| 任务方向 | Camouflaged Object Detection / Concealed Object Detection |
| 发表期刊 | IEEE Transactions on Pattern Analysis and Machine Intelligence (TPAMI) |
| 发表年份 | 2022 |
| 官方仓库 | https://github.com/GewelsJI/SINet-V2 |

## 3. 项目结构

```
SINet-V2-main/
├── Src/
│   ├── SINet_V2.py              # SINet-V2 主体网络结构
│   ├── Res2Net_v1b.py           # Res2Net 主干网络
│   ├── Dataset.py               # 数据读取与预处理
│   └── ...
├── Dataset/
│   ├── TestDataset/
│   │   ├── CAMO/
│   │   ├── CHAMELEON/
│   │   └── COD10K/
│   └── TrainDataset/
├── snapshot/
│   ├── SINet_V2/
│   │   └── Net_epoch_best.pth   # 模型权重
│   └── Res2Net_v1b/
│       └── res2net50_v1b_26w_4s-3cf99910.pth
├── Result/
│   └── SINet_V2/                # 测试预测结果
├── jittor_lib/                  # Jittor 版本实现
│   ├── lib/
│   ├── utils/
│   └── MyTesting.py
├── MyTesting.py                 # 自定义测试脚本
├── MyTrain_Val.py               # 训练脚本
├── utils/                       # 工具函数
└── README.md
```

## 4. 环境配置

建议使用 Conda 创建独立环境。

```bash
conda create -n sinetv2 python=3.10
conda activate sinetv2
```

安装 PyTorch：

```bash
pip install torch torchvision torchaudio
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

```
snapshot/
├── Res2Net_v1b/
│   └── res2net50_v1b_26w_4s-3cf99910.pth
└── SINet_V2/
    └── Net_epoch_best.pth
```

请手动下载预训练权重，并按照上述目录结构放置。

下载链接：
- 模型权重：[Google Drive](https://drive.google.com/file/d/1D3RKQ8Nzd0ArV_c47StVKEuaoYTwnclR/view?usp=sharing)
- Res2Net 权重：[Google Drive](https://drive.google.com/file/d/1QumnqSY_2wa-81-Ti0X1-jQzaGDIfa9r/view?usp=sharing)

同时建议在 `.gitignore` 中加入：

```
*.pth
*.pt
snapshot/
Result/
__pycache__/
.DS_Store
```

## 6. 数据集准备

测试时使用常见 COD 数据集，例如：

```
Dataset/
└── TestDataset/
    ├── CAMO/
    │   ├── Imgs/
    │   └── GT/
    ├── CHAMELEON/
    │   ├── Imgs/
    │   └── GT/
    └── COD10K/
        ├── Imgs/
        └── GT/
```

下载链接：
- 测试数据集：[Google Drive](https://drive.google.com/file/d/1V0iSEdYJrT0Y_DHZfVGMg6TySFRNTy4o/view?usp=sharing)
- 训练/验证数据集：[Google Drive](https://drive.google.com/file/d/1M8-Ivd33KslvyehLK9_IUBGJ_Kf52bWG/view?usp=sharing)

## 7. 模型测试

运行测试脚本：

```bash
python MyTesting.py
```

测试完成后，预测结果通常会保存在：

```
Result/SINet_V2/
```

可以根据不同数据集分别生成预测图，用于后续指标评估和可视化分析。

## 8. 指标评估

本项目使用以下 COD / SOD 常见评价指标：

- **S-measure**：结构相似性度量
- **E-measure**：基于局部估计的相似性度量
- **F-measure**：精确率和召回率的调和平均
- **Weighted F-measure**：加权 F-measure
- **MAE**：平均绝对误差

评价工具可使用 [py-sod-metrics](https://github.com/lartpang/py-sod-metrics)：

```bash
pip install py-sod-metrics
```

## 9. Jittor 版本

本仓库同时包含 Jittor 框架版本的实现，位于 `jittor_lib/` 目录下。

Jittor 版本优势：推理效率更高，在某些配置下可获得 1.3x~1.5x 的加速比。

详情请参考 `jittor_lib/README.md`。

## 10. 复现工作内容

本项目主要完成了以下工作：

1. 阅读并整理 SINet-V2 论文核心思想
2. 复现并运行 SINet-V2 官方代码
3. 配置 PyTorch、OpenCV、py-sod-metrics 等实验环境
4. 下载并整理 CAMO、CHAMELEON、COD10K 等测试数据集
5. 使用预训练权重完成模型推理
6. 编写并整理自定义测试脚本 MyTesting.py
7. 记录并分析模型在不同数据集上的实验指标
8. 为后续伪装目标检测方向的模型改进与论文研究提供实验基础

## 11. Acknowledgement

本项目参考了 SINet-V2 官方开源代码，仅用于个人学习、论文复现和科研训练。

感谢原作者在 COD 数据集、基准测试和 SINet 系列模型方面所做的工作。

官方仓库：https://github.com/GewelsJI/SINet-V2

## 12. License

本仓库仅用于学习与科研交流。

如需进行商业使用、论文发表或二次开发，请遵循原论文和官方仓库的相关许可说明。
