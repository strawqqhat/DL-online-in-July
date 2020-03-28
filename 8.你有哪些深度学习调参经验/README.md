1. relu+bn：这套组合可以满足大多数的情况，除非有些特殊情况回用identity，比如回归问题，比如resnet的shortcut支路。
2. dropout：分类问题用dropout，只需要最后一层softmax前用基本就可以了，能够防止过拟合，可能对accuracy提高不大，但是dropout前面的那层如果是之后要使用的feature的话，性能会大大提升。
3. 数据的shuffle和argmentation：aug要适当使用
4. 降低学习率：随着网络训练的进行，学习率要逐渐降下来，利用tensorboard可视化工具可以发现在学习率下降的瞬间网络会有个巨大的性能提升，同样的fine-tuning也要根据模型的性能设置合适的学习率，比如一个训练的已经非常好的模型直接采用1e-3的学习率，那之前就白训练了，这就是说网络性能越好，学习率要月越小。
5. tensorboard：可视化可以帮助你监视网络的状态，来调整网络参数。
6. 随时存档模型，要有validation。把每个epoch和其对应的validation结果存下来，可以分析出开始过拟合的时间点，方便下次加载fine-tuning。
7. 网络层数，参数量：在性能不丢的情况下减到最小。
8. batch size：影响不是太大，特殊的算法需要大一些。
9. 卷积核分解：从最初的5x5分解为两个3x3，到后来的3x3分解为1x3和3x1，再到resnet的1x1,3x3,1x1，网络的计算量越来越小，层数越来越多，性能越来越好。
10. 不同尺寸feature map的concat：只用一层的feature map通常不如concat好。
11. resnet的shortcut很有用：重点在于shortcut之路一定要是identity，主路是什么conv都无所谓。
12. 针对metric learning，对feature加个classification的约束通常可以提高性能加快收敛。