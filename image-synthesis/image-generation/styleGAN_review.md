### styleGAN 介绍  

生成性对抗网络(Generative Adversarial Networks, GAN)是于2014年首次引入，目标是合成与真实图像无法区分的人工样本，如图像。随着时间的推移，GAN图像变得更加逼真。生成性对抗网络(Generative Adversarial Networks, GAN)的主要挑战之一是控制其输出，即改变人脸图像中的特定特征，如姿势、脸型和发型。

GAN应用程序的一个常见示例是通过从名人面孔数据集(celeA)学习来生成人造人脸图像。

StyleGAN就是为了解决这一挑战而提出的新模型。在合成人脸图像时，styleGAN渐进式生成人造图像，从较低的分辨率开始，一直到高分辨率（1024×1024）。通过分别修改每个级别的输入，以控制不同级中的视觉表达特征，从粗略特征(例如姿势、面部形状)到精细细节(头发颜色)，而不会影响其他级别的视觉表达特征。StyleGAN不仅能更好地理解生成的输出，还可以生成更真实的高分辨率图像。，看起来比以前生成的图像目目采用


2018年，NVIDIA首次提出用ProgressiveGAN生成高质量的大图像(尺寸:1024x1024)。ProgressiveGAN的关键创新在于渐进式训练，其首先用较低分辨率的图像(例如:4x4尺寸)训练生成器(Generator)和判别器(Discriminator)，然后每次增加一个更高分辨率的层。在ProgressiveGAN中，通过先学习低分辨率图像中出现的基本特征，来形成图像创建的基础，而且随着图像分辨率的增加和训练的进行，能够学习到更多的细节。训练低分辨率图像更容易，且更快，也有助于训练更高级别的图像(例如可防止落入次最优解之中)。因此，ProgressiveGAN总体训练速度也更快。ProgressiveGAN可以较好的生成高质量图像，但与大多数生成对抗网络(GAN)模型一样，ProgressiveGAN控制生成图像特定特征的能力也很有限，由于特征是纠缠耦合在一起的，若尝试调整输入，即使是很微笑的变化，也可能会同时影响多个特征。


StyleGAN可以视为ProgressiveGAN的升级版本，StyleGAN(Generator)重点关注生成器网络。StyleGAN的作者发现ProgressiveGAN的渐进层有一个潜在好处，如果使用得当，可以控制图像的不同视觉特征。StyleGAN的作者发现图层越低(图像的分辨率越低)，其影响的特征越粗糙。在StyleGAN论文中，特征被分为三类: 
    * 粗分辨率(例如分辨率 8x8): 通常影响姿势、一般发型、面部形状等 
    * 中等分辨率(例如分辨率 16x16和32x32): 通常影响更精细的面部特征、发型、睁眼/闭眼等  
    * 精细分辨率(例如分辨率 64x64、128x128、256x256、512x512、1024x1024): 通常影响配色方案(眼睛、头发和皮肤)等微观特征   

 
StyleGAN生成器的几个模块:   
1. 映射网络(Mapping Network): 映射网络的目标是将输入向量编码为中间向量(隐变量)，中间向量(隐变量)的不同元素控制不同的视觉特征(非常重要的特性)。使用输入向量控制视觉特征的能力时非常有限的，因其需要遵循训练数据的概率密度。例如，若黑发人的图像在数据集中更加常见，则更多的输入值将会映射到该特征。图像生成模型无法将部分输入(向量中的元素)映射到特征的现象称为特征纠缠。通过使用另一个神经网络，styleGAN模型可以生成一个无需遵循训练数据分布的向量，且可以减少特征之间的相关性。在StyleGAN模型中，映射网络由8个完全连接的层组成，其输出与输入层的大小维度相同(512x1)。     
 
2. AdaIN(自适应实例规范化)模块: 传输由映射网络(Mapping Network)产生的中间潜空间编码信息 $\boldsymbol{w}^{'} \in \mathcal{W} $ 进入合成网络(synthesis network $ g $)。中间潜空间编码信息 $\boldsymbol{w}^{'} $ 会被添加到合成网络(synthesis network $ g $)的每个分辨率层(注意styleGAN的合成网络(synthesis network $ g $)中的初始输入为常数张量)。在每个级别的分辨率层中定义特征的视觉表达: 
    * 首先对卷积层输出的每个通道进行正则归一化
    * 中间向量(隐变量) $\boldsymbol{w}^{'} $ 使用另一个完全连接的层，转化为每个通道的比例和偏差。     
    * 尺度变化和偏置移动作用于卷积输出的每个通道，从而定义卷积中每个滤波器的重要性。     .. 
然后每次