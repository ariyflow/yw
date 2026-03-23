感知层：

- 读卡，写卡，加密，防碰撞处理，遵循ISO/IEC 14443协议
- 传感器，温度，光照，霍尔，振动，温度光照包含数模转换
- 串口rs-232，向上层传输

网络层：

- 收集信息，封装
- GUI界面，实现串口交互，日志输出
- 与RFID的简单交互

应用层：

- 云端数据分析
- 后台管理系统，统筹管理所有RFID信息
- 数据库数据存储（RFID相关信息与传感器传来的数据）



##### 后端数据库设计：

数据库采用python自带的`sqlite3`实现，包含两个表：sensor_msg_table和device_msg_table。其中sensor_msg_table用于保存来自所有设备的传感器值；device_msg_table用来保存所有已知的设备（通过序列号唯一标识）

sensor_msg_table：

| 表项        | 含义                             |
| ----------- | -------------------------------- |
| id          | 每条记录的id                     |
| device_seq  | 设备序列号，唯一标识一个主机设备 |
| temperature | 记录的温度值                     |
| light       | 记录的光照值                     |
| hall        | 记录的霍尔传感器状态             |
| shake       | 记录的振动传感器状态             |
| timestamp   | 本条记录的时间戳                 |

device_msg_table：

| 表项        | 含义         |
| ----------- | ------------ |
| id          | 每条记录的id |
| device_seq  | 设备序列号   |
| create_time | 记录的时间戳 |



##### 后端API设计：

###### api/test

测试API能否进行通信。

发送：GET或POST请求

接收：200, test api success.



###### api/submit-sensor-data

提交传感器数据。

发送：POST请求，示例：

```python
import requests

url = "http://127.0.0.1:4343/api/submit-sensor-data"

data_buf = {
    "device_seq": "testseq",
    "temperature": 25,
    "light": 100,
    "hall": 1,
    "shake": 0
}

resp = requests.post(url, json = data_buf)

print(resp.text)

resp.close()
```

返回：

```bash
{
  "id": 2,
  "ok": true
}
```

id是该条目保存的id（自增），ok是保存状态，true为保存成功

###### api/fetch-sensor-data

从表`sensor_msg_table`中获取信息，需要传入两个参数：

- start: 开始的索引
- fetch_number：获取的数据条数

例如：

```python
import requests

url = "http://127.0.0.1:4343/api/fetch-sensor-data"

data_buf = {
    "start": 0,
    "fetch_number": 20
}

resp = requests.post(url, json = data_buf)

print(resp.text)

resp.close()
```

上面的代码会返回：

```bash
{
  "data": [
    {
      "device_seq": "testseq",
      "hall": 1,
      "id": 1,
      "light": 100,
      "shake": 0,
      "temperature": 25.0,
      "timestamp": "2026-03-06 07:35:22"
    }
  ],
  "fetch_number": 20,
  "ok": true,
  "start": 0,
  "total": 1
}
```

> 由于测试数据库中只包含一条数据，所以只返回了一个

