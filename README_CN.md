# 迈向统一多模态生成的视觉新范式

<p align="center">
  <a href="https://huggingface.co/sensenova/SenseNova-Vision-7B-MoT">English</a> | <strong>简体中文</strong>
</p>

<p align="center">
  <a href="https://github.com/OpenSenseNova/SenseNova-Vision">
    <img alt="GitHub Stars" src="https://img.shields.io/github/stars/OpenSenseNova/SenseNova-Vision?style=social">
  </a>
  <a href="https://arxiv.org/abs/2607.06560">
    <img alt="arXiv论文" src="https://img.shields.io/badge/arXiv-2607.06560-b31b1b.svg">
  </a>
  <a href="https://huggingface.co/sensenova/SenseNova-Vision-7B-MoT">
    <img src="https://img.shields.io/static/v1?label=🤗 HuggingFace&message=模型权重&color=green">
  </a>
  <a href="https://huggingface.co/datasets/sensenova/SenseNova-Vision-Corpus-50M">
    <img src="https://img.shields.io/static/v1?label=🤗 HuggingFace&message=数据集&color=yellow">
  </a>
  <a href="./LICENSE">
    <img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="开源协议">
  </a>
</p>

<p align="center">
  <img src="./assets/fig1_one_case_for_all.webp" alt="SenseNova-Vision单模型统一处理各类视觉任务" width="900">
</p>

<p align="center">
  <img src="./assets/fig2_system_overview.webp" alt="SenseNova-Vision 整体系统架构图" width="900">
</p>

## 🌟 概述
SenseNova-Vision 是面向计算机视觉领域的统一多模态大模型。它摒弃传统方案为每个任务单独设计预测头、解码器与损失函数的思路，将异构视觉感知任务统一建模为**文本生成**、**图像生成**或**图文混合生成**。模型通过一套统一的指令跟随交互接口，同时支持结构化视觉理解、稠密几何预测、图像分割与多视图几何重建四大类视觉能力。

## 🚀 模型介绍
SenseNova-Vision 重新定义计算机视觉范式：**统一多模态生成**。
传统计算机视觉系统针对检测、分割、深度、法向、三维几何等任务分别搭建专属预测分支；而本模型将所有异构视觉任务，映射至多模态模型原生的输入输出空间中。

用户通过自然语言指令、可选视觉提示框，指定目标任务、关注区域、多视图像、输出格式与解码规则，模型会根据任务类型输出三类不同结果：

| 输出类型 | 典型任务 | 输出形式 |
|---|---|---|
| 结构化文本 | 目标检测、指代定位、OCR文字识别、GUI界面定位、关键点检测、相机内外参 | 带归一化坐标与轻量化结构化标记的文本记录 |
| 稠密图像图 | 单目深度估计、表面法向预测、点云图、二值掩码、彩色语义掩码 | 类图像稠密预测图 |
| 图文混合输出 | 多实例分割、对话式指代分割、组合视觉感知 | 文本类别标签 + 生成掩码 / 可视化几何图 |

该统一范式让单模型可完整覆盖结构化视觉理解、稠密几何预测、分割、多视图几何重建，且所有输出均可解码适配主流评测基准。

## 🌐 核心优势
- **统一视觉任务建模**：全部异构视觉任务均可转化为多模态模型原生的文本、图像、图文混合生成任务。
- **无任务专属分支**：不单独搭建检测、分割、深度、法向、三维几何专用预测头。
- **输出可标准解码**：生成文本与图像可反向解析为评测标准所需包围框、关键点、识别文字、掩码、深度图、法向图、点云、相机参数。
- **全场景任务覆盖**：同一模型一站式完成结构化视觉理解、图像分割、稠密几何预测、多视图三维几何重建。
- **指令自定义任务**：依靠自然语言指令灵活定义任务，突破固定评测数据集的预设范式限制。

## 🛠️ 使用方法

