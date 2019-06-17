# Face-Recognition
Practice of two Pattern Recognition methods. Face Recognition based on SVM and SRC.   

#一 背景

##1.1 支持向量机简介

支持向量机（Support Vector Machine，SVM）是AT&TBell 实验室的V.Vapnik等人提出的一种机器学习算法，是迄今为止最重要的机器学习理论和方法之一，也是应用最广泛、综合效果最好的模式分类技术之一。到目前为止，支持向量机已应用于孤立手写字符识别、网页或文本自动分类、说话人识别、人脸检测、性别分类、计算机入侵检测、基因分类、遥感图象分析、目标识别、函数回归、估计、函数逼近、密度估计、时间序列预测及数据压缩、文本过滤、数据挖掘、非线性系统控制等各个领域的实际问题中。

支持向量机是一种二分类模型，其基本定义是特征空间上的间隔最大的线性分类器（当采用线性核时），即支持向量机的学习策略是间隔最大化，最终转化为凸二次规划问题的求解。该方法在1995年正式发表，因其在文本分类任务中显示出卓越性能，很快成为机器学习的主流技术，掀起了“统计学习”在2000年前后的高潮。但实际上，支持向量的概念早在二十世纪六十年代就已出现，统计学习理论在七十年代就已成型。对核函数的研究更早，Mercer定理可追溯到1909年，RKHS则在四十年代就已被研究，但在统计学习兴起之后，核技巧才真正成为机器学习的通用基本技术。

SVM的主要思想是针对两类分类问题，寻找一个超平面作为两类训练样本点的分割，以保证最小的分类错误率。在线性可分的情况下，存在一个或多个超平面使得训练样本完全分开，SVM的目标是找到其中的最优超平面，最优超平面是使得每一类数据与超平面距离最近的向量与超平面之间的距离最大的这样的平面，超平面W是h值最大的最优超平面；对于线性不可分的情况，通过使用核函数（一种非线性映射算法）将低维输入空间线性不可分的样本转化为高维特征空间使其线性可分。支持向量机的求解通常是借助于凸优化技术。

对于多分类任务要进行专门的推广，对带结构输出的任务也已有相应的算法。

核函数直接决定了支持向量机与核方法的最终性能，但核函数的选择仍是一个未决的问题。多核学习使用多个核函数并通过学习获得其最优凸组合作为最终的核函数，实际上借助集成学习机制。目前，SVM已有很多软件包，比较有名的是LIBSVM[Chang and Lin,2011]和LIBNEAR[Fan et al, 2008]。

##1.2 稀疏表示理论简介

信号稀疏表示是过去近20年来信号处理界非常引人关注的领域。稀疏表示的目的就是在给定的超完备字典中用尽可能少的原子来表示信号，可以获得信号更为简洁的表达方式，从而使我们更容易地获取信号中所蕴含的信息，更方便进一步对信号进行加工处理，如压缩、编码等。信号稀疏表示的研究热点主要集中在稀疏分解算法、超完备原子字典和稀疏表示的应用方面。其两大主要任务是字典的生成和信号的稀疏分解，对于字典的选择一般有分析字典和学习字典两大类。

目前，稀疏表示的应用范围基本为自然信号形成的图像、音频以及文本等， 对于非自然信号或数据的应用尚未有文献涉及。在应用方面，可大体划分为两类。（1）基于重构的应用。此类应用有图像去噪、压缩与超分辨、SAR成像、缺失图像重构以及音频修复等。这些应用主要将目标的特征用若干参数来表示，这些特征构成稀疏向量，利用稀疏表示方法得到稀疏向量，根据数学模型进行数据或图像重构。在这些应用中，观测数据一般含有噪声。（2）基于分类的应用。这类应用的本质是模式识别，将表征对象主要的或本质的特征构造稀疏向量，这些特征具有类间的强区分性。利用稀疏表示方法得到这些特征的值，并根据稀疏向量与某类标准值的距离，或稀疏向量间的距离判别完成模式识别或分类过程，例如盲源分离、 音乐表示与分类、人脸识别、文本检测。

##1.3 SVM和稀疏表示理论在人脸识别上的应用

###1.3.1 SVM

人脸识别技术是一种采用计算机分析人脸图像， 从中提取出有效的识别信息，用来“辨认”身份的一门技术。人脸识别技术在驾驶执照、罪犯身份识别、自动门卫系统、银行、海关的监控系统等领域等得到广泛的应用。但是由于人脸图像受到表情、成像角度、光照、姿态等条件的影响， 同时人脸识别还涉及到计算机视觉、模式识别等学科， 诸多因素使得人脸识别成为一项极富挑战性的课题。

在人脸识别过程中，特征提取从人脸图像中提取最有效的识别信息，人脸分类器是对有效信息进行建模，并对待识别人脸进行识别。当前主要的特征提取方法为主成分分析、小波分析、奇异值特征向量等方法， 这些特征提取方法都是基于光照不变特征的方法， 当光照变化不大时， 可以取得比较好的效果，但是光照变化比较大时，则计算比较复杂，容易获得带噪声的人脸特征向量，从而使后续的人脸识别精度比较低。当前人脸分类器主要有: 判别分析法、隐马尔可夫、贝叶斯分类器和最近邻分类器等。这此方法都是基于线性的分类器，当人脸表情变化不大时，识别率比较高，但是人是一种有感情的动物，表情相当丰富，获得的特征是一种非线性向量，因此这些方法有时对人脸识别率不高，尤其对于面部表情之间差别比较大的图像，识别率较低。神经网络分分类识别能力强，广泛应用于人脸识别。但是神经网络是一种大样本学习方法，本身存在不可克服的缺陷如: 过拟合、局部最优等。传统的模式识别方法中，支持向量机( Support Vector Machines，SVM) 是一种专门针对小样本、高维数模式识别问题的算法，解决了神经网络存在的缺陷，成为人脸识别的首选分类器。

