
# 关于安装 tensorflow-directml 时报错解决方案

**cmd输入：**

```shell
pip install tensorflow-directml
```

**报错信息：**

```shell
ERROR: Could not find a version that satisfies the requirement tensorflow-directml (from versions: none)
ERROR: No matching distribution found for tensorflow-directml
```
**如图**
![报错信息](https://img-blog.csdnimg.cn/3bc9a7320f0a440782d4666607831c90.png#pic_center)
## **原因：**

> - **python的版本不对**，截止至2022年1月，tensorflow-directml 包还不支持Python3.7以上的版本，且只支持**Python3.5、3.6和3.7**

## 解决方法：

安装python3.7 到 python3.5之间的版本，并用新安装的版本的pip安装

---


若在**Pycharm**上安装tensorflow-directml时报错，出现如下图情况；

![Pycharm报错](https://img-blog.csdnimg.cn/b0b48815b2c94e12b8555b616c560098.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWmFuZSBaZW5n,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)


同理，检测python版本是否正确，切换到正确python版本下重新安装 tensorflow-directml 即可。
