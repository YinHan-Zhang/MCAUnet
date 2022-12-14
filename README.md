# MCAUNet 


This repo is the official implementation of
["Survival analysis of patients with liver cirrhosis 
based on deep learning to quantify body composition"](https://arxiv.org/abs/2109.04335)

We propose a Channel Transformer module (CTrans) and use it to 
replace the skip connections in original U-Net, thus we name it "MCAU-Net".
## Requirements


Install from the ```requirements.txt``` using:
```angular2html
pip install -r requirements.txt
```

## Usage



### 1. Data Preparation


Prepare the datasets in the following format for easy use of the code:
```angular2html
├── datasets
    |
    │── Test_Folder
    │   ├── img
    │   └──labelcol
    │── Train_Folder
    │   ├── img
    │   └── labelcol
    │── Val_Folder
    │   ├── img
    │   └── labelcol
    └
```


### 2. Training
As mentioned in the paper, we introduce two strategies 
to optimize MCAUNet.

The first step is to change the settings in ```Config.py```,
all the configurations including learning rate, batch size and etc. are 
in it.

#### 2.1 Jointly Training
We optimize the convolution parameters 
in U-Net and the CTrans parameters together with a single loss.
Run:
```angular2html
python train_model.py
```

#### 2.2 Pre-training

Our method just replaces the skip connections in U-Net, 
so the parameters in U-Net can be used as part of pretrained weights.

By first training a classical U-Net using ```/nets/UNet.py``` 
then using the pretrained weights to train the UCTransNet, 
CTrans module can get better initial features.

This strategy can improve the convergence speed and may 
improve the final segmentation performance in some cases.


### 3. Testing
#### 3.1. Get Pre-trained Models
Here, we provide pre-trained weights on GlaS and MoNuSeg, if you do not want to train the models by yourself, you can download them in the following links:
* GlaS：https://drive.google.com/file/d/1ciAwb2-0G1pZrt_lgSwd-7vH1STmxdYe/view?usp=sharing
* MoNuSeg: https://drive.google.com/file/d/1CJvHoh3VrPsBn_njZDo6SvJF_yAVe5MK/view?usp=sharing
#### 3.2. Test the Model and Visualize the Segmentation Results
First, change the session name in ```Config.py``` as the training phase.
Then run:
```angular2html
python test_model.py
```
You can get the Dice and IoU scores and the visualization results. 


### 4. Reproducibility
In our code, we carefully set the random seed and set cudnn as 'deterministic' mode to eliminate the randomness. 
However, there still exsist some factors which may cause different training results, e.g., the cuda version, GPU types, the number of GPUs and etc. The GPU used in our experiments is NVIDIA A40 (48G) and the cuda version is 11.2.

Especially for multi-GPU cases, the upsampling operation has big problems with randomness.
See https://pytorch.org/docs/stable/notes/randomness.html for more details.

When training, we suggest to train the model twice to verify wheather the randomness is eliminated. Because we use the early stopping strategy, the final performance may change significantly due to the randomness. 

## Reference


* [TransUNet](https://github.com/Beckschen/TransUNet) 
* [MedT](https://github.com/jeya-maria-jose/Medical-Transformer)




## Contact 
YinHan Zhang ([1513032551@qq.com](1513032551@qq.com))