###1.3.2 稀疏表示

人脸的稀疏表示是基于光照模型。即一张人脸图像，可以用数据库中同一个人所有的人脸图像的线性组合表示。而对于数据库中其它人的脸，其线性组合的系数理论上为零。由于数据库中一般有很多个不同的人脸的多张图像，如果把数据库中所有的图像的线性组合来表示这张给定的测试人脸，其系数向量是稀疏的。因为除了这张和同一个人的人脸的图像组合系数不为零外，其它的系数都为零。

上述模型导出了基于稀疏表示的另外一个很强的假设条件：所有的人脸图像必须是事先严格对齐的。否则，稀疏性很难满足。换言之，对于表情变化，姿态角度变化的人脸都不满足稀疏性这个假设。所以，经典的稀疏脸方法很难用于真实的应用场景。

稀疏脸很强的地方在于对噪声相当鲁棒，相关文献表明，即使人脸图像被80%的随机噪声干扰，仍然能够得到很高的识别率。稀疏脸另外一个很强的地方在于对于部分遮挡的情况，例如戴围巾，戴眼镜等，仍然能够保持较高的识别性能。上述两点，是其它任何传统的人脸识别方法所不具有的。


#二 实验数据
本实验基于Yale大学研究者提供了公开数据集。

其中包含38个人的人脸图片，每人65幅共计2470幅大小为192×168的图片，实验先将每幅图像降采样为48×42大小的图像后拉伸为一个一维的向量。随机地从每个人的图片中取p幅图片作为训练样本，38个人总共38*p个训练样本，剩下的图片作为测试样本。



#三 算法简介


##3.1 SVM
###3.1.1 基本原理
给定训练样本集，在特征空间上找到一个分离超平面，将样本点分到不同的类。其中当且存在唯一的分类超平面，使得样本点距离分类超平面的距离最大。其中，距离超平面最近的点为该超平面的支持向量。找到超平面后，对于待测点，通过计算该点相对于超平面的位置进行分类，如图3-1。其中，一个点距离分离超平面的距离越大，表示分类预测的确信程度越高。这里省去SVM的数学推导。

###3.1.2 惩罚参数
惩罚参数表示对误分类点的惩罚权重。 如下图所示，惩罚参数的设置相当与在训练集的误差和间隔平面的距离上做一个折衷选择。当惩罚参数过大，易出现过拟合的情况，预测时，易导致误分情况。减小惩罚参数，开始容忍样本点落入间隔平面之内。过小会导致训练集的样本点对结果影响变小分类功能丧失。因此，选择合适的惩罚参数，会大大提高分类器的性能，非常关键。 实际运用过程中，采用交叉验证的方法选择合适的参数C。

交叉验证（Cross Validation）即将所有的样本分成：训练集（train set），验证集（validation set）和测试集（test set），合适的划分比例如（3:3:4），使用不同的参数训练样本，在验证集上验证表现性能，得到一组最佳参数再应用在测试集上计算最终精度。此方法可大大减少因设置参数花费的时间。在本文的代码中改参数选用了默认值。

###3.1.3 核函数
实际的训练集通常是线性不可分的，这就需要运用到核技巧将原空间中的点映射到线性可分的高维空间，如图3-2。常用的核函数有：线性核函数、多项式核函数、高斯核函数（RBF核函数），sigmoid核函数。核函数的选择对分类器的影响较大，本实验选取高斯核，因为时间有限，对选用哪种核函数效果更好不做进一步的讨论。

###3.1.4 多分类支持向量机
实际分类通常涉及多类问题的区分，而SVM的理论是二类问题的区分，解决多类问题通常的方法如下，前两种方法最常见：（1）一对多（one-vs-rest），构造k个SVM，分类时将未知样本分类为具有最大分类值的那类；（2）一对一（one-vs-one），任意两个样本之间设计一个SVM共n(n-1)/2，分类时为得票最多的类；（3）层次分类（H-SVMs），分层：所有类别分成子类，子类再分，循环得到单独的类。

###3.1.5 Libsvm工具箱介绍
libsvm是台湾大学林智仁教授开发设计的一个简单、快速有效的SVM模式识别和回归的软件包，可在C、Java、Matlab、C#、Ruby、Python、R、Perl、Common LISP、Labview等数十种语言里实现。该工具箱提供了很多默认参数，使用非常方便。训练模型用到函数：svmtrain。例如model = svmtrain(train_label,train_data,option)。

其中option的可选项如图3-3。


##3.2 稀疏表示
在SRC（Sparse Representation Classification）算法中，一个很重要的环节是对范数最小化问题的优化求解，不同的优化算法可涉及到L0、L1、L2范数等。对于L1范数最小化问题已经有很多种快速求解方法，主要包括有梯度投影Gradient Projection,同伦算法，迭代阈值收缩，领域梯度Proximal Gradient，增广拉格朗日方法，这几种方法都比正交匹配追踪算法OMP要高效的多。上述几种快速算法中，采用增广拉格朗日的对偶实现相比其它的快速算法要更好。在本文实验中选用Homotopy算法。

图3-4具体介绍SRC算法。

在本文实验的代码中，没有考虑上述SRC算法中的扰动e0项。
