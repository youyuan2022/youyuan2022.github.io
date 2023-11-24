+++
title = 'How to call Baidu API'
date = 2023-11-22T22:20:10+08:00
draft = false

author = "youyuan"

+++

**一、安装所需要的模块**

~首先需要安装百度的aip模块（直接pip install baidu-aip会报错，需要镜像源，同时添加对镜像源的信任）

pip install -i <https://pypi.tuna.tsinghua.edu.cn/simple> \--trusted-host pypi.tuna.tsinghua.edu.cn baidu-aip

安装完执行可能会遇到报错[No module named \'chardet\'](https://www.cnblogs.com/whylinux/p/9839162.html)

继续安装chardet：pip install chardet -i <https://pypi.tuna.tsinghua.edu.cn/simple>

&nbsp;

参考的文档：

pip国内镜像源：<https://zhuanlan.zhihu.com/p/623325525?utm_id=0&wd=&eqid=b13e6e180006c6dc0000000664816590>

遇到报错Cannot determine archive format of xxx：<https://blog.csdn.net/qq_43665602/article/details/126235981>

&nbsp;

**二、在百度智能云创建应用**

获取免费的测试资源（每月一千次）

创建的应用的AppID,API Key Secret Key等下用

&nbsp;

**三、车牌识别的代码**

参考文档：<https://developer.aliyun.com/article/1322015>

```python
from aip import AipOcr

APP_ID = '自己的 App ID'
API_KEY = '自己的 Api Key'
SECRET_KEY = '自己的 Secret Key'

# 创建客户端对象
client = AipOcr(APP_ID, API_KEY, SECRET_KEY)

# 建立连接的超时时间，单位为毫秒
client.setConnectionTimeoutInMillis(5000)

# 通过打开的连接传输数据的超时时间，单位为毫秒
client.setSocketTimeoutInMillis(5000)

# 读取图片
def get_file_content(filePath):
    with open(filePath, 'rb') as fp:
        return fp.read()

image = get_file_content('car.jpeg')

res = client.licensePlate(image)

print('车牌号码：' + res['words_result']['number'])
print('车牌颜色：' + res['words_result']['color'])
```

from aip import AipOcr：AipOcr导入了AipOcr类，用于接入百度OCR服务的类

client = AipOcr(APP_ID, API_KEY, SECRET_KEY)：凭借APP_ID、API_KEY和SECRET_KEY创建一个AipOcr的客户端实例

```python
client.setConnectionTimeoutInMillis(5000)

client.setSocketTimeoutInMillis(5000)
```

这两行设置了连接超时和数据传输超时的时间限制，避免程序长时间挂起等待响应。

def get_file_content(filePath):该函数以二进制模式读取图片文件的内容

res = client.licensePlate(image)：向百度OCR服务发送请求，用于识别图片中的车牌信息，返回的是一个字典对象

&nbsp;

**四、接口测试**

**Postman调用**

**获取Access Token**

选择GET

填写token接口地址：<https://aip.baidubce.com/oauth/2.0/token>

接着填写以下参数：

grant_type： 必须参数，固定为client_credentials

client_id： 必须参数，应用的API Key（公钥）

client_secret： 必须参数，应用的Secret Key（密钥）

点击send发送请求，得到的access_token就是需要的接口鉴权数据

**车牌识别**

选择POST

填写接口地址：<https://aip.baidubce.com/rest/2.0/ocr/v1/license_plate>

接着填写以下参数：

在Params中 access_token：上一部分中获取的

在Headers中 Content-Type：（固定值）application/x-www-form-urlencoded

在Body中 image：图片base64数据，去掉图片头，复制剩下的参数

在Body中 multi_detect：true

在Body中 multi_scale：true

点击send，即可得到识别结果

以下是参考资料和工具：

车牌识别API接口的使用方法：<https://cloud.baidu.com/video-center/video/741>

帮助文档-文字识别-车牌识别：<https://ai.baidu.com/ai-doc/OCR/ck3h7y191>

鉴权认证机制：<https://ai.baidu.com/ai-doc/REFERENCE/Ck3dwjhhu>

图片转base64编码：<http://www.jsons.cn/img2base64>

&nbsp;

**五、APIExplorer调用**
