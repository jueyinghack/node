---
title: CSDN将文章保存为markdown格式
date: 2023-05-04 16:51:15
author: sec
---
> 今天我想将之前在csdn写的博客移植到hexo上。但是我比较懒，不想一个一个复制粘贴。所以就有了这篇文章。看了看csdn的加密，然后顺着它的加密去写获取文章markdown的脚本。脚本如下。写的比较乱，不优化了。如果有想优化的你们可以研究一下。


```python
import base64
from hashlib import sha256
import hmac
import requests
import json
cookie = ""


count = 0
key = "9znpamsyl2c7cdrr9sas0le9vbc3r6ba"


# sha256加密算法
def get_sign(data, key):
    key = key.encode('utf-8')
    message = data.encode('utf-8')
    sign = base64.b64encode(hmac.new(key, message, digestmod=sha256).digest())
    sign = str(sign, 'utf-8')
    print(sign)
    return sign


# 获取文章的data
get_article_data = """GET
application/json, text/plain, */*



x-ca-key:203803574
x-ca-nonce:8b234fe2-75aa-44bf-8990-061c99da48fb
/blog/phoenix/console/v1/article/list?page=1&pageSize=1000"""

# 获取文章的url
get_article_url = "https://bizapi.csdn.net/blog/phoenix/console/v1/article/list?page=1&pageSize=1000"
# 获取文章的headers
get_article_headers = {
    "accept": "application/json, text/plain, */*",
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36",
    "x-ca-key": "203803574",
    "x-ca-nonce": "8b234fe2-75aa-44bf-8990-061c99da48fb",
    "x-ca-signature": "{}".format(get_sign(get_article_data, key)),
    "x-ca-signature-headers": "x-ca-key,x-ca-nonce",
    "cookie": cookie
}
r = requests.get(get_article_url, headers=get_article_headers)
# 获取到文章的
articles = json.loads(r.text)['data']['list']
for msg in articles:
    # 遍历文章获取title，id，时间
    time = msg['postTime']
    title = msg['title']
    id = msg['downloadShareDataUrl'][-9:]
    # 生成hexo的作者，时间，标题
    head = """---
title: {}
date: {}
author: sec
---
""".format(title,time)
    # 获取markdown的data
    data = '''GET
*/*



x-ca-key:203803574
x-ca-nonce:8b234fe2-75aa-44bf-8990-061c99da48fb
/blog-console-api/v3/editor/getArticle?id={}&model_type'''.format(id)
    # 获取markdown的headers
    headers = {
        "accept": "*/*",
        "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/112.0.0.0 Safari/537.36",
        "x-ca-key": "203803574",
        "x-ca-nonce": "8b234fe2-75aa-44bf-8990-061c99da48fb",
        "x-ca-signature": "{}".format(get_sign(data, "9znpamsyl2c7cdrr9sas0le9vbc3r6ba")),
        "x-ca-signature-headers": "x-ca-key,x-ca-nonce",
        "cookie": cookie
    }
    # 获取markdown的url
    url = "https://bizapi.csdn.net/blog-console-api/v3/editor/getArticle?id={}&model_type=".format(id)
    r = requests.get(url, headers=headers)
    # 获取到了markdown
    content = json.loads(r.text)['data']['markdowncontent']
    # 将markdown保存到文件中
    with open("blog/" + str(count) + ".md", 'w', encoding='utf-8') as f:
        f.write(head)
        f.write(content)
        f.close()
        count += 1

```