# 安装客户端SDK2.0版本 {#task_1614417 .task}

通过小程序云SDK，您可以在小程序中直接访问小程序云Serverless服务。2.0版本支持支付宝和微信小程序，在安装SDK后，您需要调用user.authorize进行授权。

## 在支付宝小程序中使用SDK {#section_kdy_jvc_h36 .section}

完成以下操作，在支付宝小程序中使用SDK：

1.  在支付宝小程序项目的根目录执行以下命令安装SDK。 

    ``` {#codeblock_ngn_0es_fmj}
    npm install @alicloud/mpserverless-sdk@2.0.0 --save
    ```

2.  在小程序项目中添加以下代码，初始化SDK。 

    ``` {#codeblock_lx1_qi7_gz4}
    import MPServerless from '@alicloud/mpserverless-sdk';
    
    const serverless = new MPServerless({
      uploadFile: my.uploadFile,
      request: my.request, 
      getAuthCode: my.getAuthCode || function (options) {
        // 通过授权页面拿到授权 code
        const code = getAuthCodeByAuthorizePage(options);
        options.complete({ authCode: code });
      },
    }, {
      appId: '1234456789', // 小程序应用标识
      spaceId: 'db4dd657-7041-470a-90xxxxx', // 服务空间标识
      clientSecret: '6c3c86xxxx6', // 服务空间 secret key
      endpoint: 'https://endpoint'， // 服务空间地址，从小程序Serverless控制台处获得
    });
    ```

    其中：

    -   appId是小程序的ID。您可以在[支付宝小程序开放平台](https://openhome.alipay.com/mini/dev/list)获取小程序的App ID。
    -   spaceIdclientSecret和endpoint在小程序Serverless控制台创建服务空间后可以获得。详情参见[创建服务空间](cn.zh-CN/小程序Serverless开发指南/服务空间管理/创建服务空间.md#)。
3.  调用user.authorize方法完成授权。 

    ``` {#codeblock_4v6_kv5_nvq}
    const res = await mpServerless.user.authorize({
      authProvider: 'alipay_openapi'
    });
    if (res.success) {
      console.log('授权成功');
    }
    ```


## 在微信小程序中使用小程序云SDK {#section_79b_gay_ywk .section}

完成以下操作，在微信小程序中使用小程序云SDK：

1.  单击[这里](https://mpserverless-sdk.oss-cn-shanghai.aliyuncs.com/2.0.0-alpha.3/mpserverless.js)下载SDK文件。
2.  将下载后的mpserverless.js文件添加到小程序项目目录下，然后在app.js或其他需要的页面中引入SDK文件。 

    ``` {#codeblock_mbw_bpn_p5d}
    //app.js
    const MPServerless = require('/sdk/mpserverless.js');
    
    const mpServerless = new MPServerless({
      uploadFile: wx.uploadFile,
      request: wx.request,
      getAuthCode: wx.login,
      getFileInfo: wx.getFileInfo,
      getImageInfo: wx.getImageInfo,
    }, {
      appId: 'appId', // 小程序应用标识
      spaceId: '', // 服务空间标识
      clientSecret: '', // 服务空间 secret key
      endpoint: '', // 服务空间地址，从小程序 serverless 控制台处获得
    });
    ```

    其中：

    -   appId是小程序的ID。您可以在[微信公众平台](https://mp.weixin.qq.com)获取小程序的App ID。
    -   spaceIdclientSecret和endpoint在小程序Serverless控制台创建服务空间后可以获得。详情参见[创建服务空间](cn.zh-CN/小程序Serverless开发指南/服务空间管理/创建服务空间.md#)。
3.  调用user.authorize方法完成授权 

    ``` {#codeblock_3xr_ve7_sz4}
    const res = await mpServerless.user.authorize({
      authProvider: 'wechat_openapi'
    });
    if (res.success) {
      console.log('授权成功');
    }
    ```