请使用 SenseNova-Vision 官方 GitHub 仓库中的推理代码：

```bash
git clone https://github.com/OpenSenseNova/SenseNova-Vision.git
cd SenseNova-Vision
```

### 环境配置

请在仓库根目录下创建运行环境：

```bash
bash setup.sh sensenova-vision
conda activate sensenova-vision
```

### 下载模型

你可以使用 `huggingface_hub` 从 Hugging Face 下载模型权重：

```python
from huggingface_hub import snapshot_download

model_path = snapshot_download("sensenova/SenseNova-Vision-7B-MoT")
print(model_path)
```

输出的 `model_path` 即为本地模型权重路径，可在后续推理中作为模型路径使用。

### 运行示例

```bash
bash scripts/run_sensenova_vision.sh example
```

### 运行单次推理请求

例如，下面的命令会对输入图像中的 `"person"` 类别进行二值分割：

```bash
bash scripts/run_sensenova_vision.sh inference \
  binary_seg \
  "person" \
  examples/images/2.jpg
```

### 🏗️ 核心创新点
- 🔗 提出统一多模态生成建模框架，将各类异构计算机视觉任务映射至统一多模态模型的原生输入输出空间。
- 🧩 构建大规模视觉指令数据集 SenseNova-Vision-Corpus，样本包含可解码文本、稠密图像、图文混合三类标注目标。
- ✨ 训练得到 SenseNova-Vision 统一视觉大模型，在结构化视觉理解、稠密几何预测、分割、多视图几何重建四大赛道均取得优异性能；同时支持通过自然语言自定义任务，不受固定评测集约束。

## 🏆 评测指标结果
我们在结构化视觉理解、稠密几何预测、图像分割、多视图几何重建四大方向开展全面评测。所有任务均使用自然语言指令定义：文本类输出解析为评测所需包围框、关键点、识别文字、相机参数；图像类输出解码为掩码、深度图、表面法向图、三维点云图。

### 结构化视觉理解
本赛道评测输出可表示为结构化文本的任务，包含框/点定位、指代检测、文字识别定位、GUI界面定位、人体关键点检测。

