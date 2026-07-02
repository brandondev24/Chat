---
license: cc-by-nc-4.0
language:
- cn
tags:
- multimodal
- vision-language
- unified-multimodal-generation
- computer-vision
- object-detection
- segmentation
- depth-estimation
- surface-normal-estimation
- 3d-reconstruction
- camera-pose-estimation
- ocr
- gui-grounding
pipeline_tag: image-to-text
inference: false
---


# SenseNova-Vision: Rethinking Computer Vision as Unified Multimodal Generation

<p align="center">
  <strong>English</strong> | <a href="https://huggingface.co/sensenova/SenseNova-Vision-7B-MoT/blob/main/README_CN.md">简体中文</a>
</p>

<p align="center">
  <a href="https://github.com/OpenSenseNova/SenseNova-Vision"><img alt="GitHub Stars" src="https://img.shields.io/github/stars/OpenSenseNova/SenseNova-Vision?style=social"></a>
  <a href="https://arxiv.org/abs/2511.18333"><img alt="arXiv" src="https://img.shields.io/badge/arXiv-2511.18333-b31b1b.svg"></a>
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

## Overview
SenseNova-Vision is a unified multimodal model for computer vision. It reformulates heterogeneous visual perception tasks as **text generation**, **image generation**, or **mixed text-image generation**, instead of relying on task-specific heads, decoders, or loss functions for each individual task. The model supports structured visual understanding, dense geometric prediction, segmentation, and multi-view visual geometry within a shared instruction-following interface.


## Model Description
SenseNova-Vision rethinks computer vision as **unified multimodal generation**. Traditional computer-vision systems usually attach task-specific prediction heads for detection, segmentation, depth, surface normal, or 3D geometry. SenseNova-Vision instead expresses these heterogeneous tasks through the native input-output spaces of a unified multimodal model.

Natural-language instructions and optional visual prompts specify the target task, regions, views, output schema, and decoding convention. The model then generates different target formats depending on the task:

| Target type | Representative tasks | Output form |
|---|---|---|
| Structured text | Detection, referring localization, OCR, GUI grounding, keypoints, camera parameters | Text records with normalized coordinates or structured fields |
| Dense image | Depth, surface normal, point maps, binary masks, color-coded masks | Image-like target maps |
| Mixed text-image | Multi-instance segmentation, grounded conversation segmentation, compositional perception | Text labels plus generated masks or visual maps |

This formulation allows a single model to cover structured visual understanding, dense geometric prediction, segmentation, and multi-view visual geometry while keeping outputs decodable for standard benchmarks.

## Key Features

- **Unified vision-task formulation:** Heterogeneous computer-vision tasks are cast into the native text, image, and mixed generation spaces of a unified multimodal model.
- **No task-specific heads:** The model does not rely on separate detection, segmentation, depth, normal, or geometry heads.
- **Decodable outputs:** Generated text and images can be converted back into benchmark-compatible boxes, points, OCR strings, masks, depth maps, normal maps, point maps, and camera records.
- **Broad task coverage:** The same model handles structured visual understanding, segmentation, dense geometry, and multi-view visual geometry.
- **Instruction-defined task variants:** Natural-language instructions enable flexible task definitions beyond fixed benchmark schemas.


## How to Use

The model requires the SenseNova-Vision inference code and task-specific post-processing scripts. The examples below assume that you have installed the project code and downloaded this model repository from Hugging Face.

### Download the Model

```python
from huggingface_hub import snapshot_download

model_path = snapshot_download("sensenovaSenseNova-Vision-7B-MoT")
print(model_path)
```

## Benchmark Results

SenseNova-Vision is evaluated across structured visual understanding, dense geometric prediction, segmentation, multi-view visual geometry, and general multimodal capability. All tasks are formulated with natural-language instructions.

### Structured Visual Understanding

| Task | Benchmark | Output | SenseNova-Vision |
|---|---:|---|---:|
| Object detection | COCO-Com. | bbox mAP | **56.6** |
| Referring localization | HR / RefCOCOg V / T | bbox | **80.2 / 79.6 / 80.5** |
| Long-tail detection | LVIS | bbox mAP | **54.8** |
| Dense detection | Dense200 | bbox mAP | **66.8** |
| Tiny detection | VisDrone | bbox / point | **43.3 / 62.9** |
| OCR localization | HierText | bbox | **31.2** |
| OCR localization | ICDAR15 | bbox | **49.5** |
| GUI grounding | ScreenSpot-V2 | bbox | 85.9 |
| Keypoint localization | COCO-Keypoint | point | **34.6** |

### Dense Geometric Prediction

| Task | Benchmark | Metric | SenseNova-Vision |
|---|---:|---|---:|
| Depth | NYUv2 | AbsRel↓ / δ1↑ | **4.0 / 98.1** |
| Depth | KITTI | AbsRel↓ / δ1↑ | **5.9 / 95.9** |
| Depth | ETH3D | AbsRel↓ / δ1↑ | 4.3 / 97.4 |
| Depth | ScanNet | AbsRel↓ / δ1↑ | **3.9 / 98.0** |
| Depth | DIODE | AbsRel↓ / δ1↑ | **20.6 / 76.4** |
| Surface normal | ScanNet | Mean↓ / 11.25°↑ | **12.8 / 68.9** |
| Surface normal | iBims-1 | Mean↓ / 11.25°↑ | 15.4 / 69.1 |
| Surface normal | NYUv2 | Mean↓ / 11.25°↑ | **14.4 / 62.7** |

