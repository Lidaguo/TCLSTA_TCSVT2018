## Introduction
This is the source code of our TCSVT 2018 paper "[Two-stream Collaborative Learning with Spatial-Temporal Attention for Video Classification](http://59.108.48.34/tiki/download_paper.php?fileId=20187)", please cite the following paper if you use our code.

    Yuxin Peng, Yunzhen Zhao, and Junchao Zhang, "Two-stream Collaborative Learning with Spatial-Temporal Attention for Video Classification", IEEE Transactions on Circuits and Systems for Video Technology (TCSVT), DOI: 10.1109/TCSVT.2018.2808685, 2018.

## Dependency
* The code for spatial-temporal attention model is based on [Caffe](https://github.com/BVLC/caffe), all the dependencies are the same as Caffe. The provided caffe code `caffe-rc3-lstm/` is modified on the [rc3](https://github.com/BVLC/caffe/tree/rc3) version.

* For the implementation of the LSTM layer in `caffe-rc3-lstm/`, please refer to [Junhyuk Oh's implementation](https://github.com/junhyukoh/caffe-lstm).

* The code for static-motion collaborative model is based on [Torch](http://torch.ch/). The provided code `CollaborativeLearning/` is modified on [Jiasen Lu's implementation](https://github.com/jiasenlu/HieCoAttenVQA), all the dependencies can be seen in [Requirements](https://github.com/jiasenlu/HieCoAttenVQA#requirements).

* The proposed TCLSTA also uses the Pre-trained ResNet-50 model with batch normalization, which can be downloaded at [Caffe model zoo](https://github.com/BVLC/caffe/wiki/Model-Zoo#imagenet-pre-trained-models-with-batch-normalization), download this model and put it in `PretrainedModel/` folder.

## Data Preparation
Here we use [UCF101](http://crcv.ucf.edu/data/UCF101.php) dataset for an example, download the UCF101 dataset, and put the extracted frames and optical flow images in `dataset/UCF101/UCF101_jpegs_256/` and `dataset/UCF101/UCF101_tvl1_flow/` folders, respectively.

It's recommended to use the [Christoph Feichtenhofer's toolkit](https://github.com/feichtenhofer/gpu_flow) to compute optical flow.

## Usage

We show the training steps on UCF101 split01 with single GPU for example.

1. The training of spatial-temporal attention model.<br/>
For the stable convergence of spatial-temporal attention model, we take the following training steps:
* Train ```Connection network``` and ```Spatial-level attention network``` jointly and get the spatial attention model.<br/>

        sh train_resnet50_sp01_spatial.sh
* Train ```Temporal-level attention network``` based on the obtained spatial attention model, with freezing the weights of ```Connection network``` and ```Spatial-level attention network```.

        Sample 10 frames for each video
        sh train_resnet50_sp01_spatial_temporal_frozen.sh
* Train the spatial-temporal attention model jointly based on the obtained model by the last step.

        sh train_resnet50_sp01_spatial_temporal.sh

  Note that spatial-temporal attention model on optical flow can be obtained by similar training steps.  

2. The training of static-motion collaborative model.<br/>
First sample 25 frames and opical flow images for each video, then extract frame and optical flow features using trained spatial-temporal attention models. Then excute the following command to train the static-motion collaborative model.

        cd CollaborativeLearning/
        th trainTest.lua

## Related Link
Welcome to our [Laboratory Homepage](http://www.icst.pku.edu.cn/mipl) for more information about our papers, source codes, and datasets.

