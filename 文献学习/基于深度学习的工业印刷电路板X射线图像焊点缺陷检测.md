# 基于深度学习的工业印刷电路板X射线图像焊点缺陷检测



### 摘要

​		随着电子电路生产方法的改进，例如元件尺寸的减小和元件密度的增加，生产线中出现缺陷的风险越来越大。检查失效焊点的技术有很多，如X射线成像、光学成像和热成像，其中X射线成像可以检查外部和内部缺陷。然而，一些先进的算法不够精确，无法满足质量控制的要求。需要大量的手动检查，这增加了专家的工作量。此外，自动X射线检查可能会产生不正确的感兴趣区域，从而恶化缺陷检测。X射线图像的高维性和图像大小的变化也对检测算法提出了挑战。最近，深度学习的最新进展为基于图像的任务提供了灵感，并与人类水平相竞争。在这项工作中，在质量控制检查中引入了深度学习。提出并比较了四种基于人工智能的联合缺陷检测模型。讨论了噪声ROI和图像尺寸变化问题。通过在真实三维X射线数据集上的实验验证了所提出模型的有效性，大大节省了专家检查工作量。

​					**关键词**：  联合缺陷检测      深度学习      自动X射线检测        质量控制



### 引言

​			随着电子电路生产方法的改进，手机和笔记本电脑等电子产品的生产速度也在提高。同时，在印刷电路板（PCB）装配过程中，由于元件尺寸的减小和元件密度的增加，缺陷的风险增加^[1]^。因此，高效准确的质量控制系统至关重要。

​		结构缺陷是组装电路板中的主要缺陷类型之一，包括焊料不足、空洞和短路^[2]^。借助自动检测方法，减少了人工工作量，减轻了专家主观判断和专家疲劳带来的错误。基于成像的自动检测方法，如光学成像和热成像，通常用于生产线的质量控制^[3–6]^。

​		与光学和热成像相比，X射线成像不仅能更好地反映外部缺陷，而且能更好地反映内部缺陷。简单地说，焊料使用重材料，如银、铜和铅，在X射线下很容易成像，而形成其他组件的轻材料不容易成像。重材料的数量也可以从X射线成像中反映出来。因此，可以在外部和内部的X射线图像上检查焊料质量。因此，**自动X射线检测（AXI(automated X-ray inspection)）**已广泛应用于电子制造业，以监测印刷电路板的质量。例如，可以在2D空间中对X射线图像特征进行建模和可视化，以进行缺陷检测。检测基于可视化缺陷关节特征与正常关节的偏差^[7]^。X射线图像也可以用于三维空间，以形成空间模型。通过比较空间特征，如体积和传播方向，可以过滤出缺陷焊点^[8]^。然而，这种计算机断层扫描图像需要很高的计算复杂度，并且需要时间来检查一个样本。

​		近年来，深度卷积神经网络（CNN）在图像分类和目标检测等许多领域显示出了具有竞争力的性能^[9-11]^。CNN可以有效地提取基于图像的特征。特征提取对于基于成像的方法非常重要，无论是光学成像、热成像还是X射线成像。在焊点检测领域，一些研究侧重于提取准确的特征，并提出了创新的光学图像提取方法，如高等人提出的基于线的聚类方法^[4]^。以及金等人提出的参考自由路径行走方法^[5]^。在基于CNN的方法方面，吴等人提出使用掩码R-CNN方法进行缺陷联合定位和检测^[12]^。Cai等人提议使用三个级联级别的CNN来检测焊点缺陷。随着级别数的增加，性能会变得更好^[13]^。然而，更复杂的级别需要更多的手动标记，并增加了手动工作量。Goto等人提出将对抗卷积自动编码器与传统异常检测方法相结合用于缺陷检测^[14]^。他们的方法在具有八个成像深度的X射线数据集上表现良好。但一些实际问题没有得到解决，例如切片数量变化和X射线图像噪声。在本文中，我们关注缺陷检测问题，以减少手动工作量。在检测过程中解决了实际问题。具体问题公式将在下一节中说明。



### 问题公式

​		在AXI中，先进技术用于焊点定位和检查。当机器检测到缺陷焊点时，会将其发送给专家进行第二轮检查。然而，有时机器检查不准确，这会将大量正常的焊点发送给专家，从而增加手动工作量。研究中很少讨论商品化的AXI机器统计。我们根据获得的真实数据集计算统计数据。**在518292张由AXI机器标记为缺陷的通孔焊接图像中，只有大约15%是由专家标记的真正缺陷。大约85%的焊点是正常的，但也被送往专家进行进一步检查。在这项工作中，主要问题是从错误分类的数据集中检测真实的缺陷焊点，并减少手动工作量。**

