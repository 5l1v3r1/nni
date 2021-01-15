#################
量化
#################

量化是指通过减少权重表示或激活所需的比特数来压缩模型，
从而减少计算量和推理时间。 在深度神经网络的背景下，模型权重主要的数据
格式是32位浮点数。 许多研究工作表明，在不显着降低精度的情况下，权重和激活
可以使用8位整数表示， 更低的比特位数，例如4/2/1比特，
是否能够表示权重也是目前非常活跃的研究方向。

A quantizer is a quantization algorithm implementation in NNI, NNI provides multiple quantizers as below. 你也可以
create your own quantizer using NNI model compression interface.

..  toctree::
    :maxdepth: 2

    Quantizers <Quantizer>
