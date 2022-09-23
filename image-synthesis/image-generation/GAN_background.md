### GAN背景介绍   

生成性对抗网络(Generative Adversarial Networks, GAN)是于2014年首次引入，目标是合成与真实图像无法区分的人工样本，如图像。  

GAN的基本组成部分是两个神经网络: i) 生成器(Generator)，样本生成; ii) 判别器(Discriminator)，从训练数据和生成器输出的样本中鉴别样本是“真”还是“假”的。生成器的输入是一个随机噪声向量(random latent vectors)，因此生成性对抗网络的初始输出也是噪声。随着训练的进行，生成器(Generator)会收到判别器(Discriminator)的反馈，生成器(Generator)会学习合成更“真实”的图像，而随着训练的进行，判别器(Discriminator)的性能不断被改进，通过将生成器(Generator)生成的样本(generated sample)与真实样本(real sample)进行比较，使得生成器(Generator)更难欺骗它。 


论文提供了升级版本，
, （）