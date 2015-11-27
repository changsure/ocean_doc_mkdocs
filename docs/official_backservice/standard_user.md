#How to use standard user service

Standard user service provide user register, login etc service for ocean applications. The app user will store both in this service and oceanclouds, authenticate only happend here.

When use this service, you need to follow these steps:

1. Subscribe this back service in ocean manage console.
1. Config your app email sending service information by editing it in ocean clouds console. For now we only support mailgun(http://www.mailgun.com/) as your mail sending services. If you don't want to use mail sending service, you can just turn the mail sending service off, and when you do that, your app's end user won't receive welcome mail and you can not use reset password service, too.
1. Develop your app client, to use [API](http://doc.oceanclouds.com/api/backservice/standard_user/) to register and login.
1. Develop your app client, after app user login, you got a accessCode for this app user, refresh the client ocean context via [fetch Ocean Context](http://doc.oceanclouds.com/api/ocean/api_for_app_user/).
1. If user want to logout, just remove the local storage of ocean context.

You can visit the sample app from http://sample.app.oceanclouds.com/ .

