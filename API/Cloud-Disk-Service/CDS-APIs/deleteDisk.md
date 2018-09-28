# deleteDisk


## 描述
-   删除一块按配置计费的云硬盘，云硬盘类型包括高效云盘和SSD云盘。
-   云硬盘被删除后，云硬盘快照可以被保留。
-   删除云硬盘时，云盘的状态必须为 待挂载（Available）。


## 请求方式
DELETE

## 请求地址
https://disk.jdcloud-api.com/v1/regions/{regionId}/disks/{diskId}

|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**diskId**|String|True||云硬盘ID|
|**regionId**|String|True||地域ID|

## 请求参数
无


## 返回参数
|名称|类型|描述|
|---|---|---|



## 返回码
|返回码|描述|
|---|---|
|**400**|Invalid parameter|
|**401**|Authentication failed|
|**404**|Not found|
|**503**|Service unavailable|
|**200**|OK|
|**500**|Internal server error|
