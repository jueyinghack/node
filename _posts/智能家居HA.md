---
title: 智能家居一
tags: ["智能家居","HomeAssistant"]
categories: 智能家居
author: sec
date: 2023-04-26
---
## 智能家居

> 先来个背景说明，我目前有设备(小米多模网关2，温湿度传感器，门窗传感器)。我想将这些设备进行读取数据和控制。
>
> 尝试了很多方法。首先是尝试自己写python脚本去读取传感器的数据。温湿度传感器还好说，读到了。但是门窗传感器怎么都读不到数据。我就放弃了。
>
> 然后就想到了HomeAssistant以下简称(HA)。下面记录一下我踩到的坑。

### Python脚本读取数据

温湿度传感器的脚本我放到下边`这里你需要将MAC地址换为你自己设备的Mac地址`

```python
from dataclasses import dataclass
import time
from bleak import BleakClient
import asyncio
# 设备的mac地址
Mac = "A4:C1:38:52:52:FA"  # 你的温度计的MAC
@dataclass
class Result:
    temperature: float
    humidity: int
    voltage: float
    battery: int = 0

async def main(address):
    client = BleakClient(address, timeout=50.0)
    await client.connect()
    print("连接成功")
    while (1):
        buff = await client.read_gatt_char("ebe0ccc1-7a0a-4b0c-8a1a-6ff2997da3a6")
        try:
            result = Result(0, 0, 0, 0)
            temp = int.from_bytes(buff[0:2], byteorder='little', signed=True) / 100
            humidity = int.from_bytes(buff[2:3], byteorder='little')
            voltage = int.from_bytes(buff[3:5], byteorder='little') / 1000
            battery = round((voltage - 2) / (3.261 - 2) * 100, 2)
            result.temperature = temp
            result.humidity = humidity
            result.voltage = voltage
            result.battery = battery
            print(result)
        except Exception as e:
            print(e)
        time.sleep(1)


asyncio.run(main(Mac))
```

### HomeAssistant踩的坑

#### HA的安装

```python
# 这个首先就是安装了。我光安装就花费了大概两天的时间。
我尝试了HA官网的vmdk，安装好之后发现它不允许我访问8123端口。我可以访问4357的端口。然后我以为是网速慢的问题，结果等了好长时间也不能访问。直接果断放弃。
然后就在网上查找其他版本的vmdk。
5.x 具体忘了5.几了。然后安装完成，可以访问8123端口，但是访问4357端口的时候显示"Healthy:Unhealthy",导致我没有办法安装插件也就是hacs。然后也果断放弃。
7.4 我先试了一个7.4的，也是显示"Healthy:Unhealthy"，也放弃了。后来又找到了一个7.4的。我抱着试试的心态安装下来。发现可以访问端口，并且可以安装hacs。
# 总结，我觉得是vmdk的问题。并不是我们自己安装的问题。(网上有说高的版本的8123端口没有办法访问了，也不知道是不是真的)
```

#### HACS的安装

```python
# 这个地方我没有踩到坑()
1. 点击我的，然后开启高级模式
2. 配置-> 加载项 -> 加载项商店 -> 搜索ssh -> 安装 # (如果显示不安全，我的建议是直接放弃换个vmdk)
3. wget -O - https://cdn.jsdelivr.net/gh/hasscc/get@main/get | HUB_DOMAIN=ghproxy.com/github.com DOMAIN=hacs REPO_PATH=hacs-china/integration ARCHIVE_TAG=china bash -
4. 配置 -> 设备服务 -> 添加集成 -> 搜索hacs -> 添加进来就可以了
```

#### miio坑

```python
# 这个我花费了好几天的时间去连接设备。
我虽然用miio连接上了设备
但是，没有办法去读取设备的参数和控制设备。我认为是miio不支持我所购买的设备。最后也是无果
```

#### xiaomigateway3

```python
# 这个是我最终的解决方案。
首先我先安装了它的最小的版本master
发现无法将设备导入进去，登陆账号也无法登陆。所以我就升级了它的版本，直接升级到最新的版本
我输入米家的手机号和密码，将网关设备导入进去，然后发现没有反应。我当时就崩溃了。
然后等了一会发现，竟然有设备了。就是连接到小米多模网关的设备都可以发现了。并且可以读取到参数。
我只能说怎么没有早用xiaomigateway3呢
```


