## 微信

### 1. 微信公众号
#### 1.1. 微信公众号
* 微信公众号类型
	* 订阅号(个人)
	* 服务号(企业或者组织)
	* 企业号
* 开发模式
	* 编辑模式：可以直接在微信公众号后台设置自动回复、底部自定义菜单、推送文章等
	* 开发模式：将公众号授权给第三方平台，通过调用公众平台提供的接口实现自动回复、自定义菜单、消息推送等功能
* Access Token
	* 公众号接口调用的唯一凭据，2个小时有效期，需要定时刷新
	* 例如微信网页开发或者网址应用微信登录开发
* jssdk - 语音
	* startRecord/ pauseVoice/ stopVoice - 本地id(localId)
	* playVoice直接播放本地id
	* uploadVoice 上传语音，localId换取serverId即media_id，有效期3天
	* downloadVoice 下载语音，serverId获取localId
* jssdk - 多媒体文件
	* chooseImage 选择图片，等到本地图片localIds
	* uploadImage localId换成serverId或者media_id
	* downloadImage 下载图片，serverId获取localId
* 微信支付
	* 打开h5页面，点击下订单按钮，向后端发起下订单请求
	* 后端会调用微信支付系统的下单api，生成预付单(prepay_id)
	* 后端将prepay_id及paySign(appid/ mch_id签名)等返回，前端向JSAPI接口请求支付
	* 微信支付系统验证参数的合法性和授权域的合法性
	* 跳出输入密码的面板，提交支付授权，微信支付系统验证授权之后，返回支付成功页面
	* 异步通知商户支付接口
	* 点击确定跳回h5页面
