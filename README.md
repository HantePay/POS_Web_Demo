"# POS_Web_Demo" H5 接入Hante 收款 支持：信用卡,微信,支付宝 原理:WebView 与 hante App  通过  HanteJSBridge.js 交互：

第一步：加载H5页面<br/>
第二步：HanteApp 监听WebView 加载 H5 完成,注入 HanteJSBridge.js  <br/>
第三步: H5 页面 使用 HanteJSBridge.js  交互 <br/>

使用 HanteJSBridge.js 发送消息(JSON格式) <br/>

3.1发起交易 <br/>
  window.HanteWebApi.sendMessage({
    'type': 'transaction',
    'merchantNo':'1025258896',//商户号
    'orderNo':’1665461679816464’,//订单号
    'transType':'SALE',//SALE 消费 AUTH 预授权
    'amount':1,//单位美分
    'tipAmount':1,//单位美分
    });
    <br/>
3.2发起退款 <br/>
window.HanteWebApi.sendMessage({
    'type': 'refund',
    'merchantNo':'1025258896',
    'transactionId':’2023050815651846454’,//hante 交易流水号
     'amount':1,//退款金额 单位美分
});
<br/>
3.3待授权订单作废 <br/>
window.HanteWebApi.sendMessage({
    'type': 'Void',
    'merchantNo':'1025258896',
    'transactionId':’2023050815651846454’,//hante 交易流水号
});
<br/>
3.4待授权订单授权 <br/>
window.HanteWebApi.sendMessage({
    'type': 'Capture',
    'merchantNo':'1025258896',
    'transactionId':’2023050815651846454’,//hante 交易流水号
});
<br/>
 3.5查询订单<br/>
 window.HanteWebApi.sendMessage({
	'type':’queryOrder’,
	'merchantNo':'1025258896',
	'transactionId':’2023050815651846454’,//hante 交易流水号
});

    
 3.6使用 HanteJSBridge.js 监听结果 <br/>

    function connectHanteWebApi(callback) {
            if (window.HanteWebApi && HanteWebApi.inited) {
                callback(HanteWebApi)
            } else {
                document.addEventListener(
                    'HanteWebApiReady', function() {
                        callback(HanteWebApi)
                    },false
                );
            }
        };

 connectHanteWebApi(function(bridge) {
 
            bridge.init(function(message, responseCallback) {
                //初始化
            });

            bridge.registerHandler("receiveMessage", function(data, responseCallback) {
		//接收消息 data
  		var response=JSON.parse(data);
    		var type=response['type'];
      		//解析交易结果
      		if('transaction'==type){
			var resultCode=response['resultCode'];//SUCCESS 成功  FAIL失败
 			if('SUCCESS'==resultCode){
    				//交易成功
				//获取 hante 交易流水号
				var transactionId=response['transactionId'];
			}else{
   				//失败消息
   				var resultMsg=response['resultMsg'];
       
			}
		}
            });
   });
