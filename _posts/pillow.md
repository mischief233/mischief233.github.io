# pillow

## 一、使用Image类

```
from PIL import Image
im = Image.open("hopper.ppm")
print(im.format, im.size, im.mode)
PPM (512, 512) RGB
#宽度和高度
im.show()
```



## 二、读写图像

### 1、将文件转换为jpeg

```
import os, sys
from PIL import Image

for infile in sys.argv[1:]:
    f, e = os.path.splitext(infile)
    outfile = f + ".jpg"
    if infile != outfile:
        try:
            with Image.open(infile) as im:
                im.save(outfile)
        except OSError:
            print("cannot convert", infile)
```

### 2、剪切、粘贴和合并图像

### （1）从图像复制子面角

```
box = (100, 100, 400, 400)
region = im.crop(box)
```

区域由四元组定义，其中坐标为（左、上、右、下）。python图像库使用左上角带有（0，0）的坐标系。还要注意，坐标是指像素之间的位置，因此上面示例中的区域正好是300x300像素。

### （2）粘贴图片

```python
from PIL import Image
 
a = Image.new('RGB', (300, 300), (255, 0, 0))  # 生成一张300*300的红色图片
b = Image.new('RGB', (200, 200), (0, 255, 0))  # 200*200的绿色图片
a.paste(b, (50,50)) # 这个点是左上角
a.show()  # 显示a
```

粘贴区域时，区域的大小必须与给定区域完全匹配。此外，区域不能扩展到图像之外。但是，原始图像和区域的模式不需要匹配。如果没有，则在粘贴之前区域将自动转换。



