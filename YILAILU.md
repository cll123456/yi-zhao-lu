# 记录单

## 1、添加记录单
* 简要描述：添加记录单
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/addRecordSheet](http://127.0.0.1:8080/api/recordSheet/addRecordSheet)
* 请求方式：POST
* 表头：
```
Content-Type:application/json
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|clothArr| 是| array|查看下面的衣服对象|
|userName|是|String|用户名字|
|recordTime|是|String|提交记录时间|
|recordStatus|是|Number|记录单状态(添加时后台默认为1)：0:禁用(PC查询可查询禁用的数据)；1:启用|

**衣服对象**

|参数名|是否必填|类型|备注|
|---|---|---|---|
|clothShape|是|string|衣服款式|
|clothQuantity|是|Number|衣服提交数量|
|clothPrice|否|Number|单件衣服的价格|
|clothSize|是|String|衣服的尺寸如：L,M,XL,110,120等|
|clothColor|是|String|衣服颜色|
* 输入示例：
```
{
	clothArr:[
	{
		"clothShape":"童装",
		"clothQuantity":100,
		"clothSize":"L",
		"clothPrice":21,
	},
	{
		"clothShape":"童装",
		"clothQuantity":100,
		"clothSize":"L",
		"clothPrice":21,
	},
	],
	"userName":"wq",
	"recordStatus":1,
	"recordTime":"2020-12-01 12:00:00"
}
```


## 2、记录单列表
* 简要描述：根据条件查询记录单数据
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/recordSheetList](http://127.0.0.1:8080/api/recordSheet/recordSheetList)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|pageSize|否|Number|当前页(后台默认第一页)|
|pageNum|否|Number|每页显示条数(后台默认显示10条)|
|clothSize|否|String|衣服的尺寸如：L,M,XL,110,120等|
|userName|否|String|用户名字|
|recordTime|否|String|提交记录时间|
|recordStatus|否|Number|记录单状态（默认不传查询启用记录，传入0，查询禁用数据）|

* 输入示例：
```
{
	"pageSize":1,
	"pageNum":10,
	"clothSize":"L",
	"userName":"wq"，
	"recordTime":"2021-11-12 10:12:02"
}
```
* 响应参数：

|参数名|类型|备注|
|---|---|---|
|userId|String|用户ID|
|remark|String|备注|
|clothSize|String|衣服的尺寸如：L,M,XL,110,120等|
|userName|String|用户名字|
|recordTime|String|提交记录时间|
|clothPrice|Number|衣服价格|
|clothShape|String|衣服款式|
|clothQuantity|Number|衣服提交数量|
## 3、记录单详情
* 简要描述：更具记录单Id查看详情
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/recordSheetDetail](http://127.0.0.1:8080/api/recordSheet/recordSheetDetail)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|id|是|String|记录单Id|

* 输入示例：
```
/api/recordSheet/recordSheetDetail?id="123213231231"
```
* 响应参数：

|参数名|类型|备注|
|---|---|---|
|userId|String|用户ID|
|remark|String|备注|
|clothSize|String|衣服的尺寸如：L,M,XL,110,120等|
|userName|String|用户名字|
|recordTime|String|提交记录时间|
|clothPrice|Number|衣服价格|
|clothShape|String|衣服款式|
|clothQuantity|Number|衣服提交数量|
|clothColor|String|衣服颜色|

```
{
	clothArr:[
	{
		"clothShape":"童装",
		"clothQuantity":100,
		"clothSize":"L",
		"clothPrice":21,
	},
	{
		"clothShape":"童装",
		"clothQuantity":100,
		"clothSize":"L",
		"clothPrice":21,
	},
	],
	"userName":"wq",
	"recordStatus":1,
	"recordTime":"2020-12-01 12:00:00"
}
```

## 3、记录单启用\禁用
* 简要描述：更具记录单Id改变记录单状态，启用：在查询时只会查询启用的数据，禁用后的数据必须通过PC端进行查看，禁用相当于历史数据
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/recordSheetEnableOrDisable ](http://127.0.0.1:8080/api/recordSheet/recordSheetEnableOrDisable)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|id|是|String|记录单Id|

* 输入示例：
```
/api/recordSheet/recordSheetEnableOrDisable?id="123213231231"&recordStatus=1
```
* 响应参数：
```
{
"code":200,
"message":"启用成功",
"result":null,
}
```

## 4、记录单删除
* 简要描述：更具记录单Id删除记录单，该删除是真删除，直接把该数据从数据库删除，该删除可能是记录时数据有问题，或者不需要作废情况，一般情况谨慎操作，可以在删除前做一份备份，
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/deleteRecordSheet](http://127.0.0.1:8080/api/recordSheet/deleteRecordSheet)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|id|是|String|记录单Id|

* 输入示例：
```
/api/recordSheet/deleteRecordSheet?id="123213231231"
```
* 响应参数：
```
{
"code":200,
"message":"删除成功",
"result":null,
}
```

## 5、记录单修改
* 简要描述：更改记录单数据
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/uptateRecordSheet](http://127.0.0.1:8080/api/recordSheet/uptateRecordSheet)
* 请求方式：POST
* 表头：
```
Content-Type:application/json
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|id|是|String|记录单Id|
|clothShape|是|String|衣服款式|
|clothQuantity|是|Number|衣服提交数量|
|clothPrice|否|Number|单件衣服的价格|
|clothSize|是|String|衣服的尺寸如：L,M,XL,110,120等|
|userName|是|String|用户名字|
|recordTime|是|String|提交记录时间|
|clothColor|是|String|衣服颜色|
|recordStatus|是|Number|记录单状态(添加时后台默认为1)：0:禁用(PC查询可查询禁用的数据)；1:启用|

* 输入示例：
```
{
	"id":"12324241",
	"clothShape":"童装",
	"clothQuantity":100,
	"clothSize":"L",
	"clothPrice":21,
	"userName":"wq"，
	"recordTime":"2021-11-12 10:12:02"
}
```
* 响应参数：
```
{
"code":200,
"message":"修改成功",
"result":null,
}
```


## 6、记录单分享
* 简要描述：分享记录单生成二维码
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/shareRecordSheet](http://127.0.0.1:8080/api/recordSheet/shareRecordSheet)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|id|是|String|记录单Id|

* 输入示例：
```
/api/recordSheet/shareRecordSheet?id="123213231231"
```
* 响应参数：

```
{
"code":200,
"message":"",
"result":{
	QRCode:"sadadsafdvsfrqacvdfadfafafas"
	},
}
```

## 6、查看分享记录单信息[输入密码验证，验证通过后获取详情]
* 简要描述：分享记录单生成二维码
* 请求URL：[http://127.0.0.1:8080/api/recordSheet/shareRecordSheetInfo](http://127.0.0.1:8080/api/recordSheet/shareRecordSheetInfo)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|QRCode|是|String|分享记录单二维码|

* 输入示例：
```
/api/recordSheet/shareRecordSheetInfo?QRCode="123213231231"
```
* 响应参数：

|参数名|类型|备注|
|---|---|---|
|userId|String|用户ID|
|remark|String|备注|
|clothSize|String|衣服的尺寸如：L,M,XL,110,120等|
|userName|String|用户名字|
|recordTime|String|提交记录时间|
|clothPrice|Number|衣服价格|
|clothShape|String|衣服款式|
|clothQuantity|Number|衣服提交数量|
|clothColor|String|衣服颜色|


# 个人中心
## 1、用户信息
* 简要描述：查询用户信息
* 请求URL：[http://127.0.0.1:8080/api/user/userInfo](http://127.0.0.1:8080/api/user/userInfo)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数

|参数名|是否必填|类型|备注|
|---|---|---|---|
|userId|是|String|登录用户Id|

* 输入示例：
```
/api/user/userInfo?userId="123213231231"
```

* 响应参数：

|参数名|类型|备注|
|---|---|---|
|userId|String|用户ID|
|nickName|String|用户昵称|
|userAvatar|String|用户头像|
|userPhoneNum|String|用户手机号|
|userPassword|String|用户密码（加密）|
|userAccount|String|用户账号（加密）|	

## 2、开发者信息
* 简要描述：查询用户信息
* 请求URL：[http://127.0.0.1:8080/api/user/developerInfo ](http://127.0.0.1:8080/api/user/developerInfo)
* 请求方式：GET
* 表头：
```
Content-Type:application/x-www-form-urlencoded;charset=utf-8
```
* 请求参数: 无
* 输入示例：
```
/api/user/developerInfo
```

* 响应参数：userList 开发者多个

|参数名|类型|备注|
|---|---|---|
|developerId|String|开发者ID|
|nickName|String|开发者昵称|
|developerAvatar|String|开发者头像|
|developerPhoneNum|String|开发者手机号|
|developerPassword|String|开发者密码（加密）|
|developerAccount|String|开发者账号（加密）|	