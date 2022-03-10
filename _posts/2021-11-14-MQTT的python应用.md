---
layout: post
title: MQTT的python应用
categories: 日志
tags: 
    - 日志
    - 大二
BGImage: 'https://raw.githubusercontent.com/xutongxin1/PictureBed/master/img0/20220310123346.png'
jekyll-theme-WuK:
    musicid: '34367899'
---

这一天

很复杂

电赛的实际效果和结局

虽然已经能猜到这个结局

但是还是停不下自我的指责和对自己的谩骂

或许都是我咎由自取吧





首先是Python包，安装

```shell
pip install paho-mqtt
```

官方文档：

https://www.eclipse.org/paho/index.php?page=clients/python/docs/index.php

例程

```python
import paho.mqtt.client as mqtt
import struct
import json
import base64
import hmac
import time
from urllib.parse import quote

DEV_ID = "first"  # 设备ID
PRO_ID = "    "  # 产品ID
DEV_NAME = 'first'
accesskey = "   "


def token(id, access_key):  # 官方文档给出的核心秘钥计算算法

    version = '2018-10-31'

    res = 'products/%s' % id  # 通过产品ID访问产品API

    # 用户自定义token过期时间
    et = str(int(time.time()) + 3600)

    # 签名方法，支持md5、sha1、sha256
    method = 'sha1'

    # 对access_key进行decode
    key = base64.b64decode(access_key)

    # 计算sign
    org = et + '\n' + method + '\n' + res + '\n' + version
    sign_b = hmac.new(key=key, msg=org.encode(), digestmod=method)
    sign = base64.b64encode(sign_b.digest()).decode()

    # value 部分进行url编码，method/res/version值较为简单无需编码
    sign = quote(sign, safe='')
    res = quote(res, safe='')

    # token参数拼接
    token = 'version=%s&res=%s&et=%s&method=%s&sign=%s' % (version, res, et, method, sign)
    return token


def on_connect(client, userdata, flags, rc):
    print("连接结果:" + mqtt.connack_string(rc))


# 从服务器接收发布消息时的回调。
def on_message(client, userdata, msg):
    print(str(msg.payload, 'utf-8'))


# 当消息已经被发送给中间人，on_publish()回调将会被触发
def on_publish(client, userdata, mid):
    print(str(mid))
def on_subscribe(client, userdata, mid, granted_qos):
    print(str(mid))

if __name__ == '__main__':
    passw = token(PRO_ID,accesskey)
    print(passw)
    client = mqtt.Client(DEV_NAME, protocol=mqtt.MQTTv311)
    # client.tls_set(certfile='/Users/mryu/PycharmProjects/MyProject/onenet/MQTTS-certificate.pem') #鉴权证书
   
    client.on_connect = on_connect
    client.on_publish = on_publish
    client.on_message = on_message
    #client.on_subscribe = on_subscribe
    client.connect('183.230.102.116', port=1883, keepalive=60)
    client.username_pw_set(PRO_ID, passw)


    client.loop_start()
    i=0
    while True:
        client.subscribe("$sys/oA8jn8R554/first/thing/property/post/reply",0)
        time.sleep(1)
        i+=1
        data='{"id": "123","version": "1.0","params": {"consumption": {"value": '+str(i)+'},"lightIntensity": {"value": 233}}}'
        client.publish("$sys/oA8jn8R554/first/thing/property/post",data)
```

要点：

先注册回调函数实例

```python
	client.on_connect = on_connect
    client.on_publish = on_publish
    client.on_message = on_message
    #client.on_subscribe = on_subscribe
```

再启动连接

```python
client.connect('183.230.102.116', port=1883, keepalive=60)
client.username_pw_set(PRO_ID, passw)
```

最后client.loop_start()即可**不阻塞**运行客户端



测试软件

MQTTX不太好用

还是用了MQTT.fx软件

不过这个软件快下载不了了，因此存档到云盘上了



以上

2021年11月14日16:14:46