<table>
  <thead>
    <tr>
      <th align="center" rowspan="3">方法</th>
      <th align="center" colspan="6">目标检测</th>
      <th align="center" colspan="2">OCR文字检测</th>
      <th align="center">GUI界面定位</th>
      <th align="center">人体关键点</th>
    </tr>
    <tr>
      <th align="center">COCO通用</th>
      <th align="center">HR/RefCOCOg 视觉/文本指代</th>
      <th align="center">LVIS长尾检测</th>
      <th align="center">Dense200稠密检测</th>
      <th align="center" colspan="2">VisDrone航拍检测</th>
      <th align="center">HierText分层文字</th>
      <th align="center">ICDAR15</th>
      <th align="center">ScreenSpot-V2</th>
      <th align="center">COCO人体关键点</th>
    </tr>
    <tr>
      <th align="center">框mAP</th>
      <th align="center">框mAP</th>
      <th align="center">框mAP</th>
      <th align="center">框mAP</th>
      <th align="center">框mAP</th>
      <th align="center">点mAP</th>
      <th align="center">框mAP</th>
      <th align="center">框mAP</th>
      <th align="center">框mAP</th>
      <th align="center">点mAP</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Grounding DINO-Swin-T</td>
      <td><strong>56.6</strong></td>
      <td>25.2 / 45.9 / 46.8</td>
      <td>38.8</td>
      <td>33.1</td>
      <td><u>38.5</u></td>
      <td>--</td>
      <td>--</td>
      <td>--</td>
      <td>--</td>
      <td>--</td>
    </tr>
    <tr>
      <td>Bagel</td>
      <td>50.2</td>
      <td>74.6 / 76.4 / <u>77.8</u></td>
      <td>46.8</td>
      <td>42.4</td>
      <td>23.0</td>
      <td>36.9</td>
      <td>7.1</td>
      <td>15.8</td>
      <td>81.1</td>
      <td>--</td>
    </tr>
    <tr>
      <td>Qwen3-VL-8B-Instruct</td>
      <td>46.6</td>
      <td>70.4 / 72.3 / 72.6</td>
      <td>43.2</td>
      <td>13.5</td>
      <td>28.7</td>
      <td>35.7</td>
      <td>22.4</td>
      <td>25.4</td>
      <td><u>90.5</u></td>
      <td>--</td>
    </tr>
    <tr>
      <td>Qwen3.5-9B</td>
      <td>49.3</td>
      <td>71.7 / 72.1 / 72.6</td>
      <td>43.2</td>
      <td>27.5</td>
      <td>26.8</td>
      <td>41.7</td>
      <td>19.6</td>
      <td>11.4</td>
      <td><strong>92.2</strong></td>
      <td>--</td>
    </tr>
    <tr>
      <td>LocateAnything</td>
      <td><u>54.7</u></td>
      <td>78.7 / <u>76.7</u> / 77.6</td>
      <td><u>50.7</u></td>
      <td><u>58.7</u></td>
      <td><u>39.9</u></td>
      <td><u>60.4</u></td>
      <td><u>29.1</u></td>
      <td>26.4</td>
      <td>85.5</td>
      <td>--</td>
    </tr>
    <tr>
      <td>Rex-Omni</td>
      <td>52.9</td>
      <td><u>79.9</u> / 73.6 / 74.3</td>
      <td>46.9</td>
      <td>58.3</td>
      <td>35.8</td>
      <td>58.9</td>
      <td>28.0</td>
      <td><u>28.1</u></td>
      <td>88.4</td>
      <td><u>32.6</u></td>
    </tr>
    <tr>
      <td>SenseNova-Vision</td>
      <td><strong>56.6</strong></td>
      <td><strong>80.2</strong> / <strong>79.6</strong> / <strong>80.5</strong></td>
      <td><strong>54.8</strong></td>
      <td><strong>66.8</strong></td>
      <td><strong>43.3</strong></td>
      <td><strong>62.9</strong></td>
      <td><strong>31.2</strong></td>
      <td><strong>49.5</strong></td>
      <td>85.9</td>
      <td><strong>34.6</strong></td>
    </tr>
  </tbody>
</table>

### 稠密几何预测
本赛道评测像素对齐几何输出，包含单目深度估计、表面法向预测。

