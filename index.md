<html>
<table width="100%" border="0" align="center">
    <tr>
    <td width="5%" border="0"></td>
    <td width="60%" border="0">
        <div id="header" style="float:left">
        <h1>Jun Liu(刘俊)</h1>
        <p>Algorithm Engineer,  <br>
        PingAn Technology, Inc. <br>
	    Shen Zhen, Guang Dong, China. </p>
        <p> <em> Email: 
        <a href="mailto:jun.liu@cnu.edu.cn">jun.liu@cnu.edu.cn</a> 
        or 
        <a href="mailto:jun.liu.cnu@gmail.com">jun.liu.cnu@gmail.com</a> 
        </em> </p>
		<p> 
		</p>
        </div>
    </td>
    <td width="30%" border="0">
        <div id="photo" style="float:right;">
        <img src="./pic.jpeg" alt="my photo" style="width:311px;height:232;"> 
        </div>
    </td>
    <td width="5%" border="0"></td>
    </tr>
</table>
</html>


---
# About Me
I am currently a Deep Learning Algorithm Engineer at [PingAn Technology, Inc.](https://tech.pingan.com/en/) Formerly I was with [Shenzhen Institutes of Advanced Technology(SIAT) ](http://english.siat.cas.cn/)as a Research Assistant, which I joined after obtained  Master Degree from [the School of Mathematical Sciences, Capital Normal University](http://math.cnu.edu.cn/) at Jun 2017. I also interned at Text Processing Lab of Samsung Electronics and Cognitive Computing Lab of Intel. My research interests mainly revolve around Artificial Intelligence and Medical Image Analysis with special interest in deep learning techniques for high performance algorithm development.
Learn more about me, please check [CV](https://drive.google.com/open?id=127YPQskghsEp05eZeYwL_3EULCUIHbHM), [LinkedIn](https://www.linkedin.com/in/chris-j-liu-87471a111/). 


---

# Work Experience 
**03/19 - Present   Algorithm Engineer at [PingAn Technology, Inc](https://tech.pingan.com/en/)**
- Built a CS backend software which is based on LabelMe and designed for labeling multi-modal images.
- Develop an algorithm which is based on deep learning techniques to address Benign&Malignant classification problem for breast lesions.

**09/17 - 03/19    Algorithm Engineer at [Imsight Medical Technology, Inc](http://www.imsightmed.com/article/en), work with [Dr. Hao Chen](http://appsrv.cse.cuhk.edu.hk/~hchen/) and [Dr. Qi Dou](https://carrend.github.io/)**
- Algorithm Optimization works: optimize the speed and memory resource consumption for volumetric image analysis algorithms: Lung segmentation and Radiomics algorithms. I do extremely optimize the algorithms for 10 times faster and with almost 50% lower memory resource consumption;
- Applying state-of-the-art Fine-Grained related ideas to address large intra-class variance and small inter-class variance in Benign&Malignant and Subtype Multi-Class Classification for pulmonary nodule;
- I designed an effective, fast and accurate algorithm which is implemented in c++11 to detect the same nodules in different series of follow-up images; 
- Built a stable CS software called Lung-Sight which is a developed lung cancer screening system with the cutting-edge AI technologies. 

**07/17 - 09/17    Research Assistant  at [Institute of Biomedical and Health Engineering,  SIAT](http://english.siat.cas.cn/SI2017/IBHE2017/)**
- RMF based Vessel planar parameterization.

**09/16 - 11/16    Research Intern at Samsung Electronics (China)R&D Center**
- Scene Text Image Synthesis, Estimate Depth From Single RGB Image, Image Segmentation, Image Fusion, etc.

**05/16 - 09/16    Research Intern supervised by Dr. Shandong, Wang and [Dr. Yurong, Chen ](https://www.linkedin.com/company/intel-corporation/)    [Intel Labs China](https://www.linkedin.com/company/intel-corporation/)**
- 3D Human Face Reconstruction From Single 2D Image, Facial Expression Transfer, Face Frontalization, etc.

**01/15 - 03/16    Image Processing Engineer  Intern   XueXiBao**
- Image Denoise, Image Deblur, Image Warpping,  etc.


---
# Education
**2014 - 2017 Master's Degree in Applied Statistics     Capital Normal University**
- Optimization Theory and Algorithm; Data Structure; Numerical Analysis; Digital Image Processing; Computer Graphics; Pattern Recognition And Machine Learning, etc .

**2009 - 2013 Bachelor's Degree in Computer Science And Technology     Hu Nan Institute Of Technology**
- C/C++ Programming; Data Structure; Computer System; Computer Theory; Discrete Mathematics; Probability Statistics; Linear Algebra, etc.


---

# Awards
- 02/2015 Single Scholarship     **CNU**
- 10/2014 National Grants     **CNU**
- 10/2011 National Scholarship  Third Prize     **HNIT**
- 06/2011 College Students Scientific And Technological Competition  Third Prize     **HNIT**
- 10/2010 National Scholarship  Third Prize    **HNIT**


---
# Project Experience

**03/19 - 06/19    PaicLabelTool    [PATech]**
- Develop a CS backend software for labeling medical images.  The software is so-called PaicLabelTool, which is well designed for labeling multi-modal images, such as thyroid ultrasound image, breast mammography image, and breast ultrasound image, and it's very easy to be extended to other modal images.  PaicLabelTool is based on LabelMe which is an open source software for labeling natural images.  I redefined PaicLabelTool as an MVC software which mainly includes six modules, Tool Bar module, Canvas module, Patient Diagnosis Information module, Annotation List module, File Tree Widget module, and the last one is the AI module.  Doctors can select different tools from the Tool Bar module, such as polygon, rectangle, and curve, to labeling the lesions and a JSON format file with a mask file in PNG format will be saved automatically when doctors click the save button.  I also designed a multi-view canvas for displaying breast mammography image which usually contains L-CC, R-CC, L-MLO, and R-MLO views.  I extracted diagnosis information from pathological diagnosis Excel table and compressed the information into an NPY format file.  Qt-based Multi-Thread Class is designed for loading the compressed NPY file automatically when the software is launched and the corresponding modal is selected.  Doctors can manipulate annotations in the Annotation List module, once annotation in the List module is selected and the corresponding delineation in the canvas is selected automatically.  Doctors can import files from a directory and the files in the directory will be structured according to two conditions which are patient ID and check date.  The AI module which can output AI results automatically, so PaicLabelTool is actually a semi-automatic labeling tool, doctors can make adjustments based on AI results, thus we improved the efficiency of labeling and reduced the heavy manual works.

**01/19 - 02/19    Humpback whale identification    [Kaggle]**
- Humpback whale identification is an Open Set Few-Shot Fine-Grained image classification problem, which includes 
5005 different whale classes. Especially, an unknown class exists, named new class, embedding learning is selected 
to address this extremely hard task. I got Private LB: 0.928, ranking top4%. 


**11/18 - 01/19    Human Protein Atlas Image Classification   [Kaggle]**
- Human Protein Atlas Image Classification is a competition held by Kaggle, which is aim to classify subcellular protein patterns in human cells. It's a multi-label classification problem, that is to say, each subject may have multiple possible labels. ResNet50 is selected as backbone which is customized for fine-grained image classification problem and loss function is well-designed for overcome extremely class imbalance. I got Private LB:0.539, ranking top2%. Please click [Chris J.Liu](https://www.kaggle.com/c/human-protein-atlas-image-classification/leaderboard) for more details.

**01/18 - 03/19    Pulmonary Nodule Type Classification   [Imsight]**
- Pulmonary Nodule Type Classification is a fine-grained image classification problem. The difficulty in this problem is that large intra-class variance and small inter-class variance. For example, the shape and texture of same type of nodule vary largely and different type of nodule display high visual similarity. The discriminative differences only exist in some subtle regions. Model should be well designed with the ability to catch subtle discriminative regions for accurate classification. Pulmonary Nodules can be classified into four classes: Ground-Glass, Part-Solid, Solid, and Calcification. As the most common type, Solid nodules exceed 50% of the whole dataset, thus, there is also a severely class imbalance problem. The well-trained model got an accuracy over 78% on my own test dataset with more than 3000 nodules.

**01/18 - 03/19    Pulmonary Nodule Benign And Malignant Classification   [Imsight]**
- Pulmonary Nodule Benign And Malignant(B&M) Classification is similar with Nodule Type Classification problem, but the B&M labels are more difficult to obtain, even obtained label can't represent the ground truth. We designed mechanisms to make sure the high quality of labels and combined deep learning techniques with the experience of doctors.

**01/18 - 03/18    Nodule Template Matching    [Imsight]**
- Nodule Template Matching algorithm is aim to detect same nodules in different series of follow-up images. I designed an effective, fast and accurate algorithm and implemented it in modern c++ 11. I do extremely optimized the Pipeline with a result that it can predict a pair of nodules within 10ms tested on device with i7-7700k CPU.

**09/17 - 12/17    Lung-Sight    [Imsight]**
- Build a stable CS software called Lung-Sight which is a developed lung cancer screening system with the cutting-edge AI technologies. Lung-Sight is designed for conveniently display the outputs generated by Lung Cancer Engine which can predict pulmonary nodules and analyze features automatically with the power of AI. The Medical Image displaying and manipulation system is based on ITK, VTK and Qt, OpenGL as well, which can communicate fluently with PACS in hospital, thus, we extend the normal image viewer system. It's worth mentioning that I won considerable bonus since I complete this project in such a short time but without compromising its high levels performance and robustness.

**07/17 - 09/17    Blood Vessel Planar Parameterization    [Shenzhen Institutes of Advanced Technology]**
- Blood Vessel Planar Parameterization is a visualization part of Robotic Catheter System for Cardiovascular Interventions which is support by The National High Technology Research and Development Program of China(863 Program). Our algorithm is based on rotation minimizing frame(RMF) which is widely used in computer graphics. Some of our experimental results can be found [here](https://www.flickr.com/photos/164791758@N03/31394806447/in/dateposted/).

**09/16 - 11/16    Text Image Synthesis     [Samsung Electronics (China)R&D Center]**
- Build a fast and scalable engine to generate synthetic images of text in clutter. This engine overlays synthetic text to existing background images in a natural way, accounting for the local 3D scene geometry.Thus, we synthesize large-scale training samples for text localization in natural images.

**05/16 - 09/16    Expression Transfer Based On 3DMM     [Intel Labs China]**
- Reconstruct 3D human face from a single 2D image based on a 3D Morphable Model(3DMM), including estimate head's pose, face shape and face expression coefficients, etc.  With predefined 68 3D Landmarks on 3DMM and detected 68 2D key points of the 2D image, we construct 68 2D-3D landmark pairs and use Gauss-Newton to optimize the distance between 3D-2D points by projecting 3D landmarks on to the 2D image. Once we finish 3D face reconstruction,  face editing like expression variation could be done with the instruction of reconstructed 3D face. For example, a different facial expression could be easily transferred onto the face of the 2D image without losing its identity.

**05/16 - 09/16    Face Frontalisation     [Intel Labs China]**
- “Frontalisation” is the process of synthesizing frontal facing views of faces appearing in single unconstrained photos. Our method produces a new face image of frontal views which can be used for face recognition and other purposes.

**02/16 - 03/16    Automatical Correction Algorithm for Skewed Document Image     [XueXiBao]**
- The idea is to turn your smartphone into a fast, efficient scanner, one that automatically deposits documents, business cards, receipts, books, whiteboards, and other real-world items into a various destination with images automatically adjusted as you move around. 


---

#### Last update: 2019-07-09
