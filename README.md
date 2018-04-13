# 基于可穿戴传感器数据的人类活动识别
   首先，如果你看到了这个Git，某种意义上，我会怀着一颗同情的心告诉你，这个研究方向，目前而言（2018年4月），前途很渺茫。当然了，这个判断是基于我是一名非985,211的一本研究生以及我的导师是从前年开始研究这个方向的（理由是他不想随大流做图像处理，而我们整个学校似乎都没有做自然语言处理的）这一大前提的。如果你所处的环境比我还恶劣，真心不建议继续这个方向，如果比我强，那就另当别论了。
# 基于可穿戴传感器数据的人类活动识别的现状
   当你从谷歌学术或者百度学术搜索相关论文的时候，你会发现，真正和这一课题相契合的文章很少，我查阅的论文大多来自于“MDPI”下的“Sensor”期刊。这是一个助攻传感器应用的期刊，这个期刊从去年10月份左右，就再也没有活动识别方向的论文发表了，就算是最新发表的几篇论文，质量也都相当一般（可想而这，目前这一研究领域的发展）。由于老师是半路出家，整个实验室在这一方向上的研究底蕴不是很足，因此，在这一课题上，我们实验室的研究现状本质上是，将其他领域的方法应用于可穿戴传感器数据上，也就是说，我们没有对可穿戴传感器数据的详细认知，所使用的方法也都是深度学习领域一些普适性很强的方法。基于这一显示，你也完全可以体会得到，这个Git的质量了。所以，到此，结合前面提到的研究环境，你就应该决定是否要继续看下去了。我虽然不知道怎么做出好东西，但是我明白垃圾是什么样子的。研究生三年里看到的水文太多了，因此，在我确实没有什么干货的前提下，真心不希望浪费别人的时间。

OK，既然你已经看到这里了，那我们就来聊聊给予我的研究背景下，我所认识到的**基于可穿戴传感器数据的人类活动识别的现状**
## 深度学习方法
### 卷积神经网络
最开始的时候，一句“循环神经网络更适合于分析时间序列数据，卷积神经网络更适合于分析网格型数据”让无数门外汉（包括我自己和我的导师）将LSTM作为了科研方向的首选，这里面有两个问题：
> 循环神经网络本身设计人为感太强，或者说他加入了太多人为臆想在里面，较之于卷积神经网络，他真的太“刻意”了。与此同时，也是他最大的问题，他无法并行计算，无论配合CUDA还是使用最新的SRU，他都不够快，不够“优雅”。

> Facebook和Google在2017年都提出了不使用循环神经网络来处理自然语言，而且精度都相当不错，至于其中的各种因果，这里不再详述。

这两个问题不是为了把RNN一棒子打死，只是如果有人再盲目吹捧RNN在时序数据上多么多么有效，离这样的人远一点。
虽然我对传感器数据本身研究的并不深入，但是深度学习目前最主要的一个问题是**可解释性较差**，算法本身都不能很好的被解释，就盲目的吹捧哪个算法好，这是真的不是做研究的人应该有的态度，与此同时，在不同的数据集上的实验也表明，绝大部分时候，CNN的精度都比RNN高，甚至某些情况下，MLP的精度都比RNN好。但是，如果你想写论文，还准备扯出一大堆理论，那么CNN里，最有尝试意义的应该是Facebook在2017年提出的Convolution Seq2Seq，这个模型兼顾CNN与时序数据，绝对可以保证你论文的字数。
### 循环神经网络
研究生阶段主要研究的方向是RNN做HAR，因此，这里主要说说RNN的应用。首先，原生LSTM或者GRU（这些真的不怎么重要，值得你去尝试，去试用，但是撑死算是前期工作），效果撑死算是中等水平。至于Sensor一篇论文里说对于不同的数据集使用不同的RNN结构（单向，双向各种级联），如果无法给出合理解释，而只是对实验结果进行分析（本人毕业论文就是这副德行），这样的工作可以帮助你发一篇比较水的论文，但是对你的科研最多只能起到夯实基础的作用，是不能有效推进你的科研任务的。各人比较倾向于研究注意力机制在RNN中的应用。包括各种简单的，复杂的。虽然就结果而言，他的精度可能不会有很大的提高，但是，这样的工作是有助于你的科研的。一些很简单的所谓注意力机制（各种层间全连接）适合于分类任务，而那些复杂的注意力机制完全就是为分割任务设计的（因为大多数注意力机制会应用于Seq2Seq模型），所以如果你准备研究注意力机制，那么最终，你的研究方向势必是分割，或者是基于分割的分类，和普通的分类任务略有不同。

### 当前阶段存在的问题
如果你看完上面的东西，结合我之前说过的，我们实验室只是在套用深度学习方法的前提，你就会发现，上述方法几乎全部来自于自然语言处理领域（上一届师兄师姐的方法全部来源于图像领域），这就注定了，如果你不回归到传感器数据本身，那么，你只能等待，等待别的领域出现什么新的方法，然后应用到传感器数据中，最后，写出一篇很水的文章。那么，就目前而言，还有哪些问题或者方法可以供我们借鉴呢（如果你自己没有能力创造新方法）？
> 传感器数据的使用方式，或者换一个高大上的名词“传感器融合”，普通方法都是直接将传感器切片，或者当做图片或者当做普通时序数据，但是需要注意的是，传感器数据有它自身的特点，比如不同身体部位的传感器应该怎么组合使用，传感器多个轴方向上的数据应该怎么组合使用等等，这些都需要仔细研究。

> 可以借鉴的领域，各人认为情感分析，文本分类以及语音分类是和HAR最相近的三个领域，前两个领域目前研究应该比较火爆，第三个没有研究，总感觉应该和HAR一样，死气成成。

> 分割问题。如上所述，引入复杂的注意力机制后，分类问题转化为了分割问题。分割问题本身还存在一大堆值得研究的问题，比如分割边缘是否及时，分割内部是否平滑等等。
