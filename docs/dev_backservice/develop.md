#Develop your back service

Develop your back-end server to provide service for app should follow some standards create by oceanclouds.

1. If your back service like to receive ocean message, please config a message address to receive ocean message. For now, when a app subscribe or cancel your service, you will receive a message with message id in it. You can retrieve and deal the message via [ocean message API](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#message).
1. When app call back service api, back service should validate the Ocean-Auth header and decrypt the header to parse which app and who is visiting.

At the end, back service can have a own manage console for app developer to config some information. When app visit your manage console, an appAuthCode is send to your manage console, back service should visit ocean API [get app by auth code](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Get App By Auth Code) to retrieve app information, so you know which app is visiting your manage console.


## Exchange starndard when back service call oceanclouds API

There are self defined **Header** in the request.

|Header Name|Description|
|:-|:-|
|content-type|application/json|
|AccessToken|back service access token, you can find it in ocean manage console.|


## Handle Oceanclouds Message

Message is the information what ocean clouds want to transfer to back service.

### Oceanclouds calls
Oceanclouds will **POST** call back service message api to send a message notice. The request structure is  

```json
{
	"id":"{messageId}"
}
```

### Call Oceanclouds to deal with message

After you receive the message id, you can follow the steps to handle it

1. Visit ocean [get message api](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Get Message) to get message detail
1. Handle the message in your back service system.
1. Visit ocean [resolve message api](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Resolve Message) to resolve this message in ocean system. 

If you system is down or don't have address for receiving ocean message, you can visit [get all messages api](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Get Messages) to deal with message in cron job.


Message Type Contains
* app_subscribe_service
* app_cancel_service

**APP_SUBSCRIBE_BACKSERVICE**
Will send this message when app subscribe your back service

The message structure is 
```json
{
  "success": true,
  "entity": {
    "_id": "55b89ba34df03bab3307c10f",
    "backServiceId": "55b89b274df03bab3307c10c",
    "type": "app_subscribe_service",
    "content": "{\"appId\":\"55b88eef4df03bab3307c0ff\",\"appKey\":\"test_app\",\"appSubscriptionId\":\"55b89ba34df03bab3307c10e\"}",
    "status": "init",
    "__v": 0
  }
}
```

**APP_CANCEL_BACKSERVICE**
Will send this message when app subscribe your back service

The message structure is 
```json
{
  "success": true,
  "entity": {
    "_id": "55b8963d4df03bab3307c105",
    "backServiceId": "55b74c844df03bab3307c0bf",
    "type": "app_cancel_service",
    "content": "{\"appId\":\"55b88eef4df03bab3307c0ff\",\"appKey\":\"test_app\",\"appSubscriptionId\":\"55b89ba34df03bab3307c10e\"}",
    "status": "init",
    "__v": 0
  }
}
```

## When you need to authenticate App User
If your back service is a app user authentication service, you need to exchange app user information with oceanclouds. The standard process is like

1. [Create app user](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Create App User) info into oceanclouds when app user register.
1. [Update app user](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Update App User) info into oceanclouds when app user update their infomation.
1. [Get app user access code](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Get App User Access Token) from oceanclouds, and return it to the app end user.


## Build a Manage Console

When an app subscribe your back service, you may want the app own developer to config some properties first, so you can provide your service. This demand your back service manage console can know if the user now has the APP.

The process is

1. Developer visit your manage console address (You save it in your back service save api). The address should contains a appAuthCode.

1. Your back service use this appAuthCode to get app information from oceanclouds. So you know which app is configuring.

1. Set cookies or other methods to let client remember this app is configuring.

In this way you can let app own developer to config the app's privilege or it's extends information. You can see **official back service** for some sample.


## Exchange starndard from app client call back service

App client request with an Ocean-Auth header for back service to validate. Before encrypt, Ocean-Auth is a json string like

```json
{
	"appKey":"{app key}",
	"isClientLogin":"{is app user login}",
	"appUserAccountId":"{app user ocean account id}"
	"appUserAccount":"{app user account name}",
	"appUserName":"{app user display name}",
	"authBackServiceKey":"this app user is from which back service",
	"expireTime":"{expire time for this ocean-auth header(Unix time millisecond)}"
}
```

The Ocean-Auth json string is encrypt by AES(aes-128-ecb) algorithm, password use back service securityKey configed in oceanclouds, input json string is 'utf8' and output is 'hex'. The encryption process in nodejs by coffeescript is like

```coffee

cipher = crypto.createCipher('aes-128-ecb', config.backServiceSecurityKey)
encryptedOceanAuth = cipher.update(oceanAuthJsonString, 'utf8', 'hex') + cipher.final('hex')

```

The description process in nodejs by coffeescript is like

```coffee
cipher = crypto.createDecipher('aes-128-ecb', config.oceanSecurity)
oceanAuthJsonString = cipher.update(encryptedOceanAuth, 'hex', 'utf8') + cipher.final('utf8')
```

When you get *oceanAuthJsonString*, it's easy to parse and identify which app and which app user is visiting.


