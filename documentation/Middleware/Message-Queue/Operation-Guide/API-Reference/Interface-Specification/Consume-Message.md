# 收消息

- 请求行

```
GET {Http接入点}/v2/messages HTTP/1.1
```

- 请求headers参数

请求公共参数请参考[公共参数](../Call-Method/Common-parameters.md)及[签名算法](../Call-Method/Signature-Algorithm.md)章节。

- Request Parameters

| 字段名               | 字段类型   | 必填         | 说明                                                        |
| :------------------- | :--------- | :----------- | :---------------------------------------------------------- |
| topic                | string     | Required     |                                                             |
| consumerGroupId      | string     | Required     |                                                             |
| size                 | int32      | Optional     | 一次**最多**拉取消息条数，0 < size <=32，defaultValue = 32  |
| consumerId           | string     | Optional     | defaultValue = httpProxyId              |
| consumeFromWhere     | string     | Optional     | 默认的起始消费位置，可选值：HEAD、TAIL，defaultValue = HEAD |
| filterExpressionType | string     | Optional     | 消息过滤表达式类型，目前可选值只有TAG                       |
| filterExpression     | string     | Optional     | 消息过滤表达式，默认没有过滤，如果需要过滤，此参数与filterExpressionType需同时传入 |
| ack                  | bool       | Optional     | 拉消息时是否由服务端自动ACK，可选值true、false，默认值defaultValue = false  |

- Response Body

1.请求成功

|  字段名   | 字段类型 | 说明                                                         |
|:----|:----|:----|
| requestId |  string  | 本次请求的requestId，用于搜索调用链                          |
|  result   |   map    | 返回格式为：`{"topicName":"lizhijian-041","ackIndex":31,"messages":[{"messageId":"messageId_1","messageBody":"test-0","properties":{"TAGS":"world"}},{"messageId":"messageId_2","messageBody":"test-2","properties":{"TAGS":"world"}}]}`|

2.请求失败

|  字段名   | 字段类型 | 说明                                                         |
|:----|:----|:----|
| requestId |  string  | 本次请求的requestId，用于搜索调用链                          |
|   error   |   map    | 返回格式为：`{"code":500,"message":"Topic information query failure","status":"INTERNAL"}` |