<table>
  <thead>
    <tr>
      <th align="center" rowspan="3">方法</th>
      <th align="center" colspan="5">深度估计</th>
      <th align="center" colspan="3">表面法向估计</th>
    </tr>
    <tr>
      <th align="center">NYUv2室内</th>
      <th align="center">KITTI自动驾驶</th>
      <th align="center">ETH3D实景重建</th>
      <th align="center">ScanNet室内重建</th>
      <th align="center">DIODE室外深度</th>
      <th align="center">ScanNet</th>
      <th align="center">iBims-1</th>
      <th align="center">NYUv2</th>
    </tr>
    <tr>
      <th align="center" colspan="5">AbsRel误差↓ / δ1精度↑</th>
      <th align="center" colspan="3">平均角度误差↓ / 11.25°内占比↑</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DSINE</td>
      <td>--</td><td>--</td><td>--</td><td>--</td><td>--</td>
      <td>16.2 / 61.0</td><td>17.1 / 67.4</td><td>16.4 / 59.6</td>
    </tr>
    <tr>
      <td>DepthAnything</td>
      <td>4.3 / <strong>98.1</strong></td><td>7.6 / 94.7</td><td>12.7 / 88.2</td><td>4.3 / 98.1</td><td>26.0 / 75.9</td>
      <td>--</td><td>--</td><td>--</td>
    </tr>
    <tr>
      <td>DepthAnything V2</td>
      <td>4.5 / 97.9</td><td>7.4 / 94.6</td><td>13.1 / 86.5</td><td>4.2 / 97.8</td><td>26.5 / 73.4</td>
      <td>--</td><td>--</td><td>--</td>
    </tr>
    <tr>
      <td>*MoGe-2</td>
      <td><strong>3.5</strong> / 98.0</td><td><strong>5.5</strong> / <strong>97.7</strong></td><td><strong>3.4</strong> / <strong>98.8</strong></td><td><strong>3.4</strong> / <strong>98.3</strong></td><td><strong>23.0</strong> / <strong>82.3</strong></td>
      <td><strong>12.8</strong> / <strong>68.4</strong></td><td><strong>14.7</strong> / <strong>70.4</strong></td><td><strong>14.7</strong> / <strong>62.3</strong></td>
    </tr>
    <tr>
      <td>Marigold</td>
      <td>5.5 / 96.4</td><td>9.9 / 91.6</td><td>6.5 / 95.9</td><td>6.4 / 95.2</td><td>30.8 / <u>77.3</u></td>
      <td>21.3 / 45.6</td><td>18.5 / 64.7</td><td>20.9 / 50.5</td>
    </tr>
    <tr>
      <td>DICEPTION</td>
      <td>6.1 / 96.0</td><td>6.9 / 94.9</td><td>5.0 / 97.5</td><td>7.2 / 94.4</td><td>28.9 / 72.2</td>
      <td>18.8 / 53.6</td><td>--</td><td>18.3 / 52.9</td>
    </tr>
    <tr>
      <td>FE2E</td>
      <td><u>4.1</u> / <u>97.7</u></td><td><u>6.6</u> / <strong>96.0</strong></td><td><strong>3.8</strong> / <strong>98.7</strong></td><td>4.4 / 97.5</td><td>22.8 / <strong>81.2</strong></td>
      <td><u>13.8</u> / <u>67.2</u></td><td><strong>15.1</strong> / <strong>70.6</strong></td><td><u>16.2</u> / <u>59.6</u></td>
    </tr>
    <tr>
      <td>Lotus-2</td>
      <td><u>4.1</u> / 97.6</td><td>6.7 / 94.5</td><td>4.6 / <u>98.1</u></td><td><u>4.2</u> / <u>97.6</u></td><td><u>22.1</u> / 75.2</td>
      <td>14.2 / 66.8</td><td><u>15.4</u> / <u>70.4</u></td><td>16.9 / 59.0</td>
    </tr>
    <tr>
      <td>SenseNova-Vision</td>
      <td><strong>4.0</strong> / <strong>98.1</strong></td><td><strong>5.9</strong> / <u>95.9</u></td><td><u>4.3</u> / 97.4</td><td><strong>3.9</strong> / <strong>98.0</strong></td><td><strong>20.6</strong> / 76.4</td>
      <td><strong>12.8</strong> / <strong>68.9</strong></td><td><u>15.4</u> / 69.1</td><td><strong>14.4</strong> / <strong>62.7</strong></td>
    </tr>
  </tbody>
</table>

### 图像分割
评测语义分割、指代分割、推理分割、对话引导分割、交互式点/框分割的掩码预测效果。

<table>
  <thead>
    <tr>
      <th align="center" rowspan="2">方法</th>
      <th align="center">通用分割</th>
      <th align="center">指代分割</th>
      <th align="center">推理分割</th>
      <th align="center">对话分割</th>
      <th align="center">交互式分割</th>
    </tr>
    <tr>
      <th align="center">全景IoU / 语义IoU</th>
      <th align="center">RefCOCO / RefCOCO+ / RefCOCOg cIoU</th>
      <th align="center">验证集 / 测试集 gIoU</th>
      <th align="center">验证集 / 测试集 gIoU</th>
      <th align="center">点输入 / 框输入 mIoU</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>LISA-7B</td><td>--</td><td>74.9 / 65.1 / 67.9</td><td>52.9 / 47.3</td><td>62.0 / 61.7</td><td>--</td></tr>
    <tr><td>PSALM</td><td><strong>55.9</strong> / <strong>66.6</strong></td><td>83.6 / 72.9 / 73.8</td><td>--</td><td>--</td><td><u>64.3</u> / 67.3</td></tr>
    <tr><td>Text4Seg</td><td>--</td><td>79.2 / 72.8 / 74.0</td><td>59.1 / 57.1</td><td>--</td><td>--</td></tr>
    <tr><td>LENS</td><td>--</td><td><u>84.2</u> / <strong>79.4</strong> / <u>81.2</u></td><td><u>62.1</u> / 57.2</td><td>--</td><td>--</td></tr>
    <tr><td>ConverSeg</td><td>--</td><td>79.4 / 74.3 / 74.9</td><td>61.9 / 57.0</td><td>--</td><td>--</td></tr>
    <tr><td>X-SAM</td><td><u>54.7</u> / <u>66.5</u></td><td><strong>85.1</strong> / <u>78.0</u> / <strong>83.8</strong></td><td>56.6 / <u>57.8</u></td><td><strong>69.4</strong> / <strong>69.0</strong></td><td><strong>65.4</strong> / <u>70.0</u></td></tr>
    <tr><td>SenseNova-Vision</td><td>48.8 / 64.0</td><td>81.3 / 76.0 / 80.3</td><td><strong>63.2</strong> / <strong>60.7</strong></td><td><u>65.7</u> / <u>66.2</u></td><td>60.9 / <strong>73.9</strong></td></tr>
  </tbody>
</table>

### 多视图视觉几何
基于多张输入图像完成几何预测，包含多视图点云重建、相机位姿估计。

<table>
  <thead>
    <tr>
      <th align="center" rowspan="3">方法</th>
      <th align="center" colspan="2">多视图重建</th>
      <th align="center" colspan="2">相机位姿估计</th>
    </tr>
    <tr>
      <th align="center" colspan="2">精度误差↓ / 完整度误差↓ / F1分数↑</th>
      <th align="center" colspan="2">旋转精度RRA@30↑ / 平移RTA@30↑ / AUC@30↑</th>
    </tr>
    <tr>
      <th align="center">7Scenes室内场景</th>
      <th align="center">ETH3D实景数据集</th>
      <th align="center">Re10K</th>
      <th align="center">CO3Dv2</th>
    </tr>
  </thead>
  <tbody>
    <tr><td>DUSt3R</td><td>0.026 / 0.034 / 87.1</td><td>0.359 / 0.531 / 66.6</td><td>99.8 / 84.9 / 67.6</td><td>97.7 / 93.4 / 78.3</td></tr>
    <tr><td>DepthAnything3</td><td><strong>0.020</strong> / <strong>0.026</strong> / <strong>90.5</strong></td><td>0.228 / 0.212 / 76.6</td><td><strong>100.0</strong> / <strong>96.4</strong> / <strong>89.6</strong></td><td><strong>99.3</strong> / <strong>98.0</strong> / <strong>91.8</strong></td></tr>
    <tr><td>VGGT</td><td>0.023 / 0.032 / 88.4</td><td><strong>0.177</strong> / <strong>0.155</strong> / <strong>80.9</strong></td><td><strong>100.0</strong> / 93.5 / 79.3</td><td>98.3 / 96.6 / 89.2</td></tr>
    <tr><td>MoRe</td><td>0.038 / 0.039 / 77.1</td><td>0.348 / 0.318 / 62.7</td><td><strong>100.0</strong> / 94.0 / 79.1</td><td>98.4 / 96.3 / 83.0</td></tr>
    <tr><td>MapAnything</td><td><strong>0.027</strong> / 0.029 / 87.8</td><td>0.400 / 0.524 / 67.0</td><td><strong>100.0</strong> / 93.5 / <strong>80.7</strong></td><td>95.5 / 91.6 / 70.9</td></tr>
    <tr><td>G2VLM</td><td>0.084 / 0.056 / 59.2</td><td>0.784 / 0.553 / 36.7</td><td>99.8 / 77.5 / 51.8</td><td>96.3 / 92.0 / 55.2</td></tr>
    <tr><td>SenseNova-Vision</td><td>0.028 / <strong>0.026</strong> / <strong>87.9</strong></td><td><strong>0.301</strong> / <strong>0.175</strong> / <strong>72.2</strong></td><td>99.8 / <strong>94.2</strong> / 77.3</td><td><strong>97.4</strong> / <strong>95.4</strong> / <strong>80.1</strong></td></tr>
  </tbody>
