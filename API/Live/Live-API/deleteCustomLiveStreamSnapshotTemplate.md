# deleteCustomLiveStreamSnapshotTemplate


## 描述
删除用户自定义直播截图模板

## 请求方式
DELETE

## 请求地址
https://live.jdcloud-api.com/v1/snapshotCustoms/{template}

|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**template**|String|True| |录制模板自定义名称|

## 请求参数
无


## 返回参数
|名称|类型|描述|
|---|---|---|
|**requestId**|String|ruquestId|


## 返回码
|返回码|描述|
|---|---|
|**400**|Invalid parameter|
|**401**|Authentication failed|
|**404**|Not found|
|**503**|Service unavailable|
|**200**|OK|
|**500**|Internal server error|
