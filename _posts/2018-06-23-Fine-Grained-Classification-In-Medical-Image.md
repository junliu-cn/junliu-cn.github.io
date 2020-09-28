---
title: 'Fine-Grained Classification In Medical Image'
date: 2018-06-23
permalink: /posts/2018/06/Fine-Grained-Classification-In-Medical-Image/
tags:
  - Fine-Grained Image Classification
---

细粒度图像分类是指对一个大类下的子类进行精细分类，如区分不同品种的狗、不同门类的汽车等，细粒度图像分类是细粒度图像分析任务的一个重要方面。得益于近年来深度学习技术的快速发展，细粒度图像分类问题上的结果有了很大的性能提升，并且不断有基于深度学习的新方法提出，试图更好地解决这个问题。然而遗憾的是目前细粒度图像分类技术的应用主要还是针对自然图像，鲜少见到把细粒度图像分类技术应用到医学图像领域的工作，这当然跟医学图像的分析往往需要极强的专业医学知识有关系。本文尝试将细粒度图像分类技术用于解决肺小结节亚类以及良恶性分类问题，下面分两个部分进行展开，第一部分回顾基于深度学习技术的细粒度图像分类问题的典型方法；第二部分阐述如何应用细粒度图像分类技术解决肺小结节分类问题。