</table>

### 通用视觉大模型横向对比
将本模型与近年通用全能视觉模型，在多类视觉任务上横向对比：

<table>
  <thead>
    <tr><th align="center" rowspan="3">方法</th><th align="center">目标检测</th><th align="center">语义分割</th><th align="center">指代分割</th><th align="center">深度估计</th></tr>
    <tr><th align="center">mAP</th><th align="center">mIoU</th><th align="center">cIoU</th><th align="center">δ1精度</th></tr>
    <tr><th align="center">COCO数据集</th><th align="center">Cityscapes城市场景</th><th align="center">RefCOCO / + / g</th><th align="center">NYUv2室内</th></tr>
  </thead>
  <tbody>
    <tr><td>Youtu-VL</td><td>47.1</td><td>70.4</td><td>80.7 / <strong>76.2</strong> / 76.5</td><td>90.4</td></tr>
    <tr><td>SenseNova-Vision</td><td><strong>53.7</strong></td><td><strong>71.2</strong></td><td><strong>81.3</strong> / 76.0 / <strong>80.3</strong></td><td><strong>98.1</strong></td></tr>
  </tbody>
</table>

<table>
  <thead>
    <tr><th align="center" rowspan="3">方法</th><th align="center">语义分割</th><th align="center">指代分割</th><th align="center">推理分割</th><th align="center" colspan="4">深度估计</th><th align="center" colspan="3">表面法向</th></tr>
    <tr><th align="center">mIoU</th><th align="center">cIoU</th><th align="center">gIoU</th><th align="center" colspan="4">δ1精度</th><th align="center" colspan="3">平均角度误差↓</th></tr>
    <tr><th align="center">Cityscapes</th><th align="center">RefCOCOg</th><th align="center">ReasonSeg</th><th align="center">KITTI</th><th align="center">NYUv2</th><th align="center">DIODE</th><th align="center">ETH3D</th><th align="center">NYUv2</th><th align="center">ScanNet</th><th align="center">DIODE</th></tr>
  </thead>
  <tbody>
    <tr><td>Vision Banana</td><td>69.9</td><td>73.8</td><td>79.3</td><td>91.5</td><td>94.8</td><td>91.7</td><td>93.5</td><td>17.8</td><td>15.1</td><td><strong>13.8</strong></td></tr>
    <tr><td>SenseNova-Vision</td><td><strong>71.2</strong></td><td><strong>80.3</strong></td><td>63.2</td><td>95.9</td><td>98.1</td><td>76.4</td><td>97.4</td><td><strong>14.4</strong></td><td><strong>12.8</strong></td><td>15.3</td></tr>
  </tbody>
</table>

### 通用多模态综合能力
模型在适配各类视觉感知任务的同时，基本保留通用图文理解与生成能力。

<table>
  <thead>
    <tr><th align="center" rowspan="2">方法</th><th align="center" colspan="3">跨模态理解</th><th align="center" colspan="2">图文生成</th></tr>
    <tr><th align="center">MMMU多模态问答</th><th align="center">MMVP视觉推理</th><th align="center">MathVista数学图文</th><th align="center">GenEval生成评测</th><th align="center">WISE图文对齐</th></tr>
  </thead>
  <tbody>
    <tr><td>Bagel</td><td>0.55</td><td>69.3</td><td>73.1</td><td>0.82</td><td>0.52</td></tr>
    <tr><td>SenseNova-Vision</td><td>0.42</td><td>79.0</td><td>67.7</td><td>0.85</td><td>0.45</td></tr>
  </tbody>
