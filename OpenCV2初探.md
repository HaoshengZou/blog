FCN做银行卡数字行detection，因为不能接触公司SDK，因此用的OpenCV架构，用到了Mat类。

Mat类都是public成员，rows, cols记录行、列数，还有一个uchar* data用来new出data，矩阵元素类型不同，uchar*可能是其他的指针类型。单通道图很好理解，多通道图不是公司SDK那样把同一个像素点上的三通道压到一个结构体内，而是：（OpenCV的高维Mat都这么理解）一维矩阵就是一个列向量，二维矩阵把每一行扩展成一行，三维矩阵把每行上的每个元素再扩展成一个小行，小行拼接起来，四维矩阵就继续把三维矩阵的每个元素扩展……总之形象地看，总是一个二维矩阵，只是每个元素还能更加深入罢了。

这跟官方文档是等价的！我第一遍看文档的时候没看出来，怪自己哇！文档中的地址计算公式：

addr(M<sub>i, j, k</sub>) = M.data + M.step[0] * i + M.step[1] * j + M.step[2] * k

i, j, k就对应row, col, channel的顺序，又有式M.step[i] >= M.step[i+1] * M.size[i+1]，
因此，channel的step[2]最小，为1（最后一个维度总是这样），col的step[1]为3 * 1=3, 
row的step[0]为：800（或实际列数）*3。
