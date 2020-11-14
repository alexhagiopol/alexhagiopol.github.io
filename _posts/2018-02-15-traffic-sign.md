---
title: 'Traffic Sign Classification Using CNNs'
date: 2018-02-15
permalink: /posts/2018/02/traffic-sign/
tags:
  - LeNet5
  - dropout
  - regularization
---

### Description:

I used TensorFlow to implement a CNN architecture created using concepts from classic papers in deep learning including [Gradient-Based Learning Applied to Document Recognition](http://yann.lecun.com/exdb/publis/pdf/lecun-01a.pdf) by LeCun et al, [Traffic Sign Recognition with Multi Scale Convolutional Networks](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6033589&casa_token=Nc46-nvO870AAAAA:HFhpeWnjMIfH75FUtxY2AI6BPmU3Xu_wvbXNSLvLwNGUi-JiwTNrTxwXOm4skPIn7yYyZ9HVlgMf&tag=1) by Sermanet and LeCun, and [Dropout: A Simple Way to Prevent Neural Networks from Overfitting](https://www.jmlr.org/papers/volume15/srivastava14a/srivastava14a.pdf) by Srivastava et al. The classifier's performance is tested using the German Traffic Sign Dataset which contains over 51000 image patches from automobile dashcams, each annotated with one of 43 traffic sign classes. This implementation achieves 99.1% validation accuracy and 97.2% test accuracy on this dataset. These results are encouraging given that human performance on this dataset is 98.8% (Sermanet & LeCun, 2011). However, overfitting the GTSRB dataset remains a challenge when attempting to generalize to any image of German traffic signs captured by any camera. Future work includes further research into generalization including further preprocessing to make reliable inferences on images from any input source. The implementation and technical report are [on GitHub](https://github.com/alexhagiopol/multiscale-CNN-classifier). 

### Teaser Image:

![](/content/traffic_sign_classifier.png)
