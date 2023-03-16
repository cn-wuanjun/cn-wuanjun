# Jenkins中配置飞书提醒

在构建中,添加构建步骤:执行shell命令:

`python /usr/feishu.py "灵感前端测试环境开始部署"`

在部署完成后再添加步骤,执行shell命令:

`python /usr/feishu.py "灵感前端测试环境已完成部署"`

#### 脚本代码如下:

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
import sys

import requests

BUILD_STATUS = sys.argv[1]
url = 'https://open.feishu.cn/open-apis/bot/v2/hook/ed6bd528-3f61-4205-a3b7-7f57a9e52053'
method = 'post'
headers = {
    'Content-Type': 'application/json'
}
json = {
    "msg_type": "interactive",
    "card": {
        "config": {
            "wide_screen_mode": True,
            "enable_forward": True
        },
        "elements": [{
            "tag": "div",
            "text": {
                "content": BUILD_STATUS,
                "tag": "lark_md"
            }
        }],
        "header": {
            "title": {
                "content": "Jenkins通知",
                "tag": "plain_text"
            }
        }
    }
}
requests.request(method=method, url=url, headers=headers, json=json)
```
