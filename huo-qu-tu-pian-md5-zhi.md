# 获取图片md5值

```python
#!/usr/bin/python 
# -*- coding:utf-8 -*- 
import hashlib 

fd=open("1.jpg","r") 
fcont=fd.read() 
fmd5=hashlib.md5(fcont) 
#get 32 value 
print fmd5.hexdigest()
```
