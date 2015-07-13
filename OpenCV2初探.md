FCN做银行卡数字行detection，因为不能接触公司SDK，因此用的OpenCV架构，用到了Mat类。

Mat类都是public成员，rows, cols记录行、列数，还有一个uchar* data用来new出data，矩阵元素类型不同，uchar*可能被替换成其他的。
