"# POS_Web_Demo" H5 接入Hante 收款 支持：信用卡,微信,支付宝 原理:WebView 与 hante App  通过  HanteJSBridge.js 交互：

第一步：加载H5页面<br/>
第二步：HanteApp 监听WebView 加载 H5 完成,注入 HanteJSBridge.js  <br/>
第三步: H5 页面 使用 HanteJSBridge.js  交互 <br/>

使用 HanteJSBridge.js 发送消息(JSON格式) <br/>

  window.HanteWebApi.sendMessage({
    'type': 'transaction',
    'merchantNo':'1025258896',
    'orderNo':’2023050815651846454’,
    'transType':'Sale'
    });

    使用 HanteJSBridge.js 接收消息 <br/>

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
            });
   });
