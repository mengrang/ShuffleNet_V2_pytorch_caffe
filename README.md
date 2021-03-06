ShuffleNet_V2_pytorch_caffe
=======================================
ShuffleNet-V2 for both PyTorch and Caffe.

This project supports both Pytorch and Caffe.
Multiple kinds of model width are supported.
Model with should be 0.25, 0.5, 1.0, 1.5 or 2.0, other model width are not tested.

Usage
=======================================

PyTorch
---------------------------------------
Just use shufflenet_v2.py as following.
```
import shufflenet_v2
num_classes = 1000
model_width = 0.5 
shufflenet_v2.Network(num_classes, model_width)
```

Caffe
---------------------------------------
Prototxt files can be generated by shufflenet_v2.py
```
python shufflenet_v2.py --save_caffe net --num_classes 1000 --model_width 1.0
```

Converting Model from PyTorch to Caffe
---------------------------------------
```
python shufflenet_v2.py --load_pytorch net.pth --save_caffe net --num_classes 1000 --model_width 1.0
```

Trained Models for PyTorch and Caffe
---------------------------------------
### shufflenet_v2_x0.25, Top-1 Acc = 46.04%
### shufflenet_v2_x0.5, Top-1 Acc = 57.51%
Models can be downloaded from https://github.com/miaow1988/ShuffleNet_V2_pytorch_caffe/releases

## Training the model
1. All ImageNet images are resized by a short edge size of 256 (bicubic interpolation by PIL). And then each of them are pickled by Python and stored in a LMDB dataset.
2. Training is done by PyTorch 0.4.0,
3. data augmentation: 224x224 random crop and random horizontal flip. No image mean extraction is used here, which can be done automatically by data/bn layers in the network.
4. A SGD with nesterov momentum (0.9) is used as optimizer, batch size is 512. Models are trained 300000 iterations, while the learning rate decayed linearly from 0.25 to 0. (The batch size used in paper is 1024, and the initial learning rate is 0.5 instead 0.25. Although, Because I don't have enough time for a time consuming training.)

## Something you might have noticed
1. Models are trained by PyTorch and converted to Caffe. Thus, you should use scale parameter in Caffe's data layer to make sure all input images are rescaled from [0, 255] to [0, 1]. 
2. The RGB~BGR problem is not crucial here. So you may ignore the difference if you use these models as pretrained models.

## Others
I barely achieved results of different kinds of sophisticated ImageNet models reported in papers. 
So if you got a better accuracy then mine, please teach me.
