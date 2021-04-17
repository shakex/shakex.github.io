# 探索OCR——在印刷体表格识别的尝试


> 去年年底在做的一个研究课题，有关于OCR财务报表识别。初次探索这个在市面上有不少落地产品的新鲜领域，简单总结一下课题的成果。[[项目地址](https://github.com/shakex/SheetOCR)]


## 摘要

OCR（Optical Character Recognition）光学字符识别，是指将图像上的文字转化为计算机可编辑的文字内容的技术，众多的研究人员对相关的技术研究已久，也有不少成熟的OCR技术和产品产生。本课题以上市公司近5年的财务报表为研究数据，主要探索了OCR在表格印刷体识别方向的一般方法和技术要点。在这一阶段（2020.10—2020.12）的研究和实践过程中，小组主要调研了市面上应用于印刷体识别的一般方法和算法模型，并初步搭建了一套用于财务报表智能识别和分析的系统。本报告针对其中的算法部分做简要介绍，主要涉及课题实现的整体思路、数据增强的技巧、文本检测和识别模型的结构、文本纠错的思路以及在实际实现中的难点和下一阶段的实施方向，作为阶段性的总结和分享。


## 简介

图像文字中包含着丰富的语义信息，OCR（Optical Character Recognition）在金融行业的应用以文档相关的印刷体文本识别为主，如卡证、票据、合同、报表等。OCR作为流程自动化的重要一环，可以极大提升业务流程的效率，减少人工审核、录入的工作量，优化产品服务的客户体验，以及为后续的数据分析与挖掘提供从图像到文本的技术基础。目前市面上的OCR产品主要按照具体的应用场景提供服务接口。从产品和服务角度来看，不同行业、场景下的文本内容往往有其特定的排版标准，按照场景划分更加贴近业务需求，并能够针对性地解决业务痛点；从技术角度来看，早期的OCR主要基于传统图像处理方法，受技术限制其应用场景主要集中在车牌识别、文档扫描、银行卡卡号识别等单一领域。随着近年来以数据驱动的机器学习，尤其是深度学习在图像目标识别、自然语言处理上的研究取得突破进展，OCR在多场景的应用与落地成为可能。在学术界，针对全场景的通用文本检测与识别技术依然是有待研究与提升的方向，其主要面临的挑战有文字多样性（多语种、字体、颜色、大小、方向、形状）、复杂背景（背景图案与纹理、遮挡）、图像质量（拍摄角度、分辨率、噪声、光照）、数据条件（小样本、无监督）等；在工业界，针对具体细分场景的算法优化和与业务系统的融合为主要解决的问题。如下表所示，参考百度OCR产品的场景分类，我们可以看出，目前较为成熟的应用方向主要针对行业相关的印刷文档或制品。而随着学术研究在文本识别和检测的发展，OCR的应用场景和范围将进一步扩大，例如影视文字识别、店铺招牌识别、街景文字识别、通用手写体识别等。


| 产品             | 识别类                                                    |
| ---------------- | --------------------------------------------------------- |
| 卡证识别         | 身份证、银行卡、营业执照、户口本、护照等                  |
| 汽车场景文字识别 | 行驶证、驾驶证、车牌、VIN码、车辆合格证、机动车销售发票等 |
| 财务票据文字识别 | 增值税发票、火车票、行程单、支票、汇票等                  |
| 医疗票据文字识别 | 医疗发票、保险单、医疗费用结算单等                        |
| 教育场景文字识别 | 手写文字、拍照搜题、作业及试卷文字与公示等                |
| 其他场景         | 仪器仪表盘读数、印章、彩票等                              |



目前OCR算法的核心包括文本检测和文本识别两个步骤。文本检测定位图像中的文字位置，一般用矩形框或四边形标记文本所在区域；文本识别根据检测得到的文本图像，由识别模型得到对应的文本信息。为了解决通用文本检测与识别中的挑战，近年来研究人员在各个研究方向都取得了较大的进展。数据方面，无论是检测还是识别模型，识别精度和效果很大程度上依赖于数据规模和数据的多样性——即在使用相同模型的情况下，训练数据越多，数据分布越广，训练的模型就具有更好的识别和泛化性能。因此，大量数据的获取、分类、清洗和标注工作对于最终取得良好的识别效果至关重要。在实际生产中，用于训练一个模型所需的图像数据往往是百万级别的，且需要耗费较多的人力和时间去完成数据标注。在合适的情况下，利用现有的公开数据集是一种可行的解决方案；此外，在很多领域，出于隐私保护、成本控制等原因，并不能获取大量的场景数据，利用数据增广方法可以解决此类样本缺失或不均衡的问题。模型方面，本文检测模型主要分为基于检测框和基于Mask两类，典型算法包括：[CTPN (Tian et al., 2016)](https://link.springer.com/chapter/10.1007/978-3-319-46484-8_4) 、[EAST (Zhou et al., 2017)](https://openaccess.thecvf.com/content_cvpr_2017/papers/Zhou_EAST_An_Efficient_CVPR_2017_paper.pdf)、PMTD、DB等；文本识别模型分为基于CTC（Connectionist Temporal Classification）的模型（[Shi et al., 2017b（CRNN）](https://arxiv.org/pdf/1507.05717.pdf) 、[He et al., 2016（DTRN）](https://arxiv.org/pdf/1506.04395.pdf)、[Gao et al., 2017](https://arxiv.org/abs/1709.04303)、[Yin er al., 2017](https://arxiv.org/abs/1709.01727)等）和基于注意力机制的编码器-解码器结构（[Lee and Osindero, 2016](https://arxiv.org/pdf/1603.03101.pdf)、[Cheng et al., 2018](https://arxiv.org/abs/1711.04226)、[Bai et al., 2018](https://arxiv.org/pdf/1805.03384.pdf)、[Liu et al., 2018d](https://ren-fengbo.lab.asu.edu/sites/default/files/16354-77074-1-pb.pdf)）。此外，目前也有不少针对不规则文本检测和识别的方法（[Wang et al., 2018（ITN）](http://openaccess.thecvf.com/content_cvpr_2018/papers/Wang_Geometry-Aware_Scene_Text_CVPR_2018_paper.pdf)、[Zhang et al., 2019](https://arxiv.org/abs/1904.06535)、[Wang et al., 2019b](https://arxiv.org/abs/1905.05980)）和将检测和识别模型联合训练合并成为端到端网络的工作（Bartz et al., 2017、Xing et al., 2019）。



{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1goyui3g5wcj30860nkta9.jpg" caption="图1 表格印刷体识别的一般流程" width="150" >}}



课题小组从目前较为成熟的文档印刷体识别方向出发，总结文档类OCR算法构建的主要步骤如图1所示：输入一张通过图像采集设备获取的文档图像，一般需要经历以下五个步骤：图像校正、版面分析、文本检测、文本识别和文本纠错，最终得到所需要提取的文字信息。具体如下：

- **图像校正**：主要利用灰度化、二值化、图像平滑、直线检测、边缘检测、透视变化等一些方法对采集的文档可能存在的倾斜、遮挡等情况进行校正。该步骤的一个主要难点在于准确定位图像主体（文档/卡证）的四个角点，并利用角点坐标对主体进行校正。针对复杂背景和角点部分遮挡的情况，需要训练分割或边缘检测模型以获得较好的校正效果。例如，图2展示了对于身份证件照片进行透视变化校正，得到用于文本检测和识别的图像。

  
  
  {{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1gp033or762j31260gi4qp.jpg" caption="图2 身份证图像校正" >}}
  
  

- **版面分析**：版面分析可以对文档内的图像、文本、公式、表格信息和位置关系进行自动分析、识别和理解。对于复杂的文档，根据版面分析的不同区块采用不同的检测与分割模型，可以提高OCR系统的识别准确率。例如，图3展示了对于一篇论文版面结构的划分。

  
  
  {{< image src="https://pic4.zhimg.com/v2-3d0977561c8dcf6b7282f9531576925f_b.jpg" caption="图 3 论文图像的版面分析" >}}
  
  
  
- **文本检测**：文本检测识别并定位文本在图像中的位置。对于规则的印刷体文本，通常以矩形框标注出每个文本目标的边框。方法主要分为基于检测框和基于Mask两类，基于检测框的文本检测，其思路是先利用若干 Anchor 产生大量候选文本框，再经过 NMS 得到最终结果；基于 Mask 的文本检测，其思路是通过分割网络进行像素级别的语义分割，再通过后处理得到文本框，由于后处理比较复杂，这个步骤会直接影响基于 Mask 文本检测算法的性能。

  

- **文本识别**：文本识别将文本检测得到的矩形框作为文本识别模型的输入，通过识别模型得到的该矩形区域内的文本内容。由于输入图像中所包含的文字数量是不确定的，因此文本识别的一个关键是解决输入图像宽度与文本字符长度的对应关系，识别模型分为两类：一类是基于CTC（Connectionist Temporal Classification）的模型，有（[Shi et al., 2017b（CRNN）](https://arxiv.org/pdf/1507.05717.pdf) 、[He et al., 2016（DTRN）](https://arxiv.org/pdf/1506.04395.pdf)、[Gao et al., 2017](https://arxiv.org/abs/1709.04303)、[Yin er al., 2017](https://arxiv.org/abs/1709.01727)等），另一类是基于意力机制的编码器-解码器结构（[Lee and Osindero, 2016](https://arxiv.org/pdf/1603.03101.pdf)、[Cheng et al., 2018](https://arxiv.org/abs/1711.04226)、[Bai et al., 2018](https://arxiv.org/pdf/1805.03384.pdf)、[Liu et al., 2018d](https://ren-fengbo.lab.asu.edu/sites/default/files/16354-77074-1-pb.pdf)）。图4 展示了利用检测和识别算法对于中文表格和英文文本的识别结果。

  

  {{< image src="https://tva1.sinaimg.cn/large/008eGmZEly1gpn0q64wnnj31ic0qyb29.jpg" caption="图4 文本检测与识别示例1" >}}
  
  
  
  {{< image src="https://tva1.sinaimg.cn/large/008eGmZEly1gpn0shrey2j31bg0u0npd.jpg" caption="图4 文本检测与识别示例2" >}}
  
  
  
  
  
- **文本纠错**：对于一些复杂场景和特殊字符，识别模型往往很难达到100%的准确率，因此利用语言作为先验信息，对识别结果进行纠错，可以增加OCR系统的鲁棒性。OCR识别纠错主要涉及文本的词法错误，例如，识别模型将“上海市”识别为“上海币”。纠错的两个关键问题分别为：语言模型和字形的相似度度量。语言模型给出当前识别结果最可能的几个真实值，字形的相似度度量给出真实值识别为当前结果的可能性。

  

当然，上述步骤并不是对于所有场景都是不可或缺，针对具体文档图像依然需要从实际问题出发，选择并设计适合的处理方法和流程。


## 方法要点

课题组实现的OCR任务如下：给定**450**张经过预处理的多家上市公司近五年财报，其中分为利润表、资产负债表和现金流量表三种类型各**150**张，每张图像包含标题、表头信息（证券代码、证券简称、报告日期、单位）和表身信息，要求识别每张图像中的文本内容，将识别结果写入CSV文件中。交互设计页面呈现如图5所示：其中左侧为输入图像的部分截图，右侧为界面显示的识别结果。



{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1goyusnsfbtj317s0hw47m.jpg" caption="图5 财务报表OCR识别及其界面呈现" >}}


由于给定的处理图像质量较好，不存在倾斜、透视等情况，因此省去了上一节所提到的图像校正步骤。针对此场景，我们进行处理的主要步骤如下：

1. 通过基于阈值的图像处理方法对表格中的文本进行检测，并同时确定对应文本所属区域（表头或表身）与键-值对应关系（版面分析+文本检测）

2. 利用CRNN模型对检测文本进行识别（文本识别）

3. 对识别结果进行纠错，主要涉及金额项中逗号和点的纠错（文本纠错）

   

### 版面分析与文本检测

给定图像只包含单张表格，且背景统一为白色。针对此类情况，我们可以直接利用基于阈值的方法划分表头和表身部分，同时裁剪出表身每个单元格内文本。具体步骤如下：

1. 灰度化

2. 形态学算子去除噪点

3. 图像行扫描，确定第一条直线位置，其上为表头信息部分，其下为表格主体部分

4. 表头信息文本检测：行列扫描，根据灰度阈值切割文本

5. 表身信息文本检测：图像行列扫描，确定每一单元格的位置；在每个单元格内，切割出对应文本矩形框

6. 对所有矩形框进行微调：调整宽高大小，确保文本有效信息不丢失。统一矩形框的高度

7. 保存检测框的位置信息，匹配文本的键-值对，记录信息至XML文件

   

检测结果可视化如图6所示。



{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1gp3lw1m1tbj30n80pa76r.jpg" caption="图6 财务报表文本检测结果" >}}





### 文本识别

#### 数据集构建

为了训练适应于当前应用域的文本识别模型，我们首先构建模型训练所需的数据集。构建数据集包括两部分工作：1. 字符集构建；2. 文本短语图像集及标注。其中，字符集的构建，需要收集样本中所用到的所有字符。针对课题所提供的报表图像，我们共统计出中文字符342个，英文+数字字符69个，共计411个字符（当然，为了训练能够认识更多中文字符的模型，需要扩展字符集。通常来说，最常用的中文字符在3000字左右）。对于文本短语数据集的构建，在对450张表格图像进行文本检测以后，得到约21w张短语图像。其中，由于不同财报图像中包含大量重复短语，因此需要对检测进行去重。去重以后图像样本在1.7w张左右，但是，样本中绝大部分为数字样本，中文短语样本仅378张。显然，如果仅使用这些数据构建训练集的话，会存在样本不均衡的问题，即数据分布集中在数字字符类别，而对于大量的中文字符类，数据样本过少，这显然与真实情况的样本分布相差甚远，会造成训练的模型对于数字样本的过拟合，图7显示了在仅利用现有数据进行模型训练（采用CRNN模型）以后的预测结果，可以看到，结果中对于数字的识别较为准确（部分字符仍然出现识别错误，说明对于数字样本而言，数据样本也需要一定程度的增广），但是对于中文的识别准确率很不理想；因此，我们需要对数据进行增广，扩大中文样本的数量，使得每个字符类的分布尽量均衡。



{{< image src="https://tva1.sinaimg.cn/large/008eGmZEly1gpn0glpxupj315s08oq5y.jpg" caption="图 7 数据样本不均衡情况下的模型训练预测结果" >}}



在进行数据增广之前，我们对现有数据的基本情况及分布做了基本统计。图8展示了短语样本长度的分布。对于数字样本，其长度分布主要集中在约15-20个字符之间；对于中文短语样本，其长度分布主要集中在约4-10个字符之间。图9展示了在所有样本中出现频次较高的字符和单词统计。对于0-9的数字字符，字符1-9出现的频次大体相同，字符0出现的频次是字符1-9出现频次的3倍，这大致符合数字作为金额时的分布规律。在关于中文字符的统计中，可以看到，出现频次前三字符为“金”，“资”，“现”，出现频次前三的单词为“现金”，“其他”，“资产”。由此可见，针对财务报表的场景，在所构建的数据集中适当提高财会类字符或词汇的占比，可以更加贴近数据的真实分布。



{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1goz003edklj30uq0audhi.jpg" caption="图 8 数据样本长度分布统计" >}}



{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1goz00uw5rgj319s0is42k.jpg" caption="图 9 数据样本高频字符和单词统计" >}}



接下来考虑文本数据的增广方法，数据增广的操作可以分为两大类：一类是对现有数据进行图像增强，如旋转、拉伸、对比度变化、模糊、膨胀、腐蚀等操作，这类操作可以在维持数据现有分布的情况下，提高模型的泛化性和鲁棒性；另一类是通过文本生成算法去产生新的文本图像，这类操作调整数据更加趋近真实场景的数据分布。在课题组的研究过程中，我们尝试了论文“EDA: Easy Data Augmentation Techniques for Boosting Performance on Text Classification Tasks”所提出的数据生成方法，并在此基础上做了适当修改。该方法针对给定短语文本，可以通过四种基本操作（随机插入、随机删除、随机替换、随机交换）产生新的短语文本，具体操作如下：

- **随机插入（Random Insertion, RI）**:在每个短语的单词间隙中随机插入n个新单词（短语长度增加）

- **随机删除（Random Deletion, RD）**：以概率p随机删除短语中所包含的单词（短语长度减少）

- **随机替换（Random Replacement, RR）**：用一个新单词替代短语中的任意一个单词，重复n次（短语长度几乎不变）

- **随机交换（Random Swap, RS）**：随机选取短语中的两个单词进行交换，重复n次（短语长度不变）

  

例如，对于“一年内到期的非流动资产”这个文本短语，利用EDA随机得到的文本如下：

|       | RI                             | RD       | RR                         | RS                     |
| ----- | ------------------------------ | -------- | -------------------------- | ---------------------- |
| 示例1 | 一年内到期税费的非流动资产     | 非流动   | 一年内川能动力的非流动资产 | 一年内资产的非流动到期 |
| 示例2 | 代码一年内到期的汇率非流动资产 | 一年内的 | 一年内其他的非流动资产     | 到期一年内的非流动资产 |



最后，我们对所有的数据样本进行文本标注，并划分训练和验证集合。由于手工标注工作量比较大，我们选择直接利用市面上的API做文本识别，得到标注结果，再针对一些标注错误的情况进行修改。对于数据集的划分，我们按照7:3的比例随机划分训练和验证集合。




#### CRNN模型

在完成数据集的构建后，我们着手探究适用于印刷体文本识别的模型构建。在这一阶段，课题组尝试了运用最广泛的CRNN模型，图11展示了该模型的结构。CRNN模型主要分为三部分：1. **卷积网络层**。主要使用CNN对输入图像进行特征提取，得到特征图；2. **循环网络层**。主要利用RNN（BiLSTM）对特征序列进行预测，对序列中的每个特征向量进行学习，并输出预测标签（真实值）分布；3. **转录层**。主要利用CTC解决针对不同输入长度的文本字符对其问题，使用CTC损失把循环网络层获取的一系列标签分布转换成最终的标签序列。模型技术细节可参考[CRNN论文](https://arxiv.org/pdf/1507.05717.pdf)。

{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1goz0lyzrv4j30g50jh76q.jpg" caption="图 10 CRNN模型结构"  width="400">}}




### 文本纠错

通过识别模型的得到的预测结果往往不能达到100%的识别准确率，在文本纠错步骤中可以结合一些先验知识对识别结果进行改进：

- 针对表格中的金额，“,”和“.”字符比较接近，识别结果容易预测出错，可以金额中逗号出现的规则设计算法进行纠正；

- 预测文本中的括号缺失问题，可通过括号匹配算法进行纠正；

- 对预测文本中的日期、金额、公司名、证券代码等关键字段设计算法进行校验，对于不合法文本，采用多模型预测融合的策略进行纠正；

- 其他词法错误，通过训练语言模型进行纠正。

  

基于以上步骤，课题小组在现阶段实现对于财务报表的识别结果如图11所示。



{{< image src="https://tva1.sinaimg.cn/large/008eGmZEgy1gpmkzp6wrhj30u00unanp.jpg" caption="图 11 利润表的OCR识别结果" >}}





## 下阶段工作展望

针对课题组在OCR算法方面的工作，下阶段主要的改进有：

- 数据方面，经过EDA数据增广后中文数据的样本量有了很大提升，但距离实际生产所需的数据量（百万级）还差很远。课题组还将尝试采用引入外部数据集与多种数据增广方法相结合的方式，建立适用于金融领域的文本识别数据库。

- 模型方面，现阶段课题组验证了CRNN模型在文本识别上的可行性，后续将测试并改进更多文本识别模型，构建模型库，并基于文本数据集完成模型训练和调优，提供可供系统调用的统一接口。

- 部署方面，现阶段的部署方案采用客户端与服务端共享文件夹，OCR程序后台轮询文件夹的方式实现。在下一阶段，将实现通过http请求方式调用服务。

  


## 参考文献

**综合**

- Long, Shangbang, Xin He and Cong Yao. “Scene Text Detection and Recognition: The Deep Learning Era.” International Journal of Computer Vision 129 (2020): 161-184.
- Du, Yuning, Chenxia Li, Ruoyu Guo, X. Yin, Weiwei Liu, Jun Zhou, Y. Bai, Z. Yu, Y. Yang, Qingqing Dang and Haoshuang Wang. “PP-OCR: A Practical Ultra Lightweight OCR System.” ArXiv abs/2009.09941 (2020): n. pag.

**表格识别**

- Li, Y., Z. Huang, Junchi Yan, Y. Zhou, Fan Ye and Xianhui Liu. “GFTE: Graph-based Financial Table Extraction.” ICPR Workshops (2020).
- Paliwal, Shubham, D. Vishwanath, R. Rahul, M. Sharma and L. Vig. “TableNet: Deep Learning Model for End-to-end Table Detection and Tabular Data Extraction from Scanned Document Images.” 2019 International Conference on Document Analysis and Recognition (ICDAR) (2019): 128-133.
- Khan, Saqib Ali, Syed Khalid, M. Shahzad and F. Shafait. “Table Structure Extraction with Bi-Directional Gated Recurrent Unit Networks.” 2019 International Conference on Document Analysis and Recognition (ICDAR) (2019): 1366-1371.

**图像校正（边缘检测）**

- Xie, Saining and Zhuowen Tu. “Holistically-Nested Edge Detection.” International Journal of Computer Vision 125 (2015): 3-18.

**版面分析**

- Viana, Matheus Palhares and D. Oliveira. “Fast CNN-Based Document Layout Analysis.” 2017 IEEE International Conference on Computer Vision Workshops (ICCVW) (2017): 1173-1180.
- Xu, Yiheng, Minghao Li, Lei Cui, Shaohan Huang, Furu Wei and M. Zhou. “LayoutLM: Pre-training of Text and Layout for Document Image Understanding.” Proceedings of the 26th ACM SIGKDD International Conference on Knowledge Discovery & Data Mining(2020): n. pag.
- Zhong, Xu, J. Tang and Antonio Jimeno-Yepes. “PubLayNet: Largest Dataset Ever for Document Layout Analysis.” 2019 International Conference on Document Analysis and Recognition (ICDAR) (2019): 1015-1022.

**文本检测**

- Zhou, X., C. Yao, H. Wen, Yuzhi Wang, Shuchang Zhou, Weiran He and Jiajun Liang. “EAST: An Efficient and Accurate Scene Text Detector.” 2017 IEEE Conference on Computer Vision and Pattern Recognition (CVPR) (2017): 2642-2651.
- Liao, Minghui, Zhaoyi Wan, C. Yao, Kai Chen and X. Bai. “Real-time Scene Text Detection with Differentiable Binarization.” AAAI (2020).
- Tian, Zhi, Weilin Huang, Tong He, Pan He and Yu Qiao. “Detecting Text in Natural Image with Connectionist Text Proposal Network.” ECCV (2016).

**文本识别**

- Shi, Baoguang, X. Bai and Cong Yao. “An End-to-End Trainable Neural Network for Image-Based Sequence Recognition and Its Application to Scene Text Recognition.” IEEE Transactions on Pattern Analysis and Machine Intelligence 39 (2017): 2298-2304.
- Lee, Chen-Yu and Simon Osindero. “Recursive Recurrent Nets with Attention Modeling for OCR in the Wild.” 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR) (2016): 2231-2239.

**文本检测+识别端到端**

- Bartz, C., Haojin Yang and C. Meinel. “SEE: Towards Semi-Supervised End-to-End Scene Text Recognition.” AAAI (2018).
- Busta, M., Lukás Neumann and Jiri Matas. “Deep TextSpotter: An End-to-End Trainable Scene Text Localization and Recognition Framework.” 2017 IEEE International Conference on Computer Vision (ICCV) (2017): 2223-2231.
- Xing, Linjie, Zeyong Tian, Weilin Huang and M. Scott. “Convolutional Character Networks.” 2019 IEEE/CVF International Conference on Computer Vision (ICCV) (2019): 9125-9135.

**文本纠错**

- Yu, Junjie and Zhenghua Li. “Chinese Spelling Error Detection and Correction Based on Language Model, Pronunciation, and Shape.” CIPS-SIGHAN (2014).

**数据处理**

- Wei, Jason and K. Zou. “EDA: Easy Data Augmentation Techniques for Boosting Performance on Text Classification Tasks.” ArXiv abs/1901.11196 (2019): n. pag.

**开源软件**

- 国内OCR服务商: [百度](https://ai.baidu.com/tech/ocr?track=cp:ainsem|pf:pc|pp:chanpin-wenzishibie|pu:wenzishibie-baiduocr|ci:|kw:10002846)/[腾讯](https://cloud.tencent.com/act/event/ocrsale?fromSource=gwzcw.4251956.4251956.4251956&utm_medium=cpc&utm_id=gwzcw.4251956.4251956.4251956)/[阿里巴巴](https://ai.aliyun.com/ocr?spm=5176.182739.artificialIntelligence.20.69111d8aNKn80V)/[第四范式](https://www.4paradigm.com/operate/smart-archive)/[华为](https://support.huaweicloud.com/ocr/index.html)/[旷视](https://www.faceplusplus.com.cn/general-text-recognition/)/[商汤](https://www.sensetime.com/cn/product-detail?categoryId=32132)/[科大讯飞](https://www.xfyun.cn/service/textRecg?ch=bd17-08&bd_vid=10414311197403882885)/[合合信息](https://www.intsig.com/public/ai_new/card.shtml)/[易道博识](http://ai.exocr.com)/[汉王数字](http://www.hw-ai.com/index.php?m=content&c=index&a=lists&catid=23)/[有道智云](https://ai.youdao.com/product-ocr-print.s)
- 开源OCR：[JaidedAI](https://github.com/JaidedAI)/**[EasyOCR](https://github.com/JaidedAI/EasyOCR)**, [PaddlePaddle](https://github.com/PaddlePaddle)/**[PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)**, [tesseract-ocr](https://github.com/tesseract-ocr)/**[tesseract](https://github.com/tesseract-ocr/tesseract)**, [chineseocr](https://github.com/chineseocr)/**[chineseocr](https://github.com/chineseocr/chineseocr)**, [DayBreak-u](https://github.com/DayBreak-u)/**[chineseocr_lite](https://github.com/DayBreak-u/chineseocr_lite)**, [OCR.space](https://ocr.space/OCRAPI), [AvensLab](https://github.com/AvensLab)/**[OcrKing](https://github.com/AvensLab/OcrKing)**, [open-mmlab](https://github.com/open-mmlab)/**[mmocr](https://github.com/open-mmlab/mmocr)**
- 数据增广：[oh-my-ocr](https://github.com/oh-my-ocr)/**[text_renderer](https://github.com/oh-my-ocr/text_renderer)**, [jasonwei20](https://github.com/jasonwei20)/**[eda_nlp](https://github.com/jasonwei20/eda_nlp)**
- 文本纠错：[shibing624](https://github.com/shibing624)/**[pycorrector](https://github.com/shibing624/pycorrector)**, [iqiyi](https://github.com/iqiyi)/**[FASPell](https://github.com/iqiyi/FASPell)**


