# Self-Supervised Learning with Vision Transformers

By [Zhenda Xie](https://github.com/zdaxie/)\*, [Yutong Lin](https://github.com/impiga)\*, [Zhuliang Yao](https://github.com/Howal), [Zheng Zhang](https://stupidzz.github.io/), [Qi Dai](https://www.microsoft.com/en-us/research/people/qid/), [Yue Cao](http://yue-cao.me) and [Han Hu](https://ancientmooner.github.io/)

This repo is the official implementation of ["Self-Supervised Learning with Swin Transformers"](https://arxiv.org/abs/2105.04553). It currently includes code and models for the following tasks:

> **Self-Supervised Learning and Linear Evaluation**: Included in this repo. See [get_started.md](get_started.md) for a quick start.

> **Object Detection and Instance Segmentation**: See [Swin Transformer for Object Detection](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection).

> **Semantic Segmentation**: See [Swin Transformer for Semantic Segmentation](https://github.com/SwinTransformer/Swin-Transformer-Semantic-Segmentation).


## Updates

*** 05/11/2021 ***

Initial Commits:
1. Self-Supervised Pre-training models on ImageNet-1K ([MoBY-Swin-T-300Ep](https://drive.google.com/file/d/1PS1Q0tAnUfBWLRPxh9iUrinAxeq7Y--u/view?usp=sharing), [MoBY-Swin-T-300Ep-Linear](https://drive.google.com/file/d/1gbQynZy07uXPO-c0tOLeyG1pQzlnVHx9/view?usp=sharing)) are provided.
2. The supported code and models for self-supervised pre-training and ImageNet-1K linear evaluation, COCO object detection and ADE20K semantic segmentation are provided.

## Introduction

### MoBY

**MoBY** (the name `MoBY` stands for **Mo**Co v2 with **BY**OL) is initially described in [arxiv](https://arxiv.org/abs/2105.04553), which is a combination of two popular self-supervised learning approaches: MoCo v2 and BYOL. It inherits the momentum design, the key queue, and the contrastive loss used in MoCo v2, and inherits the asymmetric encoders, asymmetric data augmentations and the momentum scheduler in BYOL.

**MoBY** achieves reasonably high accuracy on ImageNet-1K linear evaluation: 72.5\% and 75.0\% top-1 accuracy using DeiT and Swin-T, respectively, by 300-epoch training. The performance is on par with recent works of MoCo v3 and DINO which adopt DeiT as the backbone, but with much lighter tricks. 

![teaser_moby](figures/teaser_moby.png)
### Swin Transformer

**Swin Transformer** (the name `Swin` stands for **S**hifted **win**dow) is initially described in [arxiv](https://arxiv.org/abs/2103.14030), which capably serves as a general-purpose backbone for computer vision. It is basically a hierarchical Transformer whose representation is computed with shifted windows. The shifted windowing scheme brings greater efficiency by limiting self-attention computation to non-overlapping local windows while also allowing for cross-window connection.

**Swin Transformer** achieves strong performance on COCO object detection (`58.7 box AP` and `51.1 mask AP` on test-dev) and ADE20K semantic segmentation (`53.5 mIoU` on val), surpassing previous models by a large margin.

![teaser_swin](figures/teaser_swin.png)

## Main Results on ImageNet with Pretrained Models

**ImageNet-1K and ImageNet-22K Pretrained Models**

|      Method      | Architecture | Epochs | Params | FLOPs | img/s | Top-1 Accuracy |                                                                                            Checkpoint                                                                                            |
| :--------------: | :----------: | :----: | :----: | :---: | :---: | :------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|    Supervised    |    Swin-T    |  300   |  28M   | 4.5G  | 755.2 |      81.2      |                                              [Here](https://github.com/microsoft/Swin-Transformer#main-results-on-imagenet-with-pretrained-models)                                               |
|       MoBY       |    Swin-T    |  100   |  28M   | 4.5G  | 755.2 |      70.9      |                                                                                             [TBA]()                                                                                              |
| MoBY<sup>1</sup> |    Swin-T    |  100   |  28M   | 4.5G  | 755.2 |      72.0      |                                                                                             [TBA]()                                                                                              |
|       MoBY       |    Swin-T    |  300   |  28M   | 4.5G  | 755.2 |      75.0      | [Pre-trained](https://drive.google.com/file/d/1PS1Q0tAnUfBWLRPxh9iUrinAxeq7Y--u/view?usp=sharing) / [Linear](https://drive.google.com/file/d/1gbQynZy07uXPO-c0tOLeyG1pQzlnVHx9/view?usp=sharing) |

- <sup>1</sup> denotes the result of MoBY which has adopted a trick from MoCo v3 that replace theLayerNorm layers before the MLP blocks by BatchNorm.


## Main Results on Downstream Tasks

**COCO Object Detection (2017 val)**

| Backbone |       Method       | Model | Schd. | box mAP | mask mAP | Params | FLOPs |
| :------: | :----------------: | :---: | :---: | :-----: | :------: | :----: | :---: |
|  Swin-T  |     Mask R-CNN     | Sup.  |  1x   |  43.7   |   39.8   |  48M   | 267G  |
|  Swin-T  |     Mask R-CNN     | MoBY  |  1x   |  43.6   |   39.6   |  48M   | 267G  |
|  Swin-T  |     Mask R-CNN     | Sup.  |  3x   |  46.0   |   41.6   |  48M   | 267G  |
|  Swin-T  |     Mask R-CNN     | MoBY  |  3x   |  46.0   |   41.7   |  48M   | 267G  |
|  Swin-T  | Cascade Mask R-CNN | Sup.  |  1x   |  48.1   |   41.7   |  86M   | 745G  |
|  Swin-T  | Cascade Mask R-CNN | MoBY  |  1x   |  48.1   |   41.5   |  86M   | 745G  |
|  Swin-T  | Cascade Mask R-CNN | Sup.  |  3x   |  50.4   |   43.7   |  86M   | 745G  |
|  Swin-T  | Cascade Mask R-CNN | MoBY  |  3x   |  50.2   |   43.5   |  86M   | 745G  |

**ADE20K Semantic Segmentation (val)**

| Backbone | Method  | Model | Crop Size | Schd. | mIoU  | mIoU (ms+flip) | Params | FLOPs |
| :------: | :-----: | :---: | :-------: | :---: | :---: | :------------: | :----: | :---: |
|  Swin-T  | UPerNet | Sup.  |  512x512  | 160K  | 44.51 |     45.81      |  60M   | 945G  |
|  Swin-T  | UPerNet | MoBY  |  512x512  | 160K  | 44.06 |     45.58      |  60M   | 945G  |


## Citing MoBY and Swin

### MoBY

```
@article{xie2021selfsupervised,
  title={Self-Supervised Learning with Swin Transformers}, 
  author={Zhenda Xie and Yutong Lin and Zhuliang Yao and Zheng Zhang and Qi Dai and Yue Cao and Han Hu},
  journal={arXiv preprint arXiv:2105.04553},
  year={2021}
}
```

### Swin Transformer

```
@article{liu2021Swin,
  title={Swin Transformer: Hierarchical Vision Transformer using Shifted Windows},
  author={Liu, Ze and Lin, Yutong and Cao, Yue and Hu, Han and Wei, Yixuan and Zhang, Zheng and Lin, Stephen and Guo, Baining},
  journal={arXiv preprint arXiv:2103.14030},
  year={2021}
}
```

## Getting Started

- For **Self-Supervised Pre-training and Linear Evaluation with MoBY and Swin Transformer**, please see [get_started.md](get_started.md) for detailed instructions.
- For **Object Detection and Instance Segmentation**, please see [Swin Transformer for Object Detection](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection).
- For **Semantic Segmentation**, please see [Swin Transformer for Semantic Segmentation](https://github.com/SwinTransformer/Swin-Transformer-Semantic-Segmentation).
