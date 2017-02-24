# Hard-Aware-Deeply-Cascaded-Embedding

This project is forked from[HDC](https://github.com/PkuRainBow/Hard-Aware-Deeply-Cascaded-Embedding_release)

I'm tring to change it to work in Ubuntu with Gtx1080

Pray for me.

**Information** :  **The weight_decay for CARS196 should be 0.0002, the original version is typed to be 0.0005 !**

This repository has the source code for the paper "Hard-Aware-Deeply-Cascaded-Embedding_release"(submit to CVPR-2017). This paper is available
 on [arXiv](https://arxiv.org/abs/1611.05720). For the loss layer implementation, look at the folder caffe_layers.
 

## Citing this work
If you find this work useful in your research, please consider citing :

    @article{yuan2016HDC,
      title={Hard-Aware Deeply Cascaded Embedding},
      author={Yuan, Yuhui and Yang, Kuiyuan and Zhang, Chao},
      journal={arXiv preprint arXiv:1611.05720},
      year={2016}
    }
    
## Installation
1. Install [Caffe](https://github.com/BVLC/caffe) (including the python interface if you want to use the test code) 
2. Add the "NormalizationLayer" and "PairFastLossLayer" to the caffe.
please add the following lines the **caffe.proto** :
```
 optional PairFastLossParameter pair_fast_loss_param = 154; //change it to a number according to your version
 message PairFastLossParameter {
   //margin for dissimilar pair
   optional float margin = 1 [default = 1.0];
   optional float hard_ratio = 2 [default = 1.0];
   optional float only_pos = 3 [default = 0];
 }
```      
## Prerequisites
1. caffe (python interface)
2. matplotlib,cPickle,numpy,lmdb
3. We assumed that you are using Windows (you can rewrite the *.bat to *.sh if you choose Linux or MacOs)

## Datasets & Models 
1.  Download pretrained GoogLeNet model from [here](https://github.com/BVLC/caffe/tree/master/models/bvlc_googlenet)
2.  Download the datasets needed (you could download them from the official sets): [StanfordOnlineProducts](ftp://cs.stanford.edu/cs/cvgl/Stanford_Online_Products.zip)  [CARS196](http://ai.stanford.edu/~jkrause/cars/car_dataset.html) [CUB200](http://www.vision.caltech.edu/visipedia/CUB-200-2011.html) [DeepFashion](http://mmlab.ie.cuhk.edu.hk/projects/DeepFashion.html) (we will release our results on both both in-shop and consumer2shop.)
3.  You should change the path of images in the Training Prototxt.

## Usage
**Process Data**: (you should in the folder /code/):
```
   python hdc_process.py -d stanford_products
   python hdc_process.py -d cub200
   python hdc_process.py -d cars196
   python hdc_process.py -d deepfashion
```
   
**Training Models**: (currently we only support HDC for your convenience)
```
   python hdc_train.py -d stanford_products -c HDC
   python hdc_train.py -d cub200 -c HDC
   python hdc_train.py -d cars196 -c HDC
```
   **You could change the image path in the training prototxt to train the models with bounding boxes or not**
   
**Extract Features**:
 ```
   python hdc_feature.py -d stanford_products -c HDC
   python hdc_feature.py -d cub200 -c HDC
   python hdc_feature.py -d cars196 -c HDC
```
**Test Models**:
```
   python hdc_test.py -d stanford_products -c HDC
   python hdc_test.py -d cub200 -c HDC
   python hdc_test.py -d cars196 -c HDC
```

**Improve Space**

To get better results than the paper, you could simply by sampling more batches. For convenience, we all sample for 5000 times. In our experiments, by sampling 10,000 times could further improve the performance.