### Segmentation

| Task | Benchmark | Metric | SenseNova-Vision |
|---|---:|---|---:|
| Generic segmentation | COCO Panoptic / Semantic | Pan. / Sem. | 48.8 / 64.0 |
| Referring segmentation | RefCOCO / RefCOCO+ / RefCOCOg | cIoU | 81.3 / 76.0 / 80.3 |
| Reasoning segmentation | ReasonSeg Val / Test | gIoU | **63.2 / 60.7** |
| GCG segmentation | Val / Test | gIoU | 65.7 / 66.2 |
| Interactive segmentation | Point / Box | mIoU | 60.9 / **73.9** |

### Multi-View Visual Geometry

| Task | Benchmark | Metric | SenseNova-Vision |
|---|---:|---|---:|
| Multi-view reconstruction | 7Scenes | Acc.↓ / Comp.↓ / F1↑ | 0.028 / **0.026** / **87.9** |
| Multi-view reconstruction | ETH3D | Acc.↓ / Comp.↓ / F1↑ | **0.301 / 0.175 / 72.2** |
| Camera pose | Re10K | RRA@30↑ / RTA@30↑ / AUC@30↑ | 99.8 / **94.2** / 77.3 |
| Camera pose | CO3Dv2 | RRA@30↑ / RTA@30↑ / AUC@30↑ | **97.4 / 95.4 / 80.1** |

### General Multimodal Capability

| Model | MMMU | MMVP | MathVista | GenEval | WISE |
|---|---:|---:|---:|---:|---:|
| Bagel | 0.55 | 69.3 | 73.1 | 0.82 | 0.52 |
| SenseNova-Vision | 0.42 | 79.0 | 67.7 | 0.85 | 0.45 |

## Qualitative Examples

<p align="center">
  <img src="./assets/fig4_sensenova_vision_results.webp" alt="SenseNova-Vision qualitative results across vision tasks" width="900">
</p>

## Training Data

SenseNova-Vision is trained on the **SenseNova-Vision-Corpus-50M**, a large-scale computer-vision instruction-response corpus. The corpus converts heterogeneous annotations into a shared schema with visual inputs, natural-language instructions, and decodable targets represented as text, image, or mixed text-image responses.

The corpus covers four task families:

| Task family | Representative tasks | Target representation |
|---|---|---|
| Structured visual understanding | Detection, referring localization, pointing, keypoints, OCR, layout, GUI grounding | Text records with normalized coordinates and lightweight structure markers |
| Dense geometric prediction | Monocular depth estimation and surface-normal prediction | Deterministically encoded image targets |
| Segmentation | Referring, reasoning, interactive, generic, and grounded-conversation segmentation | Binary masks, color-coded masks, or mixed text-image responses |
| Multi-view visual geometry | Point-map reconstruction and camera-pose estimation | Image-like point maps and structured camera records |

Public annotations are converted directly when possible. Generated or curated targets are used to supplement incomplete supervision and improve diversity. A dedicated data document should provide the source lists, prompt templates, conversion rules, and corpus construction details.

## Limitations

- **Not a specialist model for every task:** Although SenseNova-Vision covers many tasks, task-specific models may still outperform it on certain specialized benchmarks.
- **Output parsing is task-dependent:** Textual outputs require task-specific parsers, and image outputs require decoding rules consistent with the training protocol.
- **Metric accuracy is not guaranteed:** Dense depth, normal, point-map, and camera-pose predictions should be validated carefully before downstream use.
- **Prompt sensitivity:** As an instruction-following model, performance can vary with prompt wording, output schema, and visual prompt style.
- **Dataset and benchmark bias:** Model behavior reflects the distribution and annotation conventions of the training corpus.

## Ethical Considerations

SenseNova-Vision may generate incorrect localization, segmentation, depth, normal, or camera predictions. Users should avoid deploying the model in safety-critical settings without independent verification. When used for datasets involving people, faces, documents, medical scenes, surveillance imagery, or private environments, users are responsible for complying with applicable privacy, consent, and data-governance requirements.

## Citation

If you find SenseNova-Vision useful, please cite the technical report:

```bibtex
@misc{sensenova_vision_2026,
  title  = {SenseNova-Vision: Rethinking Computer Vision as Unified Multimodal Generation},
  author = {{SenseTime Research Team}},
  year   = {2026},
  note   = {Technical report}
}
```

## License

License information for the SenseNova-Vision pre-release will be updated before the public release. Please also follow the licenses of the underlying models, datasets, and third-party evaluation tools used by this model.

## Contact

For questions, issues, or collaboration requests, please use the official project repository or contact the authors through the release page once available.
