# 论文复现项目仓库

本仓库包含多个计算机视觉论文复现项目，主要用于学习和研究伪装目标检测（COD）和 RGB-D 显著目标检测（RGB-D SOD）方向。

## 项目背景

论文复现是深度学习研究中的重要环节，通过复现经典论文的代码和实验，可以深入理解模型架构、掌握实验设计方法，并为后续研究打下基础。

## 复现项目列表

| 项目 | 任务 | 论文 | 会议/期刊 | 年份 |
|------|------|------|----------|------|
| SINet-V2 | 伪装目标检测 (COD) | Concealed Object Detection | TPAMI | 2022 |
| CIRNet | RGB-D 显著目标检测 | CIR-Net: Cross-modality Interaction and Refinement | TIP | 2022 |

## 项目结构

```
Projects_reproduction/
├── README.md                    # 本文件
├── SINet_V2/
│   └── SINet_V2_main/           # SINet-V2 复现项目
│       ├── README.md            # SINet-V2 复现详细说明
│       ├── lib/                 # 网络模块
│       ├── utils/               # 工具函数
│       ├── jittor_lib/          # Jittor 版本
│       ├── MyTesting.py         # 测试脚本
│       └── MyTrain_Val.py       # 训练脚本
└── CIRNet/                      # CIRNet 复现项目
    ├── README.md                # CIRNet 复现详细说明
    ├── backbone/                # 骨干网络
    ├── model/                   # 模型定义
    ├── module/                  # 功能模块
    ├── CIRNet_train.py          # 训练脚本
    └── CIRNet_test.py           # 测试脚本
```

## 各项目详细复现过程

### SINet-V2 复现过程

详细复现步骤请参考：[SINet-V2 README](./SINet_V2/SINet_V2_main/README.md)

**核心要点：**
- 模型核心思想：Search（搜索）→ Identification（识别）
- 关键模块：邻居连接解码器(NCD)、组反转注意力(GRA)
- 支持 PyTorch 和 Jittor 两种框架
- 在多个 COD 数据集上达到 SOTA 性能

### CIRNet 复现过程

详细复现步骤请参考：[CIRNet README](./CIRNet/README.md)

**核心要点：**
- 模型核心思想：Cross-modality Interaction（跨模态交互）→ Refinement（细化）
- 支持 ResNet50 和 VGG16 两种骨干网络
- 融合 RGB 和 Depth 两种模态信息
- 在 6 个 RGB-D SOD 数据集上进行测试

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

## 许可证

各项目遵循各自的许可证，具体信息请参考各项目目录下的 LICENSE 文件。

本仓库仅用于学习与科研交流。

## 致谢

感谢以下论文作者的开源贡献：
- SINet-V2: https://github.com/GewelsJI/SINet-V2
- CIRNet: 原作者提供的开源代码