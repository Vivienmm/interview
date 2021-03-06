# 什么是Code Review？
Code Review代码评审是指在软件开发过程中，通过对源代码进行系统性检查的过程。通常的目的是查找各种缺陷，包括代码缺陷、功能实现问题、编码合理性、性能优化等；保证软件总体质量和提高开发者自身水平。 Code Review是轻量级代码评审，相对于正式代码评审，轻量级代码评审所需要的各种成本要明显低得多，如果流程正确，它可以起到更加积极的效果。正因如此，轻量级代码评审经常性地被引入到软件开发过程中。

# 为什么Code Review？

1. 提高代码质量。
2. 及早发现潜在缺陷，降低修改/弥补缺陷的成本。
3. 促进团队内部知识共享，提高团队整体水平。
4. 评审过程对于评审人员来说，也是一种思路重构的过程。帮助更多的人理解系统。
5. 是一个传递知识的手段，可以让其它并不熟悉代码的人知道作者的意图和想法，从而可以在以后轻松维护代码。
6. 鼓励程序员们相互学习对方的长处和优点。 
7. 可以被用来确认自己的设计和实现是一个清楚和简单的。

# Code Review检查什么？

## 1. 结构问题
代码最大的问题，不是一两个地方有技术缺陷，也不是业务逻辑错误，而是整个软件设计的不好。前两者更容易通过测试或使用来发现和更正，但后者就不同了。如果回想一下自己见过的各种烂摊子，是不是有同感？具体哪里有问题怎么改说不上来，就是整个软件看上去混乱无章，无从下手。
具体结构问题包括：重复拷贝代码（不封装函数，不用Template/泛型……），函数过长（超过一屏幕就叫过长），错误封装（不恰当的public/不用Interface/不内聚/强耦合/在类中封装了无关方法……），内容错误（多个无关类置于一个文件/不恰当的命名……）等等。
改正结构问题，是从编写可靠软件向编写精美软件迈进的重要方法。
## 2. 业务逻辑问题
就是软件是否与需求的要求符合的问题。审核者和被审核者经常对业务需求的理解有差异，借此机会同步一下，必要时引入PO（产品经理/产品负责人）。
有人会说业务逻辑问题不是一测试就知道了吗？可是测试一般发生在很久以后，有些逻辑测试还需要一定的触发条件，而且测试只会发现失效（failure, 与预期不符）而不能发现缺陷（defect,具体哪里出了错），等积累长了，谁也找不到原因了。
## 3. 编程素养问题
很多问题属于那种“这样也行那样也行”的状态，比如命名/初始值/缩进/断行……但是高手的做法总是比新手好一些。
比如bool result= true; 这句话就有问题，刚初始化就先宣布成功，必有隐患。这是一个真实案例，而下面也的确有一个分支错误地返回了这个true（实际案例是个HRESULT）。而发现这个问题，不是测试而是代码检查。实际上测试几乎发现不了这些问题，比如上面那段代码会在某文件打不开的时候错误地返回这个true，而在测试中几乎不会故事破坏那个文件来测试其结果。

# 经常进行Code Review

常见的Code Review是高手审核新手，或者师傅走查徒弟。一般而言，大致高手每天能编写100多行有效代码（按分号计数），新手会多一些但也不超过200（他们编写代码比较费），也就是10个屏幕以内。有经验的人一定知道：高手看新手的代码，5秒钟就能发现问题。所以不用花上很长时间去做Code Review，而应该“少吃多餐”，每次可以5分钟，10分钟，每天2-3次甚至更多。看到一个问题就要彻底解决，不需要一次检查很多，问题一次比一次少即可。
但是切记不可积累，隔很长时间才去做Code Review，你就会面临那近万行的代码，以前N多掺和在一起的功能，你会发现，整个Code Review变得非常地艰难，用不了一会儿，你就会发现你会疲惫地打着哈欠，但还是要坚持，有时候，这样的Review会持续N个小时以上，相当的夸张。而且会出现相当多的问题和争论，因为，这就好像，人家都把整个房子盖好了，大家Review时这挑一点那挑一点，有时候触动地基或是承重墙体，需要大动手术，让人返工，这当然会让盖房的人一下就跳起来极力地维护自己的代码，最后还伤了他人的感情。

# 我们怎么做 Code Review

我带过的项目中，做CodeReview这方面大多感觉比较凌乱，也没有什么统一的做法。不过从形式上来看大体可以分为两大类：一类是TM技术经理对项目中成员Team一个一个的做Code Review，或者是团队资深人员来做（姑且就叫个人式吧）。一类是做Code Review Meeting，以会议形式来做Code Review（姑且叫会议式）。
## 1. 个人式
对于个人式，其实在上面“如何做CodeReview”的话题中已经谈到了很多了。包括我们要及时的不定期的每时每刻的去做Code Review，包括我们要按照结构问题，业务逻辑问题，编程素养问题逐一去检查Code等等。很多项目我们也都做了，甚至是都做到了。只是还有不够好的地方，需要深入的地方。具体的方法上面已经讲了，后面我会具体讲讲如何量化和跟踪。而对于PM来说，如何监控Code Review这件事就显得非常重要。
## 2. 会议式
会议式，真正的会议式去做代码评审，如果做到位了效果应该是最好的，最理想的情况是一堆专家（包括技术专家甚至还有业务专家、测试专家等），拿着代码一行一行的去Review。但是这种做法的成本也非常之高，不管是时间成本也好，还是费用成本都相当的昂贵，一般只有在大型尖端项目才会使用，比如航天航空的项目，做Code Review之后的缺陷率是相当的低的。我们是怎么做Code Review Meeting的呢？首先我们会在开会之前，选出典型的案例或者问题一起拿到会上去讨论，多半是分享一些经验和强调一些容易犯错的地方。一般一次会议不会超过2个小时，每周一次会议即可。这样会议的效果比较好，成本也相对较低。因为由于Team中成员的“素质”参差不齐，所以一起去做代码评审确实效果很差。

# 我对 Code Review的一点思考

作为PM我，对Code Review的思考是，我应该如何管理好Code Review？也就是说假设我把Code Review当做一个项目来看，怎样做好这个项目呢？
其实很简单，首先我要有一个正确的、真实的、可执行的计划，然后能在实施Code Review时给予TM或评审人一定的指导，再然后跟踪偏差，分析原因，变更计划。
那麼如何做計劃？而且要是正确的、真实的、可执行的。这里我们需要结合一下Project Quality Plan了。可能有的童鞋还不知道，我简单解释一下Project Quality Plan，Project Quality Plan是一个项目质量计划，主要内容有项目交付物以及交付要求，计划达到怎么样的质量目标，要采取怎么样的过程方法，Quality Breakdown各个阶段的质量目标分解等等。通过详细的质量目标分解我们就可以预测各个阶段预计产生的缺陷数是多少。此时我们PM就要思考，有了各个阶段的缺陷数量，我们是不是可以分解一下，那么我们做Code Review的目标是要发现多少缺陷呢？举个例子：假设我们代码的规模是100k行，我们目前团队产生缺陷数的基线大概是12~15 (Bugs/Kloc)，Code Review需要找出8~10 (Bugs/Kloc)，也就是100*8~10=800~1000。这样一来我们总数就有了，也就是说对于100k代码行这种规模的项目我们CodeReview总共要找到800~1000个缺陷才算达到了比较好的效果。当然如果做到这里还远远不够，我们还要对这个目标进行细化的分解。要分解到模块，分解到人（如果多人Review的话）。分解到模块很好理解，我们把整个系统分解为几个大的模块，或者模块集（相关性大的可以放一起）。然后分析模块的难易度，以及模块将来可能的负责人，然后评估每类模块我们应该找到多少缺陷。可能对于业务复杂或者算法复杂或者负责人水平较低的模块我们需要更多的时间去Review并产出更多的缺陷，反之则少。如下图：

有了具体的计划CodeReview的时候也就有了指导和参考目标。在执行的时候我们也就可以规划出人合理的力投入分配。做起来相对来说就比较容易了。
最后就是跟踪、偏差分析与变更了，当发现我们与实际计划又严重偏差我们要分析原因，然后做计划变更。比如发现偏差时，我们可以用根因分析，人、机、料、法、环、测。我们哪里做的不够好，如果可以解决，找出主要原因立刻解决即可。如果发现是计划有问题就去变更计划好了。这里就不讨论具体方法了。方法有很多，只要适合自己的项目即可。
其实CodeReview的方法还有很多，比如结对编程也是一种很好的形式，特别适合敏捷XP团队，但是因为目前我也没有很好的实践，所以也就没有写到。
最后希望我写的对大家能有一点点的帮助。也欢迎对Code Review有自己见解的朋友能和我一起来探讨这个话题。并欢迎指正我不对的地方。
