# 项目


<br><br>

<!-- 诶！该页面正在**施工中**。回{{< link href="javascript:history.back()" content=上一页 >}}去看看吧。

Oops! This page is still **under construction**. Sorry about that. Go {{< link href="javascript:history.back()" content=Back >}} and start over. -->



## OCR system for financial sheets [Oct. 2020 – Dec. 2020]

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfe1j7379j315x0kbtgt.jpg)

The project built an OCR system for financial sheets analysis. My main contribution: 1. Train and deploy a text recognition model based on CRNN; 2. Adopt an data augmentation strategy inspired by the paper EDA to handle the small sample size problem. The avg. time for text detection and recognition for an image (1000x2000px) is within 5s on CPU and the recognition accuracy over ~10,500 test samples is above 95%.	

---
	
## ID-card rectification [Jun. 2020 – Jul. 2020]

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfdlcc741j30qv0d60zh.jpg)

The project realized an image rectification API for ID-card images in natural scenes. It mainly utilized an edge detection network for card contour detection and opencv interface for perspective transformation. The API provide single and batch image processing services and currently serves for RightPrint Mini Program. 

[[code](https://github.com/shakex/Card-Rectification)]

---

## Fabric defect detection [Jun. 2019 – Aug. 2019]

An algorithm developed for fabric defect detection. The algorithm goes through the following step: 1. Image preprocessing (crop, resize, remove uneven illumination, and remove moire patterns); 2. Surface defect detection (OTSU threshold, morphological transformation); 3. Linear defect detection (Canny edge detection, Hough line detection); 4. Defect feature extraction and classification; 5. Display and generate for XML output.	

---
	
## Predicting radiotherapy sensitivity of laryngeal cancer based on DNNs [Jul. 2019 – Oct. 2019]

In this study, we enrolled 200 patients with laryngeal cancer (LC) who underwent standard radiotherapy alone. Patients were followed up and were classified into radiotherapy-sensitive (LC-RS) and radiotherapy-tolerant (LC-RT) groups according to their prognosis. I took the responsibility of modeling a convolutional neural network based on GoogLeNet, VGG16 and ResNet50 to predict the sensitivity of patients with radiotherapy, and combined the clinical features such as EBV, tumor markers, etc. to compare the imaging difference between LC-RS and LC-RT. Experimental results showed that our model reached 74.5% prediction accuracy among 55 patients of CT scans.	

---
	
## Medical MRI Segmentation [Sep. 2018 – Jun. 2020]

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnbvdvp586j32i90rqwps.jpg)

**Work#1 (ICIP-19)**: A LSTM method with Multi-modality and Adjacency constraint is proposed for segmenting three tissues in Brain MRI. Two feature sequence generation ways in the method are used, i.e., features with pixel-wise and superpixel-wise adjacency constraint. The method is more robust than clustering-based methods like FCM, K-Means, and performs better than other feature classifiers like SVM and KNN, achieving 98.66% Dice Coefficient on the BrainWeb dataset. 

[[pdf](https://ieeexplore.ieee.org/document/8802959)] [[code](https://github.com/shakex/MR-Brain-Tissue-Segmentation)]

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnbvclbi85j30om098gnl.jpg)

**Work#2 (AAAI-20)**: It proposes a Recurrent Decoding Cell (RDC) for hierarchical feature fusion in the encoder-decoder segmentation networks. RDC leverages the ability of convolutional RNNs in memorizing long-term context information. The RDC-based segmentation network achieves 99.34% Dice Coefficient on the BrainWeb dataset, which is better than FCN, SegNet and U-Net, and is robust to image noise and intensity non-uniformity in medical MRI. 

[[pdf](https://arxiv.org/pdf/1911.09401.pdf)] [[code](https://github.com/shakex/Recurrent-Decoding-Cell)]

---

## My spirit (Unity Games) [Dec. 2015 – May. 2016]

<!-- ![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfcmfozndj30d60d7js0.jpg) -->

My Spirit is a 2D hand-drawn-style adventure game. It tells the story about the legendary life of Jerry from the spirit world to the real world. Players use keyboard or Xbox controller to control the characters. We use Unity as the game engine and C# as the programming language. The game is produced by our team EBM, I took charge of the character/UI design, shooting and part of the algorithm realization in this project. 

[[doc](https://github.com/shakex/My-Spirit)] [[demo video](https://www.bilibili.com/video/av44827264?zw)]

{{< admonition type=info title="Screenshot" open=false >}}

![序章：1948——灵界（截图）](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfcsugjjlj311y0lcdpl.jpg)

![第一章：1949——丛林（截图）](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfcvpk4ynj30zk0k0kch.jpg)

![第二章：1974——工厂（截图）](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfct2zoynj311y0lc4qp.jpg)

{{< /admonition >}}

---

## FarmKit [Dec. 2014 – May. 2015]

<!-- ![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfciopylgj30c00iedgi.jpg) -->

FarmKit is a software and hardware framework made specially for agricultural research. It consists of three parts: Intelligent Agricultural Control Terminal, Robot Surveillance and Control System and the Greenhouse Control System. I tested the performance of the robot in greenhouse and collected environment data. This project competed in Imagine Cup 2015 World Citizenship Group and won the second prize in national final. 

[[doc](https://github.com/shakex/FarmKit)]
	


<br><br>





<!-- [404]({{< ref "404/index.md" >}}) -->


<!-- ---
## OCR财务报表识别 {{< version 210207 changed >}}


![OCR财务报表识别](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfe1j7379j315x0kbtgt.jpg)



---
## 身份证图像校正 {{< version 210207 changed >}}

![Picture1](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfdlcc741j30qv0d60zh.jpg)

<p align="justify">项目实现了一种在自然场景下拍摄的身份证件照片的透视变化校正。算法层采用了基于传统 Canny 边缘检测与深度边缘检测⺴ 络相结合的方法提高卡片校正的精度和速度，在复杂背景下卡片的校正成功率在 75%以上，目前该算法服务于 RightPrint 立印 微信小程序提供身份证图像校正接口。</p>

{{< link href="https://github.com/shakex/Card-Rectification" content=[Code] >}}

---

## LSTM-MA: A LSTM Method with Multi-modality and Adjacency Constraint for Brain Image Segmentation {{< version 210207 changed >}}
`research`

![pipeline](https://tva1.sinaimg.cn/large/008eGmZEgy1gnbvdvp586j32i90rqwps.jpg)

<p align="justify"><b>Abstract:</b> MR brain tissue segmentation is a significant problem in biomedical image processing. Inhomogeneous intensity and image noise influence the segmentation accuracy. In this paper, we propose a LSTM method with multi-modality and adjacency constraint for brain image segmentation, named LSTM-MA. Two feature sequence generation ways in our method are used, i.e., features with pixel-wise and superpixel-wise adjacency constraint. The LSTM model classifies the generated features into semantic labels to form the segmentation result. The evaluation experiments on BrainWeb and MRBrainS demonstrate that the proposed LSTM-MA with pixel-wise adjacency constraint achieves promising segmentation results, while LSTM-MA with superpixel-wise adjacency constraint shows its computational efficiency as well as robustness to noise.</p>

{{< link href="https://ieeexplore.ieee.org/document/8802959?denied=" content=[PDF] >}}
{{< link href="https://xueshu.baidu.com/u/citation?&url=http%3A%2F%2Fwww.researchgate.net%2Fpublication%2F335538447_LSTM-MA_A_LSTM_Method_with_Multi-Modality_and_Adjacency_Constraint_for_Brain_Image_Segmentation&sign=da8db13c42d4803c0664ddd12371676c&diversion=null&paperid=105k0a70jw0s0880pu7f0ee0jv511571&allversion=%5B%22http%3A%2F%2Fwww.researchgate.net%2Fpublication%2F335538447_LSTM-MA_A_LSTM_Method_with_Multi-Modality_and_Adjacency_Constraint_for_Brain_Image_Segmentation%22%2C%22http%3A%2F%2Fieeexplore.ieee.org%2Fdocument%2F8802959%2F%22%5D&t=bib" content=[Bib] >}}
{{< link href="https://github.com/shakex/MR-Brain-Tissue-Segmentation" content=[Code] >}}

---
## Segmenting Medical MRI via Recurrent Decoding Cell[^1] {{< version 210207 changed >}}
`research`

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnbvclbi85j30om098gnl.jpg)

<p align="justify"><b>Abstract:</b> The encoder-decoder networks are commonly used in medical image segmentation due to their remarkable performance in hierarchical feature fusion. However, the expanding path for feature decoding and spatial recovery does not consider the long-term dependency when fusing feature maps from different layers, and the universal encoder-decoder network does not make full use of the multi-modality information to improve the network robustness especially for segmenting medical MRI. In this paper, we propose a novel feature fusion unit called Recurrent Decoding Cell (RDC) which leverages convolutional RNNs to memorize the long-term context information from the previous layers in the decoding phase. An encoder-decoder network, named Convolutional Recurrent Decoding Network (CRDN), is also proposed based on RDC for segmenting multi-modality medical MRI. CRDN adopts CNN backbone to encode image features and decode them hierarchically through a chain of RDCs to obtain the final high-resolution score map. The evaluation experiments on BrainWeb, MRBrainS and HVSMR datasets demonstrate that the introduction of RDC effectively improves the segmentation accuracy as well as reduces the model size, and the proposed CRDN owns its robustness to image noise and intensity non-uniformity in medical MRI.</p>

{{< link href="https://ojs.aaai.org//index.php/AAAI/article/view/6932" content=[PDF] >}}
{{< link href="https://arxiv.org/abs/1911.09401" content=[Bib] >}}
{{< link href="https://github.com/shakex/Recurrent-Decoding-Cell" content=[Code] >}}


---
## 基于深度神经网络的喉癌放疗敏感性预测模型

{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1gnbvfs00jej306v06x0ty.jpg" caption="fig"  >}}

<p align="justify">在这项研究中，我们入组了200名接受单纯放疗标准方案的喉癌患者，随访它们的预后情况并根据预后分为放疗敏感和放疗耐受两组。我在其中负责建立基于GoogLeNet, VGG16和ResNet50的卷积神经网络模型预测患者的放疗敏感性，并结合临床特征如EBV、肿瘤标志物等比较放疗敏感和放疗耐受患者CT影像上的差异。实验显示，利用该模型对55名患者CT扫描图像进行放疗敏感性预测可以达到74.5%的准确率。</p>



---
## 布匹瑕疵检测



---
## My Spirit[^2]

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfcmfozndj30d60d7js0.jpg)



{{< admonition type=info title="游戏截图" open=false >}}

![序章：1948——灵界（截图）](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfcsugjjlj311y0lcdpl.jpg)

![第一章：1949——丛林（截图）](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfcvpk4ynj30zk0k0kch.jpg)

![第二章：1974——工厂（截图）](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfct2zoynj311y0lc4qp.jpg)

{{< /admonition >}}


---
## FarmKit（智能农业开发套件）

![](https://tva1.sinaimg.cn/large/008eGmZEgy1gnfciopylgj30c00iedgi.jpg)


<p align="justify">在这项研究中，我们入组了200名接受单纯放疗标准方案的喉癌患者，随访它们的预后情况并根据预后分为放疗敏感和放疗耐受两组。我在其中负责建立基于GoogLeNet, VGG16和ResNet50的卷积神经网络模型预测患者的放疗敏感性，并结合临床特征如EBV、肿瘤标志物等比较放疗敏感和放疗耐受患者CT影像上的差异。实验显示，利用该模型对55名患者CT扫描图像进行放疗敏感性预测可以达到74.5%的准确率。</p>









---




[^1]: Wen, Y., Xie, K., & He, L. (2020). Segmenting Medical MRI via Recurrent Decoding Cell. Proceedings of the AAAI Conference on Artificial Intelligence, 34(07), 12452-12459. https://doi.org/10.1609/aaai.v34i07.6932
[^2]:

</br> -->