</table>

## 🖼️ 可视化效果示例
<p align="center">
  <img src="./assets/fig4_sensenova_vision_results.webp" alt="SenseNova-Vision 全任务可视化输出样例" width="900">
</p>

## 📚 训练数据集
SenseNova-Vision 使用 **SenseNova-Vision-Corpus-50M** 大规模视觉指令数据集训练。该数据集将各类异构标注统一转换为标准化样本格式：输入图像 + 自然语言指令 + 可解码标注目标（文本/稠密图像/图文混合）。

数据集分为四大任务大类：

| 任务大类 | 代表性子任务 | 标注目标格式 |
|---|---|---|
| 结构化视觉理解 | 目标检测、指代定位、点标注、关键点、OCR文字、布局解析、GUI界面定位 | 带归一化坐标与轻量化结构标记的文本记录 |
| 稠密几何预测 | 单目深度估计、表面法向预测 | 固定编码规则生成稠密图像图 |
| 图像分割 | 指代分割、推理分割、交互式分割、通用语义分割、对话引导分割 | 二值掩码、彩色语义掩码、图文混合输出 |
| 多视图视觉几何 | 多视图点云重建、相机位姿估计 | 类图像点云图 + 结构化相机参数文本 |


## ⚠️ 局限性
- **非专项最优模型**：本模型覆盖全品类视觉任务，但针对单一细分场景的专用模型，在部分小众评测集上性能可能超越本模型。
- **输出解析依赖任务脚本**：文本类输出需要对应任务专用解析器，稠密图像输出必须遵循训练时的解码规则。
- **几何指标精度不保证**：深度、法向、点云、相机位姿等稠密几何预测结果，下游落地前务必独立校验精度。
- **提示词敏感**：作为指令跟随模型，任务表现会受指令措辞、输出格式定义、视觉提示框输入方式影响。
- **数据集与评测偏差**：模型行为会继承训练数据集的数据分布、标注习惯带来的固有偏置。

## 🛡️ 伦理规范
SenseNova-Vision 可能输出错误的目标定位、分割掩码、深度、表面法向或相机位姿结果。**严禁在无独立校验的安全关键场景直接部署**。
若使用包含人像、人脸、证件文档、医疗影像、监控画面、私密环境的数据集，使用者需自行遵守当地隐私法规、知情同意要求与数据管理规范。

## ✒️ 引用

如果本项目对你的研究有帮助，请引用以下技术报告：

```bibtex
@misc{han2026visionunifiedmultimodalgeneration,
  title={Vision as Unified Multimodal Generation},
  author={Xiaoyang Han and Jianhua Li and Kewang Deng and Zukai Chen and Xuanke Shi and Sihan Wang and Boxuan Li and Linyan Wang and Siyi Xie and Xin You and Jinsheng Quan and Zhongang Cai and Haiwen Diao and Ziwei Liu and Lei Yang and Dahua Lin and Quan Wang},
  year={2026},
  eprint={2607.06560},
  archivePrefix={arXiv},
  primaryClass={cs.CV},
  url={https://arxiv.org/abs/2607.06560}
}
```

## 📜 开源协议
模型权重基于 CC BY-NC 4.0 许可证发布，仅限非商业用途。

官方 GitHub 仓库中的源代码可能遵循不同的许可证。代码使用请参考对应代码仓库中的许可证说明。第三方数据集、工具和资源仍受其原始许可证和使用条款约束。

## 📮 联系我们
如有疑问、Bug反馈、科研合作需求，可前往官方项目仓库提交Issue；正式发布后也可通过项目主页联系论文作者。