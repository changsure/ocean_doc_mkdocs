#Develop your app

The concept of oceanclouds is to share back-end services via https, so we don't limit our app client. The only requirement is app can use http for communication.

If you havn't read [oceanclouds introduce](../index.md), please read it first.

## How does it work
Basicly, app accomplish its business by combining backservice. It call back service to store it's data, it call back service to send mail, it call back service to login, etc. The capibility of an app is decide by the back service it subscribe.

When visit back service, an ocean header is needed to provide the app user information, these information is fetched from oceanclouds.

	┌─────────────────┐         ┌────────────────────────────┐   ┌──────────────┐   ┌────────────┐
	│                 │         │                            │   │ Oceanclouds  │   │ Other Back │
	│   App Client    │         │   User auth Back Service   │   │   Platform   │   │  Service   │
	│                 │         │                            │   │              │   │            │
	└─────────────────┘         └────────────────────────────┘   └──────────────┘   └────────────┘
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	         │   1 Fetch ocean context for anonymous                     │                 │      
	         │ ────────────────────────────────┼───────────────────────▶ │                 │      
	         │                                 │                         │                 │      
	         │   2 Login and return app user   │                         │                 │      
	         │   accessCode                    │                         │                 │      
	         │ ─────────────────────────────▶  │                         │                 │      
	         │                                 │                         │                 │      
	         │   3 Fetch ocean context by accessToken                    │                 │      
	         │ ────────────────────────────────┼───────────────────────▶ │                 │      
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	         │   4 Visit back service APIs     │                         │                 │      
	         │ ────────────────────────────────┼─────────────────────────┼───────────────▶ │      
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	

## Fetch Ocean Context
We already know that ocean context contains the credential for visiting back service. If user has not login, app should call oceanclouds API to [fetch an Anonymous ocean context](http://doc.oceanclouds.com/api/ocean/api_for_app_user/#Fetch Anonymous App User Ocean Context). If user login, app should call the API again to [fetch a new ocean context](http://doc.oceanclouds.com/api/ocean/api_for_app_user/#Fetch Login App User Ocean Context) with app user accessToken credential.

Because oceanclouds does not take responsible for app user authentication, app developer should find an user authenticate back service himself in oceanclouds, and read the back service doc to know details about how to register/login/etc. Oceanclouds provide a [standard user service](../official_backservice/standard_user.md) , [Weibo user service](../official_backservice/weibo_user.md) , [Facebook user service](../official_backservice/facebook_user.md) .

## Visit back service API

A back service provide service by it's API, app should assemble an ocean header to visit back service. Like `Ocean-Auth:{oceanAuthHeader value from ocean context}` , then the back service will know which app is calling api and which app user is already login.

You can find some sample from http://doc.oceanclouds.com/api/backservice/data_persistence/

## Q&A

### How to visit back service from javascript in browser

We use CORS(Cross-origin resource sharing ) to share visit from browser to back service. Every back service already allow to visit from __\*__ origin.

If you are using jQuery to visit an ajax call, please make sure the options.crossDomain is set to true. In coffeescript is like 

```coffeescript
  $.ajaxPrefilter((options, originalOptions, jqXHR)->
    options.crossDomain =
      crossDomain: true
  )
```

Most browser will first try to visit API address by an **OPTIONS** method, after the **OPTIONS**  call return an batch of Access-Control-Allow header accept, it will start the true call of the API.

### How to visit HTTPS back service
Generally, our back service HTTPS SSL Certificate can be verified by browser, it won't be a problem to visit from a HTTP site or client.












