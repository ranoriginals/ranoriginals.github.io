# SPECFEM2D避免数值频散


在使用SPECFEM2D计算理论地震图的过程中，`Par_file`中`output_color_image`选项决定了是否输出波场的snapshot，这些snapshot会在OUTPUT_FILES里被命名为`forward_image*.jpg`，有了这样的输出文件之后，我们应该怎么评价这次的计算结果是否掺杂因数值频散而出现的噪声？

先说结论：

> 区域内出现的最短的波长必须大于网格的尺寸

### 首先给出一个不太合理的例子

![noise](https://raw.githubusercontent.com/ranoriginals/ranoriginals.github.io/master/images/noise.png)

上图分别给出了波场在4s，40s，76s的snapshot，注意4s图中黄色方框部分，相较于传播更快的P波，这里的S波在第一层介质中的传播过程中伴随有噪声出现，而这些噪声在4s之后的时间(如40s，76s)会逐渐传播至整个区域。由于数值方法在模拟地震波传播时，如果解是稳定的，则只会有地震波的存在，不会出现噪声。因此这里出现噪声证明了这次模拟(即数值解)不够稳定，出现了数值频散。

### 那么，对于二维的SEM稳定的数值解主要依赖于：

1.在对整个研究区域网格化时，`网格的大小必须小于区域内可能出现的波长最小值`。如果不这样设置，解会不稳定,就会出现噪声

2.DT的设置必须要满足`DT < constant*min(h/v)`，其中constant是一个常数，一般取值为0.3，h是两个GLL积分点之间的距离，v是介质中波传播的速度。min(h/v)意味着最小的网格点距离除以最大的速度。详细内容请见SPECFEM3D的user_manual中4.2节。如果网格变小，DT未随之调整变小，则会有如下报错信息。

![error](https://raw.githubusercontent.com/ranoriginals/ranoriginals.github.io/master/images/error.png)

### 具体示例

下面给出一个具体的简单的例子，研究的区域大小为200km*100km，其中X方向长为200km，Z方向长为100km，震源时间函数设置为Ricker，主频设置为2Hz。整个区域设置为3层模型，每层的厚度为30km/30km/40km，每层模型的速度-密度参数如下图：

![model](https://raw.githubusercontent.com/ranoriginals/ranoriginals.github.io/master/images/model.png)

1.在网格的大小设置上，区域内可能出现的最小波长主要与两个因素有关，一是最小的波传播的速度，二是震源频带范围的最大频率。最小的波传播的速度，也就是最小的的S波速度，为3200m/s。最大频率可以对已有的SAC文件做傅立叶变换得到，虽然之前得到了不合理的模拟结果，但是可以通过对生成的SAC文件做傅立叶变换，得到SAC文件的振幅谱图像(如下)，找出SAC文件的主要频段，也是震源时间函数的主要频段，通过下图我们可以看出主频为2Hz的震源，其有效信息的最大频率为6Hz左右，所以区域内可能出现的最小波长为3200/6=533.3m，因此将网格的大小设置为500m*500m。

![Am](https://raw.githubusercontent.com/ranoriginals/ranoriginals.github.io/master/images/Am.png)

2.在DT的选择上，如果过大会报错，如果过小则NSTEP要设置的很大才能得到足够长的地震记录，但这也会造成计算的时间会很长，所以需要尽可能取最大值。一般常数取0.3，h的大小取决于网格的大小和一个网格中GLL积分点的个数，GLL积分点的个数默认为5，h为两个GLL积分点之间的距离。一些尝试表明，在计算DT的选择范围时，h可以认为等于网格的大小/GLL积分点的个数。则对于此次模拟来说，DT应该小于0.3*(500/5/5800)=0.005172，所以DT选择0.005是合适的。在实际的计算过程中，可以使用这一关系进行大致估计，如果不出现上述的报错则说明设置的DT是正确的。

![no_noise](https://raw.githubusercontent.com/ranoriginals/ranoriginals.github.io/master/images/no_noise.png)

相较于之前(区域网格大小设置为1km*1km，DT取0.01，其余参数相同)，噪声不再出现，从波场图的角度分析这是一个合理的模拟结果。

### 注：怎么基于已有的SAC文件绘制振幅谱

```bash
$ sac

SAC > r ./AA.S0001.BXZ.SAC
SAC > fft
SAC > psp am
SAC > q
```

SEM3D的user_manual: [点这里](https://github.com/geodynamics/specfem3d/blob/master/doc/USER_MANUAL/manual_SPECFEM3D_Cartesian.pdf)