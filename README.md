---
license: cc-by-nc-4.0
language:
- en
pipeline_tag: image-text-to-text
library_name: transformers
inference: false
tags:
- multimodal
- vision-language
- image-generation
- image-editing
- segmentation
- depth-estimation
- normal-estimation
- dense-perception
- unified-multimodal-generation
---

[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC--BY--NC%204.0-lightgrey.svg)](./LICENSE)

# Vision as Unified Multimodal Generation

<p align="center">
  <strong>English</strong> | <a href="https://huggingface.co/sensenova/SenseNova-Vision-7B-MoT/blob/main/README_CN.md">简体中文</a>
</p>

<p align="center">
  <a href="https://github.com/OpenSenseNova/SenseNova-Vision"><img alt="GitHub Stars" src="https://img.shields.io/github/stars/OpenSenseNova/SenseNova-Vision?style=social"></a>
  <a href="https://arxiv.org/abs/2607.06560"><img alt="arXiv" src="https://img.shields.io/badge/arXiv-2607.06560-b31b1b.svg"></a>
  <a href="https://huggingface.co/sensenova/SenseNova-Vision-7B-MoT"><img src="https://img.shields.io/static/v1?label=%F0%9F%A4%97%20Hugging%20Face&message=Model&color=green"></a>
  <a href="https://huggingface.co/datasets/sensenova/SenseNova-Vision-Corpus-50M"><img src="https://img.shields.io/static/v1?label=%F0%9F%A4%97%20Hugging%20Face&message=Dataset&color=yellow"></a>

  <a href="./LICENSE"><img src="https://img.shields.io/badge/License-Apache%202.0-blue.svg" alt="License"></a>
</p>

<p align="center">
  <img src="./assets/fig1_one_case_for_all.webp" alt="SenseNova-Vision handles diverse vision tasks in a unified model" width="900">
</p>

<p align="center">
  <img src="./assets/fig2_system_overview.webp" alt="SenseNova-Vision system overview" width="900">
</p>

## 🌟 Overview
SenseNova-Vision is a unified multimodal model for computer vision. It reformulates heterogeneous visual perception tasks as **text generation**, **image generation**, or **mixed text-image generation**, instead of relying on task-specific heads, decoders, or loss functions for each individual task. The model supports structured visual understanding, dense geometric prediction, segmentation, and multi-view visual geometry within a shared instruction-following interface.


## 🚀 Model Description
SenseNova-Vision rethinks computer vision as **unified multimodal generation**. Traditional computer-vision systems usually attach task-specific prediction heads for detection, segmentation, depth, surface normal, or 3D geometry. SenseNova-Vision instead expresses these heterogeneous tasks through the native input-output spaces of a unified multimodal model.

Natural-language instructions and optional visual prompts specify the target task, regions, views, output schema, and decoding convention. The model then generates different target formats depending on the task:

| Target type | Representative tasks | Output form |
|---|---|---|
| Structured text | Detection, referring localization, OCR, GUI grounding, keypoints, camera parameters | Text records with normalized coordinates or structured fields |
| Dense image | Depth, surface normal, point maps, binary masks, color-coded masks | Image-like target maps |
| Mixed text-image | Multi-instance segmentation, grounded conversation segmentation, compositional perception | Text labels plus generated masks or visual maps |

This formulation allows a single model to cover structured visual understanding, dense geometric prediction, segmentation, and multi-view visual geometry while keeping outputs decodable for standard benchmarks.

## 🌐 Key Features

- **Unified vision-task formulation:** Heterogeneous computer-vision tasks are cast into the native text, image, and mixed generation spaces of a unified multimodal model.
- **No task-specific heads:** The model does not rely on separate detection, segmentation, depth, normal, or geometry heads.
- **Decodable outputs:** Generated text and images can be converted back into benchmark-compatible boxes, points, OCR strings, masks, depth maps, normal maps, point maps, and camera records.
- **Broad task coverage:** The same model handles structured visual understanding, segmentation, dense geometry, and multi-view visual geometry.
- **Instruction-defined task variants:** Natural-language instructions enable flexible task definitions beyond fixed benchmark schemas.

## 🛠️ How to Use

Please use the official inference code from the SenseNova-Vision GitHub repository:

```bash
git clone https://github.com/OpenSenseNova/SenseNova-Vision.git
cd SenseNova-Vision
```

### Environment Setup

Create the environment from the repository root:

```bash
bash setup.sh sensenova-vision
conda activate sensenova-vision
```

### Download the Model

You can download the model weights from Hugging Face with `huggingface_hub`:

```python
from huggingface_hub import snapshot_download

model_path = snapshot_download("sensenova/SenseNova-Vision-7B-MoT")
print(model_path)
```

The printed `model_path` points to the local checkpoint directory and can be used as the model path for inference.

### Run the Curated Example

We provide a curated example to quickly verify the environment and model setup:

```bash
bash scripts/run_sensenova_vision.sh example
```

### Run One Inference Request

You can also run a single inference request with the official wrapper.  
For example, the following command performs binary segmentation for the target category `"person"`:

```bash
bash scripts/run_sensenova_vision.sh inference \
  binary_seg \
  "person" \
  examples/images/2.jpg
```

### Launch the Web Demo

You can launch the Gradio web demo using the official wrapper provided in the repository.  
The wrapper will print the local URL before starting Gradio. Open the printed URL in your browser to interact with the model.

For more details, supported tasks, and additional examples, please refer to the official GitHub repository:

```text
https://github.com/OpenSenseNova/SenseNova-Vision
```
### 🏗️ Key Contributions

- 🔗 We introduce a unified multimodal generation formulation that casts heterogeneous computer vision tasks into the native input-output spaces of UMMs.
- 🧩 We construct the SenseNova-Vision Corpus, a large-scale computer-vision instruction-response corpus with decodable text, image, and mixed text-image targets.
- ✨ We train SenseNova-Vision and show strong results across structured visual understanding, dense geometric prediction, segmentation, and multi-view visual geometry, while supporting language-defined task variants beyond fixed benchmark schemas.

## 🏆 Benchmark Results

SenseNova-Vision is evaluated across structured visual understanding, dense geometric prediction, segmentation, and multi-view visual geometry. All tasks are formulated with natural-language instructions: textual outputs are parsed into benchmark-specific structures such as boxes, points, recognized text, keypoints, and camera parameters, while image outputs are decoded into masks, depth maps, normal maps, or 3D point maps.

### Structured Visual Understanding

Structured visual understanding evaluates tasks whose outputs can be represented as structured textual predictions, including box- and point-based localization, referring detection, OCR localization, GUI grounding, and keypoint localization.

<table>
  <thead>
    <tr>
      <th align="center" rowspan="3">Method</th>
      <th align="center" colspan="6">Object Detection</th>
      <th align="center" colspan="2">OCR</th>
      <th align="center">GUI</th>
      <th align="center">Keypoint</th>
    </tr>
    <tr>
      <th align="center">COCO-Com.</th>
      <th align="center">HR/RefCOCOg V/T</th>
      <th align="center">LVIS</th>
      <th align="center">Dense200</th>
      <th align="center" colspan="2">VisDrone</th>
      <th align="center">HierText</th>
      <th align="center">ICDAR15</th>
      <th align="center">ScreenSpot-V2</th>
      <th align="center">COCO-Kpt.</th>
    </tr>
    <tr>
      <th align="center">bbox</th>
      <th align="center">bbox</th>
      <th align="center">bbox</th>
      <th align="center">bbox</th>
      <th align="center">bbox</th>
      <th align="center">point</th>
      <th align="center">bbox</th>
      <th align="center">bbox</th>
      <th align="center">bbox</th>
      <th align="center">point</th>
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

### Dense Geometric Prediction

Dense geometric prediction evaluates pixel-aligned geometric outputs, including monocular depth estimation and surface normal estimation.

<table>
  <thead>
    <tr>
      <th align="center" rowspan="3">Method</th>
      <th align="center" colspan="5">Depth</th>
      <th align="center" colspan="3">Normal</th>
    </tr>
    <tr>
      <th align="center">NYUv2</th>
      <th align="center">KITTI</th>
      <th align="center">ETH3D</th>
      <th align="center">ScanNet</th>
      <th align="center">DIODE</th>
      <th align="center">ScanNet</th>
      <th align="center">iBims-1</th>
      <th align="center">NYUv2</th>
    </tr>
    <tr>
      <th align="center" colspan="5">AbsRel↓ / δ1↑</th>
      <th align="center" colspan="3">Mean↓ / 11.25°↑</th>
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

### Segmentation

Segmentation evaluates mask prediction under semantic, referring, reasoning, grounded, and interactive guidance.

<table>
  <thead>
    <tr>
      <th align="center" rowspan="2">Method</th>
      <th align="center">Gen. Seg.</th>
      <th align="center">Ref. Seg.</th>
      <th align="center">Rea. Seg.</th>
      <th align="center">GCG Seg.</th>
      <th align="center">Inter. Seg.</th>
    </tr>
    <tr>
      <th align="center">Pan. / Sem.</th>
      <th align="center">RefCOCO / + / g</th>
      <th align="center">Val / Test</th>
      <th align="center">Val / Test</th>
      <th align="center">Point / Box</th>
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

### Multi-View Visual Geometry

Multi-view visual geometry evaluates geometric prediction from multiple input images, including multi-view point map reconstruction and camera pose estimation.

<table>
  <thead>
    <tr>
      <th align="center" rowspan="3">Method</th>
      <th align="center" colspan="2">Multi-View Reconstruction</th>
      <th align="center" colspan="2">Camera Pose</th>
    </tr>
    <tr>
      <th align="center" colspan="2">Acc.↓ / Comp.↓ / F1↑</th>
      <th align="center" colspan="2">RRA@30↑ / RTA@30↑ / AUC@30↑</th>
    </tr>
    <tr>
      <th align="center">7Scenes</th>
      <th align="center">ETH3D</th>
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

### Comparison with Generalist Vision Models

We further compare SenseNova-Vision with recent generalist visual models that span multiple visual capabilities.

<table>
  <thead>
    <tr><th align="center" rowspan="3">Method</th><th align="center">Detection</th><th align="center">Sem. Seg.</th><th align="center">Ref. Seg.</th><th align="center">Depth</th></tr>
    <tr><th align="center">mAP</th><th align="center">mIoU</th><th align="center">cIoU</th><th align="center">δ1</th></tr>
    <tr><th align="center">COCO</th><th align="center">Cityscapes</th><th align="center">RefCOCO / + / g</th><th align="center">NYUv2</th></tr>
  </thead>
  <tbody>
    <tr><td>Youtu-VL</td><td>47.1</td><td>70.4</td><td>80.7 / <strong>76.2</strong> / 76.5</td><td>90.4</td></tr>
    <tr><td>SenseNova-Vision</td><td><strong>53.7</strong></td><td><strong>71.2</strong></td><td><strong>81.3</strong> / 76.0 / <strong>80.3</strong></td><td><strong>98.1</strong></td></tr>
  </tbody>
</table>

<table>
  <thead>
    <tr><th align="center" rowspan="3">Method</th><th align="center">Sem. Seg.</th><th align="center">Ref. Seg.</th><th align="center">Rea. Seg.</th><th align="center" colspan="4">Depth</th><th align="center" colspan="3">Normal</th></tr>
    <tr><th align="center">mIoU</th><th align="center">cIoU</th><th align="center">gIoU</th><th align="center" colspan="4">δ1</th><th align="center" colspan="3">Mean Error↓</th></tr>
    <tr><th align="center">Cityscapes</th><th align="center">RefCOCOg</th><th align="center">ReasonSeg</th><th align="center">KITTI</th><th align="center">NYUv2</th><th align="center">DIODE</th><th align="center">ETH3D</th><th align="center">NYUv2</th><th align="center">ScanNet</th><th align="center">DIODE</th></tr>
  </thead>
  <tbody>
    <tr><td>Vision Banana</td><td>69.9</td><td>73.8</td><td>79.3</td><td>91.5</td><td>94.8</td><td>91.7</td><td>93.5</td><td>17.8</td><td>15.1</td><td><strong>13.8</strong></td></tr>
    <tr><td>SenseNova-Vision</td><td><strong>71.2</strong></td><td><strong>80.3</strong></td><td>63.2</td><td>95.9</td><td>98.1</td><td>76.4</td><td>97.4</td><td><strong>14.4</strong></td><td><strong>12.8</strong></td><td>15.3</td></tr>
  </tbody>
</table>

### General Multimodal Capability

SenseNova-Vision largely maintains general multimodal capability while being adapted to visual perception tasks.

<table>
  <thead>
    <tr><th align="center" rowspan="2">Method</th><th align="center" colspan="3">Understanding</th><th align="center" colspan="2">Generation</th></tr>
    <tr><th align="center">MMMU</th><th align="center">MMVP</th><th align="center">MathVista</th><th align="center">GenEval</th><th align="center">WISE</th></tr>
  </thead>
  <tbody>
    <tr><td>Bagel</td><td>0.55</td><td>69.3</td><td>73.1</td><td>0.82</td><td>0.52</td></tr>
    <tr><td>SenseNova-Vision</td><td>0.42</td><td>79.0</td><td>67.7</td><td>0.85</td><td>0.45</td></tr>
  </tbody>
</table>


## 🖼️ Qualitative Examples

<p align="center">
  <img src="./assets/fig4_sensenova_vision_results.webp" alt="SenseNova-Vision qualitative results across vision tasks" width="900">
</p>

## 📚 Training Data

SenseNova-Vision is trained on the **SenseNova-Vision-Corpus-50M**, a large-scale computer-vision instruction-response corpus. The corpus converts heterogeneous annotations into a shared schema with visual inputs, natural-language instructions, and decodable targets represented as text, image, or mixed text-image responses.

The corpus covers four task families:

| Task family | Representative tasks | Target representation |
|---|---|---|
| Structured visual understanding | Detection, referring localization, pointing, keypoints, OCR, layout, GUI grounding | Text records with normalized coordinates and lightweight structure markers |
| Dense geometric prediction | Monocular depth estimation and surface-normal prediction | Deterministically encoded image targets |
| Segmentation | Referring, reasoning, interactive, generic, and grounded-conversation segmentation | Binary masks, color-coded masks, or mixed text-image responses |
| Multi-view visual geometry | Point-map reconstruction and camera-pose estimation | Image-like point maps and structured camera records |


## ⚠️ Limitations

- **Not a specialist model for every task:** Although SenseNova-Vision covers many tasks, task-specific models may still outperform it on certain specialized benchmarks.
- **Output parsing is task-dependent:** Textual outputs require task-specific parsers, and image outputs require decoding rules consistent with the training protocol.
- **Metric accuracy is not guaranteed:** Dense depth, normal, point-map, and camera-pose predictions should be validated carefully before downstream use.
- **Prompt sensitivity:** As an instruction-following model, performance can vary with prompt wording, output schema, and visual prompt style.
- **Dataset and benchmark bias:** Model behavior reflects the distribution and annotation conventions of the training corpus.

## 🛡️ Ethical Considerations

SenseNova-Vision may generate incorrect localization, segmentation, depth, normal, or camera predictions. Users should avoid deploying the model in safety-critical settings without independent verification. When used for datasets involving people, faces, documents, medical scenes, surveillance imagery, or private environments, users are responsible for complying with applicable privacy, consent, and data-governance requirements.

## ✒️ Citation

If you find SenseNova-Vision useful, please cite the technical report:

```bibtex
@misc{han2026visionunifiedmultimodalgeneration,
      title={Vision as Unified Multimodal Generation}, 
      author={Xiaoyang Han and Jianhua Li and Kewang Deng and Zukai Chen and Xuanke Shi and Sihan Wang and Boxuan Li and Linyan Wang and Siyi Xie and Xin You and Jinsheng Quan and Zhongang Cai and Haiwen Diao and Ziwei Liu and Lei Yang and Dahua Lin and Quan Wang},
      year={2026},
      eprint={2607.06560},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2607.06560}, 
}
```

## 📜 License
**Model weights**: CC BY-NC 4.0. The model weights are for non-commercial use only.

## 📮 Contact

For questions, issues, or collaboration requests, please use the official project repository or contact the authors through the release page once available.