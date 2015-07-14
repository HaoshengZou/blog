.tar.gz 格式解压代码为 `tar -zxvf xx.tar.gz`

.tar.bz2 格式解压代码为 `tar -jxvf xx.tar.bz2`， 都相当于“解压到当前文件夹”

以上两行,z和j都是格式选项，z对应gzip，j对应bzip2，一般tar出来的，-xvf就够了，x是extract，v是verbose，f指明来源文件，后直接跟要解压的文件

`tar -tf` 列出当前压缩包的内容

`rm -rf <目录名>` 删除目录及其下所有文件

`mount src dst` 将iso文件挂载到某处，目前只能挂载为只读模式，**TODO：linux下像windows下一样解压出来，可以修改文件。**

`diff -s` 判断两稳健是否相同，`-s`汇报identical，否则不汇报
