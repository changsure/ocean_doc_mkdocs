#Create and config your back service on oceanclouds

## Register a oceanclouds developer
First, you need to register oceanclouds as a developer, please visit our website to register.

## Create your back service
After register, you can login and visit our [manage console](https://console.oceanclouds.com/) to create your back service. An back service contains below properties.

|Name|Description|
|:-|:-|
|key|The back service key. Unique in oceanclouds. Can't be modified after creation.|
|name|The back service name.|
|type|PUBLIC or RPIVATE. PUBLIC for all app can use, PRIVATE only self app can use. For now only accept PRIVATE.|
|signKey|Security key for decrypt ocean-auth header.|
|messageApiAddress|The api address of accept oceanclouds message.|
|serviceApiAddress|The api address for providing main service to app client.|
|description|Description about your back service.|
|manageAddress|Your back service manage console address, this address should accept a appAuthCode to identify which app is visiting.|
|manageByOpen|True or False, Should this manage console open by a new window in ocean UI.|
|allowUserApi|True or False, is this back service a user auth back service?|


