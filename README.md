# FNA

The code of the ICLR 2020 paper [Fast Neural Network Adaptation via Parameter Remapping and Architecture Search](https://openreview.net/forum?id=rklTmyBKPH)

Deep neural networks achieve remarkable performance in many computer vision tasks. Most state-of-the-art (SOTA) semantic segmentation and object detection approaches reuse neural network architectures designed for image classification as the backbone, commonly pre-trained on ImageNet. However, performance gains can be achieved by designing network architectures specifically for detection and segmentation, as shown by recent neural architecture search (NAS) research for detection and segmentation. One major challenge though, is that ImageNet pre-training of the search space representation (a.k.a. super network) or the searched networks incurs huge computational cost. 

In this paper, we propose a Fast Neural Network Adaptation (FNA) method, which can adapt both the architecture and parameters of a seed network (e.g. a high performing manually designed backbone) to become a network with different depth, width, or kernels via a Parameter Remapping technique, making it possible to utilize NAS for detection/segmentation tasks a lot more efficiently.

![framework](./imgs/framework.png)

## Results

In our experiments, we conduct FNA on MobileNetV2 to obtain new networks for both segmentation and detection that clearly out-perform existing networks designed both manually and by NAS. The total computation cost of FNA is significantly less than SOTA segmentation/detection NAS approaches: 1737x less than DPC, 6.8x less than Auto-DeepLab and 7.4x less than DetNAS.

![seg_results](./imgs/seg_results.png)
![seg_cost](./imgs/seg_cost.png)
![det_results](./imgs/det_results.png)
![det_cost](./imgs/det_cost.png)

## FNA on Object Detection

### Requirements

* python 3.7
* pytorch 1.1 
* mmdet 0.6.0 (53c647e)
* mmcv 0.2.10

### Parameter Adaptation

1. Adapt the parameters of the target architecture on COCO. The adaptation is performed on 8 TITAN-XP GPUs.

    ```bash
    cd fna_det
    sh scripts/param_adapt.sh
    ```

    The seed network MobileNetV2 is trained on ImageNet using the code of [DenseNAS](https://github.com/JaminFong/DenseNAS). We provide the pre-trained weights and `net_config` of the seed network in [MobileNetV2](https://drive.google.com/open?id=1XW0NxkLckKQ68s6V7nf7vF4qe1WsL3GE). The code of MobileNetV2 model is constructed in `models/derived_imagenet_net.py`.

2. We provide the adapted parameters and `net_config` in checkpoint [RetinaNet](https://drive.google.com/open?id=1BatmfFQ6ArcYN3l8OD9epl3asRhjx1yx). You can evaluate the checkpoint with the following script.
`sh scripts/test.sh`

## FNA on Semantic Segmentation

### Requirements

* python 3.7
* pytorch 1.1

### Evaluate

1. The adapted parameters and `net_config` are available in [DeepLabV3](https://drive.google.com/open?id=1dNK5QEn-Mhcx20fctz7Zo9yZzDzCvyBt).

2. Put the adapted parameters `epoch-last.pth` into `fna_seg/model/deeplab/cityscapes.deeplabv3`.

3. You can evaluate the checkpoint with the following script. 

    ```bash
    cd fna_seg/model/deeplab/cityscapes.deeplabv3
    sh eval.sh.
    ```

## Citation

```bash
@inproceedings{
    fang2020fast,
    title={Fast Neural Network Adaptation via Parameter Remapping and Architecture Search},
    author={Jiemin Fang* and Yuzhu Sun* and Kangjian Peng* and Qian Zhang and Yuan Li and Wenyu Liu and Xinggang Wang},
    booktitle={International Conference on Learning Representations},
    year={2020},
}
```

## Acknowledgement

The code of FNA on is based on 

* [mmdetection](https://github.com/open-mmlab/mmdetection/tree/v0.6.0)
* [TorchSeg](https://github.com/ycszen/TorchSeg)

Thanks for the contribution of the above repositories.