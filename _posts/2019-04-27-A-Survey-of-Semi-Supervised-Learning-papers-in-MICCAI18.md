---
title: 'A Survey of Semi-Supervised Learning papers in MICCAI1818'
date: 2019-04-27
permalink: /posts/2019/04/A-Survey-of-Semi-Supervised-Learning-papers-in-MICCAI18/
tags:
  - Semi-Supervised-Learning
---

&emsp;&emsp;主要对MICCAI18的半监督学习的文章进行了survey。通过"semi-supervised"关键字进行检索，发现4篇文献标题包含该关键字。4篇文章无一例外都在做分割任务，[1]应用在盆骨器官分割，[6]做脑区域分割，[9]研究心肌分割，[11]做肝脏、肿瘤分割，下面分别进行介绍。

#### **Attention Based Semi-supervtised Deep Networks for Medical Image Segmentation**[1]

&emsp;&emsp;该文章提出一种新的半监督学习框架来解决盆骨器官分割问题，该框架引入了对抗生成网络的思想。具体来说，即用简化的VNet[2]（internal pool-conv-deconv   are removed）作为生成网络，输出segmentationm map。原始的VNet网络结构如下图所示：
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-14%20%E4%B8%8A%E5%8D%8810.24.59.png?token=AJ0zaSBgfc9NLe-4Zb0ZyGw8lMGRhf_Eks5cu9StwA%3D%3D)

判别网络则使用FCN[3]形式的网络输出分割网络结果的confidence map(M)，从而可以对confidence map设置超参数阈值`$\gamma$`(`$M>\gamma$`)得到segmentation map的可靠区域，作者将之称作region attention。基于confidence map，作者设计了一个semi-supervised loss，从而可以将unlabeled data加入训练，达到半监督的目的。算法的pipeline如下图所示：
![pipeline](https://farm8.staticflickr.com/7818/47450920622_0ddc173ae6_k.jpg)

&emsp;&emsp;Confidence Network的损失函数Adversarial Loss定义如下，
![Adversarial Loss](https://farm8.staticflickr.com/7840/46780055494_c91a1dec0c_k.jpg)
其中，X、P分别代表输入图像和对应的label，S()、D()分别代表Segmentation Network和Confidence Network，`$\theta_d$`为网络参数，`$L_{BCE}(\hat{Q}, Q)$`就是binary cross entropy的加和。


&emsp;&emsp;Segmentation Network的损失函数由三个部分组成，第一个部分的loss设计，作者参考Focal Loss[4]的思想，引入了所有类别的平均Dice系数dsc作为该sample的概率值，从而实现所谓的sample attention，具体定义如下：
![](https://farm8.staticflickr.com/7845/46588366955_8b86301e8a_k.jpg)
该loss函数的第一项完全模仿的focal loss，第二项为Multi-class Dice Loss[5]，其中，
`$\pi_c = 1 / (\Sigma_{h=1}^{H}\Sigma_{w=1}^{W}\Sigma_{t=1}^{T}P_h,w,t,c)^2 $`，H, W, T为输入图像的不同维度，c为类别数。P为标注的label，`$\hat{P}$`为分割网络输出的segmentation map。
第二个部分是作者定义的
Adversarial Loss和confidence network的损失函数类似，
![](https://farm8.staticflickr.com/7866/47504472031_7f52db7d82_k.jpg)，作者称该loss可以迫使分割网络保持预测的segmentation map和ground truth map之间的高阶一致性。第三个部分则是半监督的loss，定义如下：
![](https://farm8.staticflickr.com/7907/46588313135_58c5019076_k.jpg)，其中，`$\bar{P}$`为`$argmax(\hat{P})$`的one-hot encoding形式，`$[]$` 是一个 indicator function，M为Confidence Network输出的confidence map，`$\gamma$`为设定的超参数，即只有confidence map中大于该参数的部分会被用来计算分割网络的Semi-supervised loss，遗憾的是，作者并没有在文章中说明该参数在他们实验中的取值。在Implementation Detail中，也没有说明模型训练的详细步骤，个人觉得这个模型很难做到end-to-end的训练，应该是先将Segmentation Network训练好，然后再加入判别网络Confidence Network进行训练，最后才将unlabeled data加入训练。

&emsp;&emsp;作者在两个数据集上做了实验。其中，第一个数据集包括50个有标注和20个无标注的病人数据，第二个数据集是[PROMISE12-challenge](https://promise12.grand-challenge.org/)数据集，这个数据集用来验证文章所提方法的泛化性能。两个数据集的详细信息统计如下：
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-13%20%E4%B8%8B%E5%8D%8810.43.07.png?token=AJ0zaag41WQXDaYrTNR6Kx6lRq37TdFQks5cuzBfwA%3D%3D)
模型的输入尺寸为`$64\times64\times16$`，测试使用sliding window的方式遍历整个volume，遗憾的是作者没有说明data augmentation细节。

作者在第一个数据集上和另外5种state-of-the-art方法做了比较，这5种方法分别是(1) multi-atlas label fusion (MALF), (2) SSAE [14], (3) UNet [15], (4) VNet [16], and (5) DSResUNet [17]。
Qualitative和Quantitative分析结果分别如下所示：
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-14%20%E4%B8%8A%E5%8D%889.44.41.png?token=AJ0zaTZwYNJrdofTLNzb4ZerhSRC_llKks5cu9AewA%3D%3D)

Metric是Dice Similarity Coefficient(DSC)和 Average Surface Distance(ASD)。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-14%20%E4%B8%8A%E5%8D%889.55.02.png?token=AJ0zaagWYp09FnXda55nI5sHT8ouNag6ks5cu9AhwA%3D%3D)
作者又进一步分析了每个模块（Sample Attention Loss，CNN-based and FCN-based discriminator，Semi-supervised Loss）对结果的影响。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-14%20%E4%B8%8A%E5%8D%889.57.43.png?token=AJ0zaZd7rF9CgHoulty0r7zz7-GgTM-Cks5cu9AlwA%3D%3D)



#### **Semi-supervised Learning for Segmentation Under Semantic Constraint[6]**
&emsp;&emsp;该文章介绍可以利用某些医学图像的解剖结构不变性先验知识进行建模，进而提出NonAdjLoss，对解剖结构之间的连接性进行建模，并在MR T1脑图像上进行了实验。该方法并非端到端的训练，首先，在训练集上训练好[7]类似的经典Encoder-Decoder网络，如下图所示：

![Encoder-Decoder](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-07%20%E4%B8%8B%E5%8D%886.59.56.png?token=AJ0zacIFUdHhk2tw3tytCdeooj59zKrbks5cszxSwA%3D%3D)

其次，从训练集Ground-Truth中提取出相邻解剖结构之间的邻接先验知识，文章中表示为`$\tilde{A}=(A > 0)$`，其中，`$A$`为作者定义的不同结构之间的邻接图，比如结构`$i, j$`之间的连接权重可以表示为：`$A_{i, j} = \sum_{_x}^{}\sum_{_v\in_{V}}^{}\delta_{i, s(x)}\delta_{j, s(x - v)} $`，`$x$`为voxel，`$s(x)$`为voxel到label 的映射函数，`$\delta$`为Kronecker delta函数，`$V$`是`$x$`的邻域并且不包括0。最终，`$A$`可以用来表示3D Volume中不同结构之间的邻接contour数，并且`$\tilde{A}$`对于所有的Subject都是不变的。基于此，作者提出了Non-Adjacency loss，如下图所示：

![Non-Adjacency loss](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-07%20%E4%B8%8B%E5%8D%889.13.40.png?token=AJ0zac9V3WwSCRfycO7kVG4O3rRSSddMks5cszx5wA%3D%3D)

其中，`$L$`可以为任何segmentation loss，`$\phi{(x, w)}$`表示网络输出, `$D_S$`为训练数据集，`$G(w)$`如下图所示：
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-07%20%E4%B8%8B%E5%8D%889.13.50.png?token=AJ0zadnDn_gejuzvQMrQTVr7yutrLQX9ks5csz2iwA%3D%3D)
其中，`$a_{i, j}$`为根据网络输出概率图得到的结构`$i, j$`的邻接属性，`$D_{N A}$`
为无标注的数据集，`$F$`集合在文章中并没有说明含义，个人理解应当是存在错误连接的结构集合。

作者将NonAdjLoss乘上预先定义的比例`$\lambda$`加上segmentation loss作为最终的loss，应用在第一步中训练好的模型上，进行fine-tuning。更进一步，由于NonAdjLoss的计算不需要人工标注，作者将之应用到无标注数据集`$D_{N A}$`上，从而有了Semi-supervised Learning，作者声称这个部分有利于模型的泛化。

作者在MICCAI 2012 multi-atlas challenge, IBSRv2以及OASIS[8]三个数据集上做了实验，数据集的划分如下：
![dataset](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-07%20%E4%B8%8B%E5%8D%8810.35.28.png?token=AJ0zaVvT1FWp0tLwNoSDkDNzIO9XmbOdks5cs0ZQwA%3D%3D)

所有的图像都做了Affine Registration，并使用N4ITK进行Bias Field Correction操作。对于Data Augmentation部分，作者没有详细说明，只是提到主要使用了elastic deformation[18]方法。
作者给出了Qualitative分析的实验结果，得出结论：NonAdjLoss有效减少了错误连接数，同时很好的保留了正确连接数，semi-supervised则进一步提升了结果。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-07%20%E4%B8%8B%E5%8D%8810.36.27.png?token=AJ0zafYITbGEZrXDptXQkFvKFwbELyQ2ks5cs0aTwA%3D%3D)

作者又通过Quantitative分析，研究了semi-supervised对结果的影响，得出结论：semi-supervised加强了模型的泛化能力。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-07%20%E4%B8%8B%E5%8D%8810.37.26.png?token=AJ0zabCm0YmtIGoN55pt-dvji_qKDar2ks5cs0a6wA%3D%3D)

#### **Factorised spatial representation learning: application in semi-supervised myocardial segmentation**[9]
&emsp;&emsp;这篇文章提出的思想比较有意思，跟我之前的一个Idea非常类似，即通过一种Decomposer-Reconstructor的机制学习图像的latent representation。
![fig2](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-08%20%E4%B8%8B%E5%8D%8810.21.32.png?token=AJ0zaXaYBIS5r27Lnw3c8SapK18PBH4Gks5ctJQIwA%3D%3D)

&emsp;&emsp;所谓SDNet，全称为spatial decomposition network, 核心思想是通过Decomposer得到原始图像的spatial map(Mask)和一个latent vector，分别对应anatomical information和intensity information(non-anatomical information)，再通过一个Reconstructor网络将之前分解的信息合成和原图一样的图像。这个过程可以迫使Decomposer网络学习到如何将一张图像分解成指定的不同成分信息。

&emsp;&emsp;作者设计的损失函数包括5个部分：1个reconstruction loss，2个supervised loss，还有2个adversarial loss。reconstruction loss定义为：

```math
L_{rec}(f, g) = \mathop{{}\mathbb{E}}_X[\|X - g(f(X))\|_1]
```
其中，`$f, g$`分别代表decomposer和reconstructor网络。2个supervised loss由mask的分割loss和利用Ground-Truth Mask重建的loss组成，分别定义为：

```math
L_M(f) = \mathop{{}\mathbb{E}}_X[Dice(M_X, f_M(X))], 

L_I(f, g) = \mathop{{}\mathbb{E}}_X[\|X - g(M_X, f_Z(X))\|_1]
```
最后，作者提到一个discriminator网络`$D_X$`，但是没有针对`$D_X$`进行详细说明，只给出了参考文献[10]，基于判别网络`$D_X$`，作者针对定义了2个adversarial loss，分别为：

```math
A_I(f, g, D_X) = \mathop{{}\mathbb{E}}_X[D_X(g(f(X))^2 + (D_X(X) - 1)^2], 

A_M(f) = \mathop{{}\mathbb{E}}_{X, M}[D_M(f_M(X))^2 + (D_M(X) - 1)^2]
```
所以，整体的loss可以写成如下形式：

```math
\lambda_1L_M(f) + \lambda_2A_M(f, D_M) + \lambda_3L_{rec}(f, g) + \lambda_4L_I(f, g) + \lambda_5A_I(f, g, D_X)
```
作者根据经验将`$\lambda_1, \lambda_2,\lambda_3,\lambda_4,\lambda_5$`分别赋值为`$10, 10, 1, 10, 1$`。在半监督学习部分，对于unlabelled image，该loss函数不包括第一和第四项。

&emsp;&emsp;作者在[ACDC](https://www.creatis.insa-lyon.fr/Challenge/acdc/databases.html)和[QMRI](https://www.ed.ac.uk/clinical-sciences/qmri-facilities)两个数据集上做了实验。这两个数据集详细情况分别如下所示：
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8A%E5%8D%8811.20.28.png?token=AJ0zaf7T7Cq0RByGdoOIJobdYn1t9iiwks5ctUpTwA%3D%3D)

![QMRI](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8A%E5%8D%8811.24.31.png?token=AJ0zadrebjFv1DP_MUQDEAeUfahWhl1Lks5ctUstwA%3D%3D)

&emsp;&emsp;作者通过Qualitative分析，可视化不同`$(M,Z)$`组合重建的图像，得出结论：Decomposer网络将原图中的不同信息分解到了Mask和Latent Vector中，Mask中包含了Shape和Position信息而Latent Vector中包含了non-anatomical信息（MR characteristics）。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-08%20%E4%B8%8B%E5%8D%8810.22.09.png?token=AJ0zaULMpztTezr1iKh1lpB4EgVvy0CXks5ctJRKwA%3D%3D)

对于分割结果，作者给出了Qualitative和Quantitative分析。并由此得出结论：在low-data regime情形下，文章所提半监督方法在myocardial分割任务中的有效性。遗憾的是，作者没有对Data Augmentation部分进行说明。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-08%20%E4%B8%8B%E5%8D%8810.22.20.png?token=AJ0zaX7UCUO4oBN_5WT2sEYh5vAQsQHeks5ctJROwA%3D%3D)
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-08%20%E4%B8%8B%E5%8D%8810.22.31.png?token=AJ0zaThgZ3QrEsH1jV2esmMYP01xxSqiks5ctJRSwA%3D%3D)


#### **A Diagnostic Report Generator from CT Volumes on Liver Tumor with Semi-supervised Attention Mechanism**[11]

&emsp;&emsp;这篇文章提出一种方法将分割模型和语言模型结合起来，分割病灶的同时，自动生成相应的诊断报告。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8A%E5%8D%8811.52.53.png?token=AJ0zaQGCFUUVBpbwMKJ-AzD9HvwMnycEks5ctW08wA%3D%3D)
这篇文章的想法其实没什么新意，利用FCN分割肝脏、肿瘤，再用LSTM生成格式化的诊断报告，这在目前很多公司的肝脏AI软件系统中都有实现。而且分割模型是用的2D FCN，在Slice Level来做的话会丢失3D的信息。

我现在还没想明白这篇文章中的semi-supervised attention是怎么实现的。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8A%E5%8D%8811.54.00.png?token=AJ0zacRtqG03-PMUliuL8kdPSM6wM4gtks5ctXelwA%3D%3D)

作者在[MICCAI 2017 LiTS Challenge](https://competitions.codalab.org/competitions/17094)数据集上进行了实验，训练集和测试集数据统计如下：

Train | Test
---|---
131 | 70
作者通过两个影像科专家和一个外科医生的合作，得到训练集数据的诊断报告，并用作语言模型的Label。
对于生成报告模型，作者进行了Quantitative和Qualitative分析。
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8A%E5%8D%8811.53.34.png?token=AJ0zaTLQc5G469x1lN43DJVAww_1Yad0ks5ctXeewA%3D%3D)

Metric是BLEU(B)[12]和ROUGE(R)[13]
![](https://raw.githubusercontent.com/chrisluu/Pics4PaperReading/master/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-09%20%E4%B8%8A%E5%8D%8811.53.52.png?token=AJ0zae0o-MJd1nQzeupM5eWtA8OOpJjWks5ctXeiwA%3D%3D)   





References：

[1] Nie D., Gao Y., Wang L., Shen D. (2018) ASDNet: Attention Based Semi-supervised Deep Networks for Medical Image Segmentation. In: Frangi A., Schnabel J., Davatzikos C., Alberola-López C., Fichtinger G. (eds) Medical Image Computing and Computer Assisted Intervention – MICCAI 2018. MICCAI 2018. Lecture Notes in Computer Science, vol 11073. Springer, Cham

[2] Milletari, Fausto, Nassir Navab, and Seyed-Ahmad Ahmadi. "V-net: Fully convolutional neural networks for volumetric medical image segmentation." 2016 Fourth International Conference on 3D Vision (3DV). IEEE, 2016.

[3] Long, J., et al.: Fully convolutional networks for semantic segmentation. In: CVPR,
pp. 3431–3440 (2015)

[4] Lin, T.-Y., et al.: Focal loss for dense object detection. arXiv preprint
arXiv:1708.02002 (2017)

[5] Sudre, C.H., et al.: Generalised dice overlap as a deep learning loss function for highly unbalanced segmentations. DLMIA/ML-CDS -2017. LNCS, vol. 10553, pp. 240–248. Springer, Cham (2017). https://doi.org/10.1007/978-3-319-67558-9 28

[6] Ganaye P A, Sdika M, Benoit-Cattin H. Semi-supervised learning for segmentation under semantic constraint[C]//International Conference on Medical Image Computing and Computer-Assisted Intervention. Springer, Cham, 2018: 595-602.

[7] Roy, A.G., Conjeti, S., Sheet, D., Katouzian, A., Navab, N., Wachinger, C.: Error corrective boosting for learning fully convolutional networks with limited data. In: Descoteaux, M., Maier-Hein, L., Franz, A., Jannin, P., Collins, D.L., Duchesne, S. (eds.) MICCAI 2017. LNCS, vol. 10435, pp. 231–239. Springer, Cham (2017). https://doi.org/10.1007/978-3-319-66179-7 27

[8] Marcus, D.S., Fotenos, A.F., Csernansky, J.G., Morris, J.C., Buckner, R.L.: Open
access series of imaging studies: longitudinal MRI data in nondemented and
demented older adults. J. Cogn. Neurosci. 22(12), 2677–2684 (2010)

[9] Chartsias, Agisilaos, et al. "Factorised spatial representation learning: application in semi-supervised myocardial segmentation." International Conference on Medical Image Computing and Computer-Assisted Intervention. Springer, Cham, 2018.

[10] Mao, X., Li, Q., Xie, H., Lau, R.Y., Wang, Z., and Smolley, S.P.: On the Effective-
ness of Least Squares Generative Adversarial Networks. arXiv:1712.06391 (2017)

[11] Tian, Jiang, et al. "A Diagnostic Report Generator from CT Volumes on Liver Tumor with Semi-supervised Attention Mechanism." International Conference on Medical Image Computing and Computer-Assisted Intervention. Springer, Cham, 2018.

[12] Papineni, K., Roukos, S., Ward, T., Zhu, W.J.: BLEU: a method for automatic evaluation of machine translation. In: ACL, pp. 311–318 (2002)

[13] Lin, C.-Y.: Rouge: a package for automatic evaluation of summaries, pp. 74–81. In: ACL Workshop (2004)

[14] Guo, Y., et al.: Deformable MR prostate segmentation via deep feature learning
and sparse patch matching. IEEE TMI 35, 1077–1089 (2016)

[15] Ronneberger, O., Fischer, P., Brox, T.: U-Net: convolutional networks for biomed- ical image segmentation. In: Navab, N., Hornegger, J., Wells, W.M., Frangi, A.F. (eds.) MICCAI 2015. LNCS, vol. 9351, pp. 234–241. Springer, Cham (2015).
https://doi.org/10.1007/978-3-319-24574-4 28

[16] Milletari, F., et al.: V-net: fully convolutional neural networks for volumetric med-
ical image segmentation. In: 3DV, pp. 565–571. IEEE (2016)

[17] Yu, L., et al.: Volumetric ConvNets with mixed residual connections for automated prostate segmentation from 3D MR images. In: AAAI (2017)

[18] Simard, P.Y., Steinkraus, D., Platt, J.C.: Best practices for convolutional neural
networks applied to visual document analysis. In: Proceedings of the Seventh Inter- national Conference on Document Analysis and Recognition, pp. 958–963, August 2003

