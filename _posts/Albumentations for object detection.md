# Albumentations for object detection

## 安装&加载

```python
pip install -U albumentations
```

```python
import albumentations as A
import cv2
```



## 主要数据增强方法

### Compose与参数传递

在该包中，所有的增强方法均需被封装于`Compose`类中，通过以下形式进行构建

重点关注参数bbox_params

```python
transform = A.Compose([
    A.way1(params),
    A.way2(params),
    A.way3(params),
    ...
    A.wayN(params,p=),
    '''
    所有way均会被执行，可以额外增加参数p控制该方法进行的概率
    '''
	],bbox_params=A.BboxParams(
    format='coco',
    '''
    四种可选择（示意图如下）
    '''
    min_area=100,
    min_visibility=0.3,
    '''
    控制变换后bbox的筛选标准（分别从bbox大小的绝对值和相对值进行控制，低于对应阈值直接排除对应	     bbox
    例如，若变换后的bbox尺寸（像素长乘宽）小于100像素点，则丢弃对应的bbox，
    若变换后的bbox尺寸除以变换前bbox尺寸商小于0.3，则丢弃对应bbox
    '''
    
))
```

![image-20210409110324361](C:\Users\wangjinsong\AppData\Roaming\Typora\typora-user-images\image-20210409110324361.png)

两种传递bboxes方法

```python
bbox1=[
[23,22,75,80,'1'],
]

bbox2=[
    [23,22,75,80],
]
label2=['1']

transformed1=transform(image=image,bboxes=bbox1)
transformed2=transform(image=image,bboxes=bbox2,class_labels=label2)

 '''
 其中包含
 '''
transformed1['image']
transformed2['image']

transformed1['bboxes']
transformed2['bboxes']
transformed2['class_labels']
```



 ### 常见的way

```python
A.HorizontalFlip(p=0.5), #水平翻转
A.ShiftScaleRotate(),#随机放射变换



```

等

可参考

https://www.cnblogs.com/54hys/p/12694084.html

## 示范代码

```python
import albumentations as A
import cv2



transform=A.Compose([
 A.RandomCrop(width=30,height=40),
 A.HorizontalFlip(p=0.7),
 A.RandomBrightnessContrast(p=0.3),
],bbox_params=A.BboxParams(format='coco')
)



image1=cv2.imread('xxx.jpg')
image1=cv2.cvtColor(image1,cv1.COLOR_BGR2RBG)
bbox=[
    [12,33,45,76,'1']
]


transformed=transform(image=image1,bboxes=bbox)
transformed_image=transformed['image']
transformed_bboxes=transformed['bboxes']

'''
保存transformed_image及其对应bbox和标签transformed_bboxes即可
'''
```