​		为了实现这一目标，需要解决两个问题，即**噪声感兴趣区域（ROI( noisy region of interest)）**和可变切片数。目前基于X射线成像的焊料缺陷检测研究很少涉及X射线成像数据集中的噪声。AXI提取的一些ROI不正确，无法包围焊点区域，这会降低焊点缺陷检测。此外，切片的数量也不尽相同。在数据收集过程中忽略了焊料的某些深度，因为这些深度可能不需要人工检查。因此，并非所有焊点的切片数都相同。尽管有一些基于深度学习的方法实现了上述PCB缺陷检测，但很少有研究专注于X射线成像和处理不同数量的切片。一般来说，这些问题可以总结如下：

1、AXI错误呼叫增加了专家工作量。

2、来自AXI的不正确ROI恶化了焊接缺陷检测。

3、切片数量的变化使建模变得困难。

​		在这项工作中，由于深度学习方法适用于高分辨率的基于图像的任务，因此将其纳入其中。我们通过在生产线上提出基于深度学习的检查模块来缓解AXI机器效率低下的问题。三维X射线图像有两种查看方式，这决定了特征提取的方式。第一种方法是分别查看每个切片，并直接从每个切片中提取特征。第二种方法是从所有切片中提取特征，并根据提取的特征进行缺陷检测。

​		提出了两种预处理方法。第一种方法是直接堆叠焊点的每个薄片。第二种方法是将焊点的所有切片处理在一起，并以统计方式对其进行操作。专门设计的预处理方法可以解决不同切片数的问题和不正确的ROI问题。

​		















### 参考文献

1. Liukkonen M, Havia E, Hiltunen Y (2012) Computational intelligence in mass soldering of electronics-a survey. Expert Syst Appl 39(10):9928–9937

2.  Leinbach G, Oresjo S (2001) The why, where, what, how, and when of automated x-ray inspection. Agilent Technologies, Loveland, Colorado

3. Huang CY , Hong JH, Huang E (2019) Developing a machine vision inspection system for electronics failure analysis. IEEE Trans Compon Packag Manuf Technol 9(9):1912–1925       

   黄，洪建华，黄E（2019）开发了一种用于电子故障分析的机器视觉检测系统。IEEE Trans Compon Package Manuf Technol 9（9）：1912–1925

4. Gao H, Jin W, Y ang X, Kaynak O (2016) A line-based-clustering approach for ball grid array component inspection in surface-mount technology. IEEE Trans Ind Electron 64(4):3030–3038

   高H，金W，杨X，Kaynak O（2016）表面贴装技术中球栅阵列元件检测的基于线的聚类方法。IEEE Trans-Ind Electron 64（4）：3030–3038

5. Jin W, Lin W, Y ang X, Gao H (2017) Reference-free path-walking method for ball grid array inspection in surface mounting machines.IEEE Trans Ind Electron 64(8):6310–6318

   金，林，杨，高（2017）表面贴装机球栅阵列检测的参考自由路径行走法。IEEE Trans-Ind Electron 64（8）：6310–6318

6. Lu X, He Z, Su L, Fan M, Liu F, Liao G, Shi T (2018) Detection of micro solder balls using active thermography technology and k-means algorithm. IEEE Trans Ind Inf 14(12):5620–5628

   陆，何，苏，范，刘，廖，施（2018）利用主动热成像技术和k-means算法检测微焊球。IEEE Trans-Ind-Inf 14（12）：5620–56287.

7. Jewler SJ (2019) High resolution automatic X-Ray inspection for continuous monitoring of advanced package assembly. In: 2019 International wafer level packaging conference (IWLPC), pp 1–5.

   https://doi.org/10.23919/IWLPC.2019.8914113

   Jewler SJ（2019）用于连续监测先进封装组件的高分辨率自动X射线检测。2019年国际晶圆级封装会议（IWLPC），第1-5页。

8. Y ung LC (2018), Investigation of the solder void defect in IC semiconductor packaging by 3D computed tomography analysis.

   In: 2018 IEEE 20th Electronics packaging technology conference (EPTC), 2018, pp 886–889. https://doi.org/10.1109/EPTC.2018.

   8654370

   Y ung LC（2018），通过三维计算机断层扫描分析研究集成电路半导体封装中的焊料空洞缺陷。摘自：2018年IEEE第20届电子封装技术会议（EPTC），2018年，第886–889页。

9. 

