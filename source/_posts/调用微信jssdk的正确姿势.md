---
title: 调用微信jssdk的正确姿势
date: 2016-08-09 22:50:09
tags: 微信jssdk
---

# 姿势步骤
## 引入js
//todo截图
## wx.config()
//todo截图
## wx.ready()
//todo截图
## wx.error()
//todo截图
<!-- more -->
# 引入js
html模板里加上
```
<script src="http://res.wx.qq.com/open/js/jweixin-1.1.0.js"></script>
```
//todo template.vm里的js截图
# wx.config()
```
wx.config({
    debug: true, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: '', // 必填，企业号的唯一标识，此处填写企业号corpid
    timestamp: , // 必填，生成签名的时间戳
    nonceStr: '', // 必填，生成签名的随机串
    signature: '',// 必填，签名，见下面
    jsApiList: [] // 必填，需要使用的JS接口列表
});
```
## 签名（调用sdk最复杂也是最核心的部分）
### 设置可信域名
1.先在微信企业号后台配置可信域名，只有在可信域名下的页面才有资格调用微信jssdk
![](http://ob3wg7deo.bkt.clouddn.com/QQ20160809-0@2x.png)

2.可信域名必须是备案过的域名，须写上端口号（不写则默认80端口）

3.本地调试时可以随便设置一个备过案的域名(www.baidu.com)，然后用gas mask设置代理

### 获取appId和appsecret
![获取appId和appsecret](http://ob3wg7deo.bkt.clouddn.com/QQ20160809-1@2x.png)
#### 和服务号不同点
##### 企业号里叫作CorpID和Secret，服务号叫作appId和appsecret
##### 企业号里每个管理组都有独立的secret，服务号只有1个appsecret

### 获取AccessToken(get方式)
>https://qyapi.weixin.qq.com/cgi-bin/gettoken?corpid=id&corpsecret=secrect

```
{
   "access_token": "accesstoken000001",
   "expires_in": 7200
}```

### 获取jsapi_ticket(get方式)

>https://qyapi.weixin.qq.com/cgi-bin/get_jsapi_ticket?access_token=ACCESS_TOKE

```
{
"errcode":0,
"errmsg":"ok",
"ticket":"bxLdikRXVbTPdHSM05e5u5sUoXNKd8-41ZO3MhKoyN5OfkWITDGgnr2fwJ0m9E8NYzWKVZvdVtaUgWvsdshFKA",
"expires_in":7200
}```
### sha1签名
#### 签名所需元素
```
noncestr=Wm3WZYTPz0wzccnW
```
```
jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg
```
```
timestamp=1414587457
```
```
url=http://mp.weixin.qq.com //需调用jssdk页面的url
```
#### 按字典序排列拼接成字符串
```
jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg&noncestr=Wm3WZYTPz0wzccnW&timestamp=1414587457&url=http://mp.weixin.qq.com
```

#### 进行sha1签名
得到
```
0f9de62fce790f9a083d5c99e95740ceb90c27ed
```

#### 附赠python手动sha1签名代码
```
import hashlib
a = 'jsapi_ticket=sM4AOVdWfPE4DxkXGEs8VMCPGGVi4C3VM0P37wVUCFvkVAy_90u5h9nbSlYy3-Sl-HhTdfl2fzFy1AOcHKP7qg&noncestr=Wm3WZYTPz0wzccnW&timestamp=1414587457&url=http://mp.weixin.qq.com'
print hashlib.sha1(a).hexdigest()
```

## jsApiList
所有需要用到的api接口都要写在这里！不写就还是不给用~

# wx.ready()
```
wx.ready(function(){
    // config信息验证后会执行ready方法，所有接口调用都必须在config接口获得结果之后，config是一个客户端的异步操作，所以如果需要在页面加载时就调用相关接口，则须把相关接口放在ready函数中调用来确保正确执行。对于用户触发时才调用的接口，则可以直接调用，不需要放在ready函数中。
});
```

# wx.error()
```
wx.error(function(res){
    // config信息验证失败会执行error函数，如签名过期导致验证失败，具体错误信息可以打开config的debug模式查看，也可以在返回的res参数中查看，对于SPA可以在这里更新签名。
});
```
