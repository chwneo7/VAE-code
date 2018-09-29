# 变分编码器-code
电网技术论文所用代码。
包含两个网络及训练好的模型：
标准变分编码器中含卷积、反卷积层，实现编码解码。
条件变分编码器中，考虑到输入数据维度多变，卷积层输入数据维度及参数比较固定，使用全连接层进行了替换。
参考了keras官方例程https://github.com/keras-team/keras/blob/master/examples/variational_autoencoder_deconv.py
![标准变分编码器结构-基于Keras](https://www.z4a.net/images/2018/07/29/model.png)
![条件变分编码器结构-基于Keras](https://www.z4a.net/images/2018/07/29/CVAE-model.png)


# 方法优势
- **训练速度更快**：变分编码器由于编码器与解码器是同时训练的，同时采用全局交叉熵（或者你也可以选择MSE，效果一样）作为损失值，训练速度更快。在场景生成问题的测试中，变分编码器在300epochs时已经稳定收敛。
![300 epochs时，变分编码器已经收敛，损失值为交叉熵](https://i.loli.net/2018/07/29/5b5d72cae1b1f.png)

- **拟合效果好**：在图像领域，由于变分编码器采用全局交叉熵（或者MSE）作为损失值，相较于GAN生成的图像细节比较模糊。但是生成数据的整体概率分布上与实际数据拟合效果较好。在场景生成问题中也是如此，对于风电出力的部分断崖式概率变化，变分编码器会尝试以平滑曲线拟合整条概率曲线。由于功率曲线相比于图像来说，细节并不丰富，我们并没有在场景生成中发现细节模糊不清的问题，相反生成的功率曲线与实际曲线一致性很强。
![变分编码器以平滑曲线拟合整条概率曲线](https://s8.postimg.cc/cylg6kp79/PDF.png)
![生成数据与实际数据一致性更强](https://i.loli.net/2018/07/29/5b5d75cbad339.jpg)

- **反映空间相关性**：变分编码器同样能够学习到训练数据之间的相关关系，我们采用了具有复杂空间相关性的发电单元进行测试，同样在300 epochs时，变分编码器就实现了对复杂空间相关性的学习，生成数据的相关性只是稍有降低。
![快速学习到样本的相关性](https://www.z4a.net/images/2018/07/29/Snipaste_2018-07-29_16-23-14.md.png)
- **能灵活处理标签数据**：你可以使用我们提供的简化条件变分编码器生成标签数据，这样能避免繁琐的卷积层设计工作，简化后网络生成数据的概率分布同样贴近实际数据。


# 相关参考
chenyize实现了基于WGAN的场景生成
> Chen Y, Wang Y, Kirschen D, et al. Model-free renewable scenario generation using generative adversarial networks[J]. IEEE Transactions on Power Systems, 2018, 33(3): 3265-3275.

- 论文地址：
IEEE Xplore Digital Library [https://ieeexplore.ieee.org/abstract/document/8260947/](https://ieeexplore.ieee.org/abstract/document/8260947/) 
Arxiv [https://arxiv.org/abs/1707.09676](https://arxiv.org/abs/1707.09676)
- 代码仓库：[https://github.com/chennnnnyize/Renewables_Scenario_Gen_GAN](https://github.com/chennnnnyize/Renewables_Scenario_Gen_GAN)

