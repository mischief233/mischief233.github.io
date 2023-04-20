# Pytorch基础

## 一、transforms的使用方法

因为得到的图片数据集很少，在训练网络的术后准确度很低。但是又很难找到其他数据集。所以在训练网络的时候，我们很关注对图像的预处理操作，并使用了数据增强的方法。

- **1 裁剪-Crop**

中心裁剪：transforms.CenterCrop

随机裁剪：transforms.RandomCrop

随机长宽比裁剪：transforms.RandomResizedCrop

上下左右中心裁剪：transforms.FiveCrop

上下左右中心裁剪后翻转，transforms.TenCrop

- **2 翻转和旋转——Flip and Rotation**

依概率p水平翻转：transforms.RandomHorizontalFlip(p=0.5)

依概率p垂直翻转：transforms.RandomVerticalFlip(p=0.5)

随机旋转：transforms.RandomRotation

- **3 图像变换**

resize：transforms.Resize
标准化：transforms.Normalize
转为tensor，并归一化至[0-1]：transforms.ToTensor
填充：transforms.Pad
修改亮度、对比度和饱和度：transforms.ColorJitter
转灰度图：transforms.Grayscale
线性变换：transforms.LinearTransformation()
仿射变换：transforms.RandomAffine
依概率p转为灰度图：transforms.RandomGrayscale
将数据转换为PILImage：transforms.ToPILImage
transforms.Lambda：Apply a user-defined lambda as a transform.

- **4 对transforms操作，使数据增强更灵活**

transforms.RandomChoice(transforms)， 从给定的一系列transforms中选一个进行操作

transforms.RandomApply(transforms, p=0.5)，给一个transform加上概率，依概率进行操作

transforms.RandomOrder，将transforms中的操作随机打乱

- **5 对transforms操作，使数据增强更灵活**

将它们编写为可调用的类，而不是简单的函数，这样就不必每次调用转换时都传递其参数。 为此，我们只需要实现**call**方法和（如果需要传递参数）**init**方法即可。 然后我们可以使用这样的转换:

- tx = Transform(params)

- transformed_sample = tx(sample)

  ```
  #将n_arrays转换为pytorch的tensor 
  class ToTensor(object):
    """
    Numpy的image: H*W*channels 
    Tensor的image: channels*H*W
    """
    def __call__(self,sample):
      image,key_pts = sample['image'],sample['keypoints']
      
      # 给灰度图增加channel纬度
      if len(image.shape) == 2:
        image = image.reshape(image.shape[0],image.shape[1],1)
      
      image = image.transpose((2,0,1)) # c h w 
  
      return {'image': torch.from_numpy(image),
              'keypoints':torch.from_numpy(key_pts)}
  ```




## 二、 nn.Module

torch.nn的核心数据结构是Module，它是一个抽象的概念，既可以表示神经网络中的某个 层，也可以表示一个包含很多层的神经网络。最常见的做法就是继承nn.Module，编写自己的网 络。



要实现一个自定义层大致分以下几个主要的步骤：

 1）自定义一个类，继承自Module类，并且一定要实现两个基本的函数 

✓ 构造函数__init__ 

✓ 前向计算函数forward函数

 2）在构造函数_init__中实现层的参数定义。 

✓ 例如：Linear层的权重和偏置 

✓ Conv2d层的in_channels, out_channels, kernel_size, stride=1,padding=0, dilation=1,  groups=1,bias=True, padding_mode='zeros'这一系列参数

**说明**
`bigotimes`: 表示二维的相关系数计算 `stride`: 控制相关系数的计算步长
`dilation`: 用于控制内核点之间的距离，详细描述在[这里](https://github.com/vdumoulin/conv_arithmetic/blob/master/README.md)
`groups`: 控制输入和输出之间的连接： `group=1`，输出是所有的输入的卷积；`group=2`，此时相当于有并排的两个卷积层，每个卷积层计算输入通道的一半，并且产生的输出是输出通道的一半，随后将这两个输出连接起来。

参数`kernel_size`，`stride,padding`，`dilation`也可以是一个`int`的数据，此时卷积height和width值相同;也可以是一个`tuple`数组，`tuple`的第一维度表示height的数值，tuple的第二维度表示width的数值

**Parameters：**

- in_channels(`int`) – 输入信号的通道
- out_channels(`int`) – 卷积产生的通道
- kerner_size(`int` or `tuple`) - 卷积核的尺寸
- stride(`int` or `tuple`, `optional`) - 卷积步长
- padding(`int` or `tuple`, `optional`) - 输入的每一条边补充0的层数
- dilation(`int` or `tuple`, `optional`) – 卷积核元素之间的间距
- groups(`int`, `optional`) – 从输入通道到输出通道的阻塞连接数
- bias(`bool`, `optional`) - 如果`bias=True`，添加偏置

## 三、DataLoader函数

定义如下： DataLoader(dataset, batch_size=1, shuffle=False, sampler=None, num_workers=0,  collate_fn=default_collatem, pin_memory=False, drop_last=False) 

➢ 参数解释如下： 

✓ dataset：加载的数据集（Dataset对象）

 ✓ batch_size：batch size（批大小）

 ✓ shuffle：是否将数据打乱

 ✓ sampler：样本抽样 

✓ num_workers：使用多进程加载的进程数，0代表不使用多进程 

✓ collate_fn：如何将多个样本数据拼接成一个batch，一般使用默认的拼接方式即可 

✓ pin_memory：是否将数据保存在pin memory区，pin memory中的数据转到GPU会快一些

 ✓ drop_last：dataset中的数据个数可能不是batch_size的整数倍，drop_last为True会将多出来不足一 个batch的数据丢弃



## 四、计算机视觉工具包：torchvision 

视觉工具包torchvision独立于PyTorch，需要通过pip install torchvision安装。torchvision 主要包含以下三个部分：

 ✓ models：提供深度学习中各种经典网络的网络结构及预训练好的模型，包括Net、VGG系 列、ResNet系列、Inception系列等。 

✓ datasets：提供常用的数据集加载，设计上都是继承torch.utils.data.Dataset，主要包括 MNIST、CIFAR10/100、ImageNet、COCO等。

 ✓ transforms：提供常用的数据预处理操作，主要包括对Tensor以及PIL Image对象的操作



五、示例

```python
class MyLayer(torch.nn.Module):
    def __init__(self, in_features, out_features, bias=True):
        super(MyLayer, self).__init__() # 和自定义模型一样，第一句话就是调用父类的构造函数
        self.in_features = in_features
        self.out_features = out_features
        self.weight = torch.nn.Parameter(torch.Tensor(in_features, out_features)) # 由于weights是可以训练的，所以使用Parameter来定义。

```

