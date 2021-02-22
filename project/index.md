# 项目


[404]({{< ref "404/index.md" >}})


---
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

</br>





