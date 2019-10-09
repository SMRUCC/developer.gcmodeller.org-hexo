# 基因组功能富集计算分析

<!-- 2019-08-12 -->
<!-- :: course,dev,gsea,kegg,fisher test -->

基因富集分析是分析基因表达信息的一种方法，富集是指将基因按照先验知识，也就是基因组注释信息进行分类。
Aravind Subramanian等人于2005年提出了基于基因集 (gene set) 定义的基因富集分析方法。  首先要定义基因集，也就是基于我们的先验知识（基因组注释信息），将基因富集，可以想象成，用一堆代表基因功能的箱子（bin）把具有相同或相似功能的基因装起来，起到了降维的作用，当然，每个基因可能同时参与好几种功能。

GSEA基因功能富集分析所表达的思想和理念可以概括为系统生物学中对生命结构的系统性以及整体性的研究思路。对于某一个代谢途径而言，假设只存在有一个基因有很高的变化FoldChange，而其他基因的表达无明显变化，则会因为对于在该代谢途径中而言，过于缺少变化较大的partner基因，从而该代谢途径的最终表型变化不明显。则这个代谢途径收到实验设计的影响比较小。
反之一个代谢途径大部分基因都处于差异较明显的变化状态中，则可以认为该代谢途径是因为受到我们的实验设计的影响，产生了显著变化。即我们的生物学实验设计，让细胞的某一种表型产生了变化。

这样，得到这两组数据后，我们所分析的不是单个基因表达的差异，而是箱子与箱子之间的差异。由此，我们得到的数据更容易解释。进行基因富集计算分析，我们一般会需要完成下面的几个步骤：

1.	Background的创建
Background一般是对基因组中的基因按照特定的目标功能进行分组，例如KEGG代谢途径，COG直系同源簇，GO词条注释等。当然，这个步骤主要是针对非模式生物的基因组，模式生物因为一般都有前人的研究工作的构建结果，我们一般是直接拿来用即可。如果我们觉得用到的background的数据比较陈旧了，也可以自己手动完成注释，进行Background的更新。
Background的创建我们一般是通过Uniprot蛋白序列数据库来完成的，因为在Uniprot数据库之中存在着非常丰富完整的经过人工修订的KO，COG，GO等注释信息，只需要用工具提取出这些信息然后做蛋白序列比对取直系同源的结果就好了。
2.	置换检验
在拥有了需要进行分析的目的基因列表之后，我们就可以在所创建的Background的基础上进行置换检验，也就是富集计算分析了。最常见的置换检验为Fisher精确检验。
假设检验用来检验一次随机实验的结果是否支持对于某个随机实验的假设。具体如下：随机事件发生的概率小于0.05则认定该事件为小概率事件。一般原则认为在某个假设前提下，一次随机实验的结果不会出现小概率事件。若一次随机实验的结果出现了小概率事件则认定该假设不被支持。
Fisher精确检验是统计显著性检验方法，用于检查两个二进制变量的相关性。所谓二进制变量就是变量的值域只有两个值，例如：出现在某一个代谢途径中或者不出现在该代谢途径中。

3.	结果可视化
因为我们一般使用pvalue来表示富集计算的结果，而pvalue是小于1的一个很小的数，不方便进行作图。所以在进行可视化的时候，我们一般会对结果数据做负log10转换，取负log10（pvalue），这样子就可以转换为一个一般分布于0到1000以内的结果值，方便进行诸如柱状图，气泡图，网络图之类的富集分析结果的可视化了。
进行富集分析结果可视化，我们一般是使用柱状图来完成。在柱状图中，横坐标是基因的功能分类，例如KEGG代谢途径，GO词条，COG直系同源分类等。而纵坐标，则是取负log10转换之后的P值。

手工富集计算分析
前面我说了那么多，可能大家还是不太明白怎么个计算。为了能够让大家可以更加深入的理解富集计算的原理，在这里跟着我进行手工富集计算分析吧。
在这里我们所研究的黄单胞菌基因组中存在着4252个基因，通过对蛋白组实验数据的分析，我们得到了132个差异蛋白。
对于一个双组份系统代谢途径A，其在基因组中有133个基因，所以在基因组中还有4252减去133总共4119个基因不存在于这个代谢途径A中。
对于我们所得到的132个差异蛋白构成的基因列表而言，属于代谢途径A的基因有132个，所以在我们的差异蛋白列表中，还有132减去132总共0个基因不存在于代谢途径A中。
我们现在就有了进行Fisher计算所需要的a,b,c,d这4个数字了。

嗯，来来来，重新复习一次，刚才走神的同学可以再学习一次：
a等于132，是差异基因列表中存在于途径A中的基因数量
b等于133，是整个基因组中，途径A中的基因数量
c等于0，是我们的132个差异基因中不存在于途径A中的基因的数量
d等于4119，是整个基因组中，不存在于途径A中的基因的数量
同学们，记住了么？

根据Fisher检验的简单计算公式，我们会需要计算几个数的阶乘就好了。。
嗯，。。我们会需要分别进行a+b, c+d, a+c 和b+d这几个和的阶乘计算，将这4个结果值的阶乘乘上，得到了分子部分X。
接着呢，我们就需要分别计算a，b，c，d这4个数字他们各自单独的阶乘结果值，以及a+b+c+d总数的阶乘值，将这5个阶乘结果值乘上，我们就得到了分母部分Y。
最后呢，将分子除以分母，计算X/Y就可以得到Fisher检验的Pvalue结果值了。嗯！程序溢出了吧，哈哈哈哈。
在实际的计算编程中，因为64位双精度的内存长度限制的原因，阶乘计算一般会超过Double类型的上限，Fisher检验还不可以这样子直接来计算。给你们看一下我写的Fisher检验的计算代码吧，够复杂的吧。大家都先别急着头晕，调用这个检验计算函数还是非常简单的。
我们的例子的富集计算的pvalue结果值约等于为2.82526342406577E-175，进行负log10的转换后为174.5489，富集计算的结果非常的显著。
在R环境中，我们可以直接使用fisher.test函数来进行检验计算，我们首先要用matrix函数构建出一个2x2的二元个数矩阵，然后直接将这个个数矩阵传递进入fisher.test函数中就可以完成计算了。嗯嗯嗯，fisher.test函数的计算结果显示pvalue约等于4.455168e-178，这个是在KOBAS平台上面做的富集分析结果，pvalue为6.37772879983E-173，和我们的程序的计算结果有一些不一致，可能是大家用到的对log gamma结果的估算算法不一样的原因导致的吧。。