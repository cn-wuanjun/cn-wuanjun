# 图片处理后出现的色差问题

图片的rgb颜色标准有2类：sRGB和Adobe RGB

一般网上的参考代码不管是PIP还是OpenCV都不会提及这一点，在遇到Adobe RGB格式的图片时，处理完后就会出现明显的色差问题。

#### 解决方法

一张图片都是有配置信息的，存储在icc文件中，我们只需要读取原始图片的icc配置信息，最后在写入新图片时配置上对应的icc信息即可：

```python
img.save(res_path, icc_profile=origin_img.info.get('icc_profile'), quality=100)
```

> 参考网页：https://www.qiniu.com/qfans/qnso-11041044#comments
