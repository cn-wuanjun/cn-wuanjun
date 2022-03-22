---
description: 解决清晰度不足/模糊的问题
layout: editorial
---

# 图片处理-指定大小缩放

单独使用opencv或者PIL库的话都会出现不同程度的图片模糊，这里参考了文章：[https://www.hawu.me/coding/26](https://www.hawu.me/coding/26)，同时运用了这两个库。最终清晰度明显提高。

```python
import cv2, Image, ImageEnhance, time, os

strSourceFolder = raw_input("Input Source Images Folder:")
strOutputFolder = raw_input("Input Output Images Folder:")
nWidth = int(raw_input("Input Resized Width:"))

print "resizing..."
StartTime = time.clock()
nCounter = 0
for file in os.listdir(strSourceFolder):
    strSourceFilePathName = os.path.join(strSourceFolder, file)
    CV2_ImgOriginal = cv2.imread(strSourceFilePathName)
    fRatio = float(CV2_ImgOriginal.shape[1]) / nWidth
    nHeight = int(CV2_ImgOriginal.shape[0] / fRatio)
    CV2_ImgResized = cv2.resize(CV2_ImgOriginal, (nWidth, nHeight), None, 0, 0, cv2.INTER_AREA)
    CV2_ImgResized = cv2.cvtColor(CV2_ImgResized, cv2.COLOR_BGR2RGB)
    PIL_ImgResized = Image.fromarray(CV2_ImgResized)
    PIL_ImgEnhanced = ImageEnhance.Sharpness(PIL_ImgResized).enhance(2.0)
    strFilename = os.path.splitext(file)[0]
    PIL_ImgEnhanced.save(os.path.join(strOutputFolder, strFilename) + "_resized.jpg", 'JPEG', quality = 95)
    nCounter = nCounter + 1
    print file, "processed"
EndTime = time.clock()
print "Processed", nCounter, "images"
print "Total elapsed time:", EndTime - StartTime, "Seconds"
```

