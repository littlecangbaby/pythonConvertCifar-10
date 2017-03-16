#找遍了网上都没有都没有直接用python将cifar-10转换成图片格式的方法，因此特开一个教程，供大家一起交流学习。本人致力于打造菜鸟教程。
##0.已经转好的图片格式供大家下载。
	[百度盘](https://pan.baidu.com/s/1skN4jW5)
	密码z6i3

从上到下依次为图片的0-9类，共10类，其中训练集5W张图片，每类5K张；验证集1W张图片，每类1K张。共计6W张图片。

![这里写图片描述](http://img.blog.csdn.net/20170316145353375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3VvaHVpZmVuZ2J5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##1.准备工具
* cifar10数据集，需要自行下载，并解压到与转换脚本同一文件夹内。keras官方下载路径为[keras的cifar-10官方下载路径](http://www.cs.toronto.edu/~kriz/cifar-10-python.tar.gz)
* python依赖库：numpy、等等。（如果运行过程中报错，请自行根据错误提示装上依赖的库）

##2.代码
```
#encoding:utf-8
#author：guohuifeng
#email：309884616@qq.com
from scipy.misc import imsave
import numpy as np

# 解压缩，返回解压后的字典
def unpickle(file):
    import cPickle
    fo = open(file, 'rb')
    dict = cPickle.load(fo)
    fo.close()
    return dict

# 生成训练集图片，如果需要png格式，只需要改图片后缀名即可。
for j in range(1, 6):
    dataName = "data_batch_" + str(j)  # 读取当前目录下的data_batch12345文件，dataName其实也是data_batch文件的路径，本文和脚本文件在同一目录下。
    Xtr = unpickle(dataName)
    print dataName + " is loading..."

    for i in range(0, 10000):
        img = np.reshape(Xtr['data'][i], (3, 32, 32))  # Xtr['data']为图片二进制数据
        img = img.transpose(1, 2, 0)  # 读取image
        picName = 'train/' + str(Xtr['labels'][i]) + '_' + str(i + (j - 1)*10000) + '.jpg'  # Xtr['labels']为图片的标签，值范围0-9，本文中，train文件夹需要存在，并与脚本文件在同一目录下。
        imsave(picName, img)
    print dataName + " loaded."

print "test_batch is loading..."

# 生成测试集图片
testXtr = unpickle("test_batch")
for i in range(0, 10000):
    img = np.reshape(testXtr['data'][i], (3, 32, 32))
    img = img.transpose(1, 2, 0)
    picName = 'test/' + str(testXtr['labels'][i]) + '_' + str(i) + '.jpg'
    imsave(picName, img)
print "test_batch loaded."
```

##3.检验
	根据本文的图片命名方式，采用"标签_当前batch中图片序号"的方式，可以解决图片名重复带来的问题。下面根据前文中第0章的大类展示来做下测试：
	
* 第0类，飞机，正确

![这里写图片描述](http://img.blog.csdn.net/20170316151053368?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3VvaHVpZmVuZ2J5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* 第6类，青蛙，正确

![这里写图片描述](http://img.blog.csdn.net/20170316151547854?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvZ3VvaHVpZmVuZ2J5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)							

##4.对未来工作的展望
cifar-100的图片转换类似，有兴趣的话请自行转换。

