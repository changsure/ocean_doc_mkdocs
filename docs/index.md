# Welcome to Ocean Clouds Guide Book

## Catalog

* How to build an app
	* [Create and config your app on oceanclouds](dev_app/create_and_config.md)
	* [Develop your app](dev_app/develop.md)
	* [Publish your app](dev_app/publish.md)
* How to build a back service
	* [Create and config your back service on oceanclouds](dev_backservice/create_and_config.md)
	* [Develop your back service](dev_backservice/develop.md)
	* [Publish your back service](dev_backservice/publish.md)
	* [Test your back service on oceanclouds](dev_backservice/test.md)
* Guide book for official back services
	* [How to use standard user service](official_backservice/standard_user.md)
	* [How to use weibo user service](official_backservice/weibo_user.md)
	* [How to use facebook user service](official_backservice/facebook_user.md)
	* [How to use data persistence service](official_backservice/persistence.md)


## Introduce

In normal application, business are processed in back-end service, and we develop different back-end for different applications. This way we think that make application more hard to build, and we believe many application business is too simple to use an individual back-end service. If we move the simple business part to the front client, and call API service like persistence, mail, SMS, etc to finish the service part, we can also finish the application, and finish it quick, easy and dis-coupled.

**Oceanclouds** is an BAAS(Backend as a Service) platform. It helps individial front-end developer to achieve their *NO self mantain* back server application(such as Javascript client, IOS app, Android App, etc..) by using back service APIs which are published by oceanclouds itself or 3rd back-end developers.

The slogan of oceanclouds is *Internet Service Connected*. The key concept is to accomplish application by using batch of exsiting back-end services.

For more specifics design, oceanclouds take charge of these key data:
* App information.
* Back service information.
* App subscribed back service list.
* App's users(Only user information, not including authorize properties). 

And, to be a platform, oceanclouds give the standard of these exchange actions:
* How app subscribe or cancel a back service?
* How app user authenticate?
* How app visit back service?

If you are a **front-end developer**, you want to build an app, please read
*  How to develop an app
*  Official back services

And if you are a **For back-end developer**, you want to deploy a back service for serving apps, please read
*  How to develop a back service

## Platform
Oceanclouds platform play a broker between application and back service. 	
	
	
	                        ┌────────────────────────────┐                                          
	                        │                            │                                          
	         ┌──────────────│    Oceanclouds Platform    │◀────────────┐                            
	         │              │                            │             │                            
	         │              └────────────────────────────┘             │                            
	         │                                                         │                            
	         │                                                         │                            
	         │Fetch Ocean Credential                                   │ Exchange Information       
	         │                                                         │                            
	         │                                                         │                            
	         │                                                         │                            
	         │                                                         │                            
	         ▼                                                         ▼                            
	┌─────────────────┐                                      ┌──────────────────┐                   
	│                 │                                      │                  │                   
	│   App Client    │─────────────────────────────────────▶│   Back Service   │                   
	│                 │       Visit back service API         │                  │                   
	└─────────────────┘                                      └──────────────────┘	                                    

A simple way to explain the process is app client fetch ocean credential from oceanclouds platform first, and use the credential to visit back service. Back service identify app information from the encrypt credentials, and exchange necessary information from oceanclouds to accomplish its service.


##App User Management

**Oceanclouds store app user information but don't own the authenticate function.**

A key function about application is user management like register, login, logout, reset, etc. User is the most important value for application, and this information must share between application and back service for some privilege restriction. At the same time, we don't want our oceanclouds to be too heavy to charge of any business part in application. Another reason is that we also know that 3rd user authenticate (like login by google or facebook) is very popular in application, we don't what the 3rd authenticate to be different in oceanclouds system.

So oceanclouds only store the app user information but don't take resposible for authenticating app user. This part of function is left to back services. Application developer can choose which user management back service he like to act the app user authenticate role, and he also can choose more than one app user back service to support different 3rd user authenticate.

A picture to explain the process.

	┌─────────────────┐         ┌────────────────────────────┐       ┌────────────────────────────┐
	│                 │         │   User Authenticate Back   │       │                            │
	│   App Client    │         │          Service           │       │    Oceanclouds Platform    │
	│                 │         │                            │       │                            │
	└─────────────────┘         └────────────────────────────┘       └────────────────────────────┘
	         │                                 │                                    │              
	         │                                 │                                    │              
	         │                                 │                                    │              
	         │                                 │                                    │              
	         │  1.1 User Register via API      │                                    │              
	         │ ──────────────────────────────▶ │                                    │              
	         │                                 │                                    │              
	         │                                 │    1.2 Save app user information   │              
	         │                                 │ ─────────────────────────────────▶ │              
	         │                                 │                                    │              
	         │  2.1 Login via API              │                                    │              
	         │ ──────────────────────────────▶ │                                    │              
	         │                                 │                                    │              
	         │                                 │   2.2 Fetch app user accessToken   │              
	         │                                 │ ─────────────────────────────────▶ │              
	         │                                 │                                    │              
	         │ 2.3 Return app user accessToken │                                    │              
	         │ ◀────────────────────────────── │                                    │              
	         │                                 │                                    │              
	         │                                 │                                    │              
	         │                                                                      │              
	         │                                                                      │              
	         │  3 Fetch ocean context (App user credential)                         │              
	         │ ──────────────────────────────────────────────────────────────────▶  │              
	         │                                                                      │              
	         │                                                                      │              
	         │                                                                      │              

        
	
1. User Register
	- App client visit back service register API to create a app user.
	- Back service will visit oceanclouds api to [create a user](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Create App User) in the particular app. 
1. Login 
	- App client visit back service API to login by user name/email/cellphone and authenticated by password/cellphone code/QR code.
	- After back service verified user, the back service will visit oceanclouds API to [fetch this user accessToken](http://doc.oceanclouds.com/api/ocean/api_for_backservice/#Get App User Access Token).
	- The accessToken of app user will return to the app client.
1. Fetch Ocean Context
	- App client can use this accessToken to visit oceanclouds API to [fetch **Ocean Context**](http://doc.oceanclouds.com/api/ocean/api_for_app_user/).

Well, we are now ready to talk about **Ocean Context**.

##How app visit back service

**App client use ocean context as app user's credential to visit previlege restrict back service.**

**Ocean Context** contains which back services app already subscribed, the api address and *Ocean-Auth* encrypt string of these subscribed back services. When visit back service, the ocean clouds standard order the app client use *Ocean-Auth* header to prove its identity, so the back service can tell which app and who visit the service.

You may already found and ask *"If user has not login and fetch the accessCode from oceanclouds, how can he visit back service API to register or login?"* The answer is that app should visit oceanclouds to [fetch a anonymous ocean context](http://doc.oceanclouds.com/api/ocean/api_for_app_user/#Fetch Anonymous App User Ocean Context) first. Then he can visit back service, and the back service will identify this visit is from an ANONYMOUS user. 

So the whole process is like this

	┌─────────────────┐         ┌────────────────────────────┐   ┌──────────────┐   ┌────────────┐
	│                 │         │   User Authenticate Back   │   │ Oceanclouds  │   │ Other Back │
	│   App Client    │         │          Service           │   │   Platform   │   │  Service   │
	│                 │         │                            │   │              │   │            │
	└─────────────────┘         └────────────────────────────┘   └──────────────┘   └────────────┘
	         │                                 │                         │                 │      
	         │                                 │                         │                 │      
	         │   1 Fetch ocean context for anonymous                     │                 │      
	         │ ────────────────────────────────┼───────────────────────▶ │                 │      
	         │                                 │                         │                 │      
	         │   2 Login and return app user   │                         │                 │      
	         │   accessToken                   │                         │                 │      
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
		

Now, you already know how the oceanclouds works, let's talk about the details of the development in the next chapter.

##Sample App

We develop an sample app at 

* Sample app address: *http://sampleblog.app.oceanclouds.com/* 
* Sample app code: *https://github.com/changsure/ocean_sample* 

Feel free to visit. 