# 基于深度学习的细粒度图像分类
细粒度图像分类任务相对通用图像（general/generic images）任务的区别和难点在于其图像所属类别的粒度更为精细[[1]](https://zhuanlan.zhihu.com/p/24738319)。更具体的来说，细粒度图像分类任务的难处在于图像的巨大类内差异和微小的类间差异，这一点在医学图像中体现的更加突出。如下图所示，可以看到第一行磨玻璃结节在大小、形态等外在表征上可谓千差万别，但同时注意到如果纵向每一列进行比较的话，磨玻璃结节和部分实性结节在外在表征上又有极其相似的特点，这给分类带来了很大的困难。

![image](https://farm2.staticflickr.com/1949/43967138685_dbbf04c44b_o.png)

因此，如何有效地捕捉图像中强判别度的精细特征成为解决细粒度图像分类问题的关键。根据[13]，目前已有的基于深度学习的细粒度图像分类方法可以被分为以下四类：1）直接使用一般的分类模型如VGG net[2]、ResNet[3]等，对细粒度图像进行分类；2）对细粒度物体进行物体部件的细分，基于有强判别度的物体部件局部特征进行分类；3）使用多模型集成对细粒度图像进行分类；4）使用注意力机制自动学习细粒度图像中有强判别度的特征，从而完成分类。下面分别对每一类中比较有代表性的方法进行介绍。

### 基于通用卷积神经网络的方法
卷积神经网络在大规模数据集ImageNet上展示出了强大的威力，人们认识到卷积神经网络可以从图像中提取出非常具有区分度的特征，因此有人将在ImageNet上预训练的模型用于细粒度图像分类问题。比较典型的有VGG net、ResNet等。目前亚类分类模型就是从ResNet50在ImageNet的预训练模型上微调得到，虽然这种方法简单高效，但是针对肺小结节这种超细粒度图像分类问题，要达到非常满意的结果，还需要精心地设计有针对性的模型。

### 基于物体部件检测和对齐的方法
这类方法的核心思想是通过定位不同的物体部件，进而通过特征提取网络得到更有针对性的强辨识度的精细特征用于图像分类，以此提高细粒度图像的分类性能。

- Part-based R-CNN[4]
![image](https://farm2.staticflickr.com/1902/31005453338_01438a7614_o.png)

Part-based R-CNN是对R-CNN[5]的扩展，基本流程是首先利用Selective Search[16]得到物体以及物体部件的候选框，然后训练检测物体以及物体部件的模型，并对检测模型得到的物体以及物体部件边界框加上几何约束，得到合理的结果。之后在此基础上进行特征提取，最终得到物体以及物体部件的特征表示，并将其级联用于分类模型的训练。由于考虑了物体整体特征以及物体部件精细特征，Part-based R-CNN得到的分类结果比较理想，但这种方法比较依赖物体部件的检测精度，在复杂背景的图像中，比如存在遮挡时，物体部件的检测性能往往不能令人满意，这时候需要在测试时提供人工标注的物体边界框信息才能有较高的分类精度，这限制了Part-based R-CNN在实际中的应用。

- Multi-Proposal Consensus[6]

与Part-based R-CNN通过几何约束来得到合理的物体部件框不同，Multi-Proposal Consensus通过修改AlexNet[7]的FC8层为两个输出层，同时得到15个物体部件的特征点坐标及其可见性的置信度，然后通过设定阈值对可见性的置信度进行过滤，再进一步通过中心点算法确定最终的特征点。如下图所示，可以看到预测眼睛特征点的过程。

![image](https://farm2.staticflickr.com/1915/31005453628_5a273d2d35_o.png)

具体流程是首先训练物体部件特征点回归网络，使用ImageNet上的预训练模型初始化权重，并从每张图像得到edge box crops[8]作为模型输入进行训练。物体部件特征点预测模型训练好之后，可以通过预测的特征点得到不同物体部件（头、躯干）的边界框，然后利用位置信息产生图像块作为分类模型的输入。在分类部分，通过AlexNet提取图像块的特征，具体做法是级联所有图像块的FC6层特征得到整张图像的特征表示并用于分类。

- Pose Normalized Nets[9]

与Multi-Proposal Consensus通过修改AlextNet网络的输出层，回归得到物体部件特征点不同，Pose Normalized Nets利用DPM算法[10]得到13个预定义的物体部件特征点及其可见性的置信度。此外，Pose Normalized Nets还通过计算物体的姿态参数来进行物体以及物体部件姿态对齐的操作，这使得提取的特征更具有鲁棒性。

![image](https://farm2.staticflickr.com/1958/31005452798_008089aefb_o.png)

基于CNN不同层的特征具有不同的表示特性，Pose Normalized Nets还提出针对不同的物体部件提取不同网络层的特征，最后将所有的特征级联在一起作为整张图像的特征表示，用于分类。

- Part-stack CNN[11]

与之前的方法不同，Part-stack CNN利用分割的思想解决物体部件的定位问题，具体做法是利用part annotation生成物体部件矩形块的mask，训练FCN分割子网络。然后使用训练好的FCN网络预测结果获得物体部件的位置信息，再利用位置信息对AlexNet网络中间的卷积特征图做part crop操作就可以得到物体部件级别的特征表示，物体级别的特征则通过人工标注的边界框获得，最后将这些特征级联用于分类。具体的算法流程图如图所示：

![image](https://farm2.staticflickr.com/1935/44878707831_1190065008_o.png)

该工作创新性的将FCN网络用于解决细粒度图像物体部件定位问题，具有以下优点：
1. FCN的输出结果可直接用于分类子网络；
2. 通过FCN可一次性得到所有物体细分部件的位置信息；

- Mask-CNN[12]

Mask-CNN与Part-stack  CNN出现于同一时期，同样利用FCN来定位物体部件，之后利用物体部件的位置信息分别提取各个物体部件的特征。

![image](https://farm2.staticflickr.com/1920/31005453828_41dd2e9392_o.png)

与Part-stack CNN利用同一个AlexNet子网络提取多个物体部件特征不同的是Mask-CNN分别用了两个子网络来提取不同部件的特征，此外，还有一个筛选关键卷积特征描述子的操作，如图(c)-(d)所示。作者表示筛选特征描述子的好处在于，可以保留表示前景的描述子，而去除表示背景的卷积描述子的干扰。之后，对筛选过的特征描述子进行全局平均和最大池化操作并级联作为子网络的特征表示。对于物体整体则是将各个物体部件的mask进行加和得到位置信息，特征提取操作与物体部件类似。最后将三个子网络输出的特征再次级联作为整张图像的特征表示。Mask-CNN在经典CUB数据集上取得了目前细粒度图像分类最高的分类准确率（在经典CUB数据上，基于ResNet的模型对200类不同鸟类分类精度可达87.3%）。值得关注的是，该工作对提取特征的后处理方法，作者通过实验证明了筛选特征描述子对于选择细粒度图像精细特征的有效性。


### 基于网络集成的方法
- Bilinear CNN[14]

Bilinear CNN使用图像级别(class label)而不是物体部件级别(part annotation)的标注进行训练，因此对数据的标注要求比较低，具体的流程图如下图所示。stream A和stream B的作用是特征提取函数，其输出结果通过外积操作汇聚得到bilinear特征，并用于之后的图像分类。
![image](https://farm2.staticflickr.com/1956/31005454248_d1371ec4db_o.png)

与基于物体部件检测方法不同的是，Bilinear CNN分别利用两个模型完成物体部件的定位和特征提取功能，形式上更简单，整体性更好，实际上训练也更简单。值得一提的是Bilinear CNN具有很好的泛化性能，这种网络架构还被用于其他的图像分类任务，如行人重检测（person Re-ID）。


### 基于注意力机制的方法
对图像进行精细标注的代价是很大的，一张鸟类图像的mask可能需要人工花半个小时进行标注，这一点在医学图像上更加突出。基于注意力机制的方法核心思想是通过一种学习机制，让模型自动学习到图像中有区分度的特征，从而可以避免人工花费大量精力进行物体部件的标注工作。
- Two Level Attention Model[15]

Two Level Attention Model在只使用类别标注信息的情况下，完全依赖算法本身完成物体和物体局部区域的检测。

![image](https://farm2.staticflickr.com/1909/29941925187_3873e70773_o.png)

该模型主要分为三个阶段。(1) 预处理模型：利用Selective search[16]从输入图像中产生大量的图像块，通过物体级别的FilterNet对这些图像块进行过滤，保留包含前景物体的图像块；(2) 物体级模型：训练一个网络实现对物体级图像进行分类。在此需要重点介绍的是(3)物体部件级模型。我们来看，在不借助part annotation的情况下，该模型怎样做到part检测。

作者发现可以利用物体级模型训练的网络来对每一个图像块提取特征，接着对这些特征进行谱聚类，得到k个不同的聚类簇，每个簇可视为代表一类物体部件信息，如头部、脚等。则可以将每个簇看做一个区域检测器，从而达到对测试样本局部区域检测的目的。


# 应用细粒度图像分类技术解决肺小结节分类问题
### 问题区别
肺小结节亚类分类问题与自然图像的物体分类主要有三个不同点：
1. 肺小结节没有物体和物体部件的概念，结节本身是一个非常微小的病灶，3mm以下的结节在512X512尺寸的图像上就是表现为一个小点，这种情况下，很难从图像中获得有类别区分度的信息。
2. 肺小结节亚类分类是在结节检出之后，相对于自然图像的物体分类而言已经有了位置信息，故只需要关注如何提取有强分辨度的精细特征以及如何组合这些特征以提升分类性能。
3. 图像背景这个概念在自然图像和医学图像中具有不同的意义。 自然图像中细粒度图像的分类是在物体级别(Object Level)的分类，比如不同物种的鸟类、不同类别的汽车等都是在物体级别，即鸟、汽车的级别上进行分类，其图像背景对于鸟或者汽车的分类是没有帮助的，甚至增加了分类的复杂度。而对肺小结节而言，其分类是结节级别(Nodule Level)的分类，病灶是良性还是恶性，跟病灶环境，即图像背景，关系紧密，例如恶性结节的周围血管相对更粗。因此，在医学细粒度图像分类问题中，图像背景往往也包含了一些很重要的判别信息。

### 难点分析
1. 不同数据来源的CT图像Hu值变化范围差异很大，这导致使用固定的窗宽窗位截取Hu值造成很大的variance。
2. 同一个Patch中可能包含两个或者以上结节，并且这些结节可能属于不同的类别，包括不同的亚类以及良恶性等，这给结节的分类造成了很大困难。
3. 对于直径小于3mm的微小结节，结节本身的图像信息很少，这种情况下对结节的分类极其困难。

### 肺小结节图像特征分析
要提取有强辨别度的结节特征，首先要知道结节有哪些典型特征，以磨玻璃结节为例，其主要特征表现在形态、边缘、内部结构等，下面分别进行介绍。
###### 形态特征

- 分叶征

![image](https://farm2.staticflickr.com/1922/43967139025_140b90702b_o.png)

征象：表现结节表面呈凹凸不平的多个弧形，形似多个结节融合而成。

病理：肿瘤边缘细胞分化程度不一，生长速度不同，生长不均匀。肺结缔组织间隔限制，肿瘤生长受限。


###### 边缘特征
- 毛刺征

![image](https://farm2.staticflickr.com/1916/43967138885_6618b0586c_o.png)

征象：表现为自结节边缘向周围生长的、放射状的、无分支的直而有力的短线条影，结节端略粗。

病理：肿瘤内部成纤维化反应导致肺泡壁和小叶间隔向肿瘤聚集，以及肿瘤浸润瘤体周围支架结构或伴炎症反应。

- 胸膜凹陷征

![image](https://farm2.staticflickr.com/1944/44878707341_808dde6465_o.png)

征象：表现为规则线条影自结节牵拉胸膜,胸膜凹入形成典型喇叭口，胸膜凹入处为液体。

病理：肿瘤内纤维瘢痕收缩，导致胸膜牵拉。

###### 内部结构
- 空泡征

![image](https://farm2.staticflickr.com/1975/31005454138_9d3c6b3dbd_o.png)

征象：结节内透亮影

病理：肿瘤实体内残存的未受累的肺支架结构；或肺泡、扩张扭曲的支气管、黏液腺腔结构。

- 血管集束征

![image](https://farm2.staticflickr.com/1921/29941924857_1dca04b065_o.png)

征象：肺结节周围一支或几支血管到达瘤体内，在瘤体边缘截断或穿过瘤体。

病理：认为与恶性病变血供丰富有关。


- 血管增粗、紊乱
![image](https://farm2.staticflickr.com/1915/31005452478_3c65ebda86_o.png)


# References
[[1] 「见微知著」——细粒度图像分析进展综述](https://zhuanlan.zhihu.com/p/24738319)

[2] Simonyan, Karen, and Andrew Zisserman. "Very deep convolutional networks for large-scale image recognition." arXiv preprint arXiv:1409.1556 (2014).

[3] He, Kaiming, et al. "Deep residual learning for image recognition." Proceedings of the IEEE conference on computer vision and pattern recognition. 2016.

[4] Zhang, Ning, et al. "Part-based R-CNNs for fine-grained category detection." European conference on computer vision. Springer, Cham, 2014.

[5] Girshick R, Donahue J, Darrell T, et al. Region-based convolutional networks for accurate object detection and segmentation[J]. IEEE transactions on pattern analysis and machine intelligence, 2016, 38(1): 142-158.

[6] Shih K J, Mallya A, Singh S, et al. Part localization using multi-proposal consensus for fine-grained categorization[J]. arXiv preprint arXiv:1507.06332, 2015.

[7] Krizhevsky, Alex, Ilya Sutskever, and Geoffrey E. Hinton. "Imagenet classification with deep convolutional neural networks." Advances in neural information processing systems. 2012.

[8] Zitnick, C. Lawrence, and Piotr Dollár. "Edge boxes: Locating object proposals from edges." European conference on computer vision. Springer, Cham, 2014.

[9] Branson, Steve, et al. "Bird species categorization using pose normalized deep convolutional nets." arXiv preprint arXiv:1406.2952 (2014).

[10] Branson, Steve, Oscar Beijbom, and Serge Belongie. "Efficient large-scale structured learning." Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2013.

[11] Huang, Shaoli, et al. "Part-stacked cnn for fine-grained visual categorization." Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition. 2016.

[12] Wei, Xiu-Shen, Chen-Wei Xie, and Jianxin Wu. "Mask-cnn: Localizing parts and selecting descriptors for fine-grained image recognition." arXiv preprint arXiv:1605.06878 (2016).

[13] Zhao, Bo, et al. "A survey on deep learning-based fine-grained object classification and semantic segmentation." International Journal of Automation and Computing 14.2 (2017): 119-135.

[14] Lin, Tsung-Yu, Aruni RoyChowdhury, and Subhransu Maji. "Bilinear cnn models for fine-grained visual recognition." Proceedings of the IEEE International Conference on Computer Vision. 2015.

[15] T. Xiao, Y. Xu, K. Yang, J. Zhang, Y. Peng, and Z. Zhang. The application of two-level attention models in deep convolutional neural network for fine-grained image classification. In Proceedings of IEEE Conference on Computer Vision and Pattern Recognition, pages 842–850, Boston, MA, Jun. 2015.

[16] Uijlings, Jasper RR, et al. "Selective search for object recognition." International journal of computer vision 104.2 (2013): 154-171.
