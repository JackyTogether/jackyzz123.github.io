---
layout: post
title: RESTful API
---

根据method不同做不同操作

```python
# 基于FBV
def order(request):
	if request.method == 'GET':
		return HttpResponse('获取订单')
	elif request.method == 'POST':
		return HttpResponse('创建订单')
# 基于CBV
class OrderView(View):
	def get(self,request,*args,**kwargs):
		return HttpRe sponse('获取订单')
	def post(self,request,*args,**kwargs):
		return HttpResponse('创建订单')
```

### RESTful API设计

- 协议：总是使用HTTPs协议
- 域名：两种表现URL是API接口的方式
    - www.luffycity.com变成api.luffycity.com，得解决跨域问题（一次option预检，一次post，jsonp也可解决跨域）
    - （优选）www.luffycity.com变成www.luffycity.com/api/
- 版本：
    - （优选）URL如https://api.example.com/v1/
    - 请求头，让服务端知道版本
- 路径：视网络上任何东西都是资源，推荐都写名词（可复数），https://api.example.com/v1/名词
- method：GET、POST、PUT（全部更新）、PATCH（局部更新）、DELETE
- 过滤：https://api.example.com/v1/order/?status=1
- 状态码：状态码status（浏览器定义，不写也可以）与code（自定义）结合解决自定义情况
- 错误处理：如果状态码是4xx，应该向用户返回出错信息。一般来说，返回的信息中将error作为键名，出错信息作为键值即可`{error: "Invalid API key"}`
- 返回结果：针对不同操作，服务器向用户返回的结果应该符合以下规范

    > GET /collection：返回资源对象的列表（数组） 比如/order/
    GET /collection/resource：返回单个资源对象  比如/order/1/
    POST /collection：返回新生成的资源对象
    PUT /collection/resource：返回完整的资源对象
    PATCH /collection/resource：返回完整的资源对象
    DELETE /collection/resource：返回一个空文档

- RESTful API最好做到Hypermedia，即返回结果中提供链接，连向其他API方法，使得用户不查文档，也知道下一步应该做什么

    ```python
    # /order/
    [
    	{
    		id:1,
    		name:'苹果',
    		url:http://www.oldboyedu.com/1/
    	},
    ]
    ```