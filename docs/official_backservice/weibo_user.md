# How to use weibo user service

Weibo user service provide 3rd weibo authenticate service for app. App user can login by weibo user. To use weibo user, you need to follow these steps.

1. Subscribe weibo service on https://console.oceanclouds.com .
1. Create weibo open platform developer from http://open.weibo.com/.
1. Create your weibo connect website app from http://open.weibo.com/connect. Or create your weibo connect mobile app from http://open.weibo.com/development/mobile. 
1. After you create your website(for example), read your app key and app secret on "网站信息-基本信息" page.
1. Config your auth callback page in "接口管理-授权机制", modify "OAuth2.0 授权设置", config your "授权回调页" and "取消授权回调页" to your static html page, this page must be your app page.
1. Open https://weibo.service.oceanclouds.com/config/ to save your weibo app key, app secret, auth callback page address.
1. Develop your app client, if user want to login by weibo, redirect user to *https://api.weibo.com/oauth2/authorize?client_id={WEIBO_APP_KEY}&response_type=code&redirect_uri={WEIBO_CALLBACK_ADDRESS}*,weibo open platform will let user to login by weibo account and redirect user to your call back address with an code parameter called code.
1. In your callback address, please parse code out, and call our back service, the follow-up process of weibo will be done by weibo service. Call [weibo api](http://doc.oceanclouds.com/api/en/backservice/weibo_user.html) and fetch oceanclouds accessCode
1. Use accessCode to [fetch Ocean Context](http://doc.oceanclouds.com/api/en/ocean/api_for_application.html#Fetch App User Ocean Context).
1. If user want to logout, just remove the local storage of ocean context.


You can visit the sample app from http://sample.app.oceanclouds.com/ .
