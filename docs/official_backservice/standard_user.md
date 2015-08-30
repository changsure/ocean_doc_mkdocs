#How to use standard user service

Standard user service provide user register, login etc service for ocean applications. The app user will store both in this service and oceanclouds, authenticate only happend here.

When use this service, you need to follow these steps:

1. Subscribe this back service in ocean manage console.
1. Develop your app client, to use [API](http://doc.oceanclouds.com/api/en/backservice/standard_user.html) to register and login.
1. Develop your app client, after app user login, you got a accessCode for this app user, refresh the client ocean context via [ocean API by accessCode](http://doc.oceanclouds.com/api/en/ocean/api_for_application.html#Fetch App User Ocean Context).
1. If user want to logout, just remove the local storage of ocean context.

You can visit the sample app from http://sample.app.oceanclouds.com/ .

