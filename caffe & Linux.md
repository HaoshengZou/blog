nano似乎默认没有自动换行，也最好别设置成自动换行==

`nvidia-smi`命令查看显卡信息和进程占用资源情况。

`top`类似任务管理器，可以看到所有进程的用户、CPU内存占用情况等。

出现了输入正确密码不能进Ubuntu桌面的情况！烦了一晚上！终于解决了！
编译caffe的时候修改了`/etc/ld.so.conf`，以便能找到动态链接库，但重启后就进不了桌面，删掉自己新加的东西就好啦。进入桌面后需要重新加进去的东西在`新加卷`里的文本文档里，新加后再`sudo ldconfig`即可！

---
blob别想太多，就是一团、一组、一族、一簇的意思。

mnist第一层卷积层的learning rate (代码里往往称为lr) 的设置就已经靠经验了：

> blobs_lr are the learning rate adjustments for the layer’s learnable parameters. In this case, we will set the weight learning rate to be the same as the learning rate given by the solver during runtime, and the bias learning rate to be twice as large as that - this usually leads to better convergence rates.

即：weight和bias分别为solver里的一倍和两倍


写layer的规则在`$CAFFE_ROOT/src/caffe/proto/caffe.proto`，
网络的定义写在` $CAFFE_ROOT/examples/mnist/lenet_train_test.prototxt`，
solver（感觉是训练程序）写在`$CAFFE_ROOT/examples/mnist/lenet_solver.prototxt`,
再用一个脚本`./examples/mnist/train_lenet.sh`调用solver，实现训练，

源码都在Github上，就是直接pull下来的。caffe官网有一些理解性介绍，比如介绍各种层的定义和参数的意义：http://caffe.berkeleyvision.org/tutorial/layers.html

