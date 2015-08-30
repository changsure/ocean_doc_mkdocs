# How to use facebook user service

Facebook user service provide 3rd facebook authenticate service for app. App end user can login by facebook user. To use this back service, you need to follow these steps.

1. Subscribe facebook service on https://console.oceanclouds.com .
1. Create facebook open platform developer from https://developers.facebook.com/apps/, and add your app. 
1. After you create your website(for example), read your app key and app secret on dashborad page.
1. Config your website site url in setting-basic page. And add "Valid OAuth redirect URIs" in setting-Advance page.
1. Open https://fb.service.oceanclouds.com/config/ to save your facebook app key, app secret, auth callback page address.
1. Develop your app client, if user want to login by facebook, redirect user to *https://www.facebook.com/dialog/oauth?display=popup&client_id={FACEBOOK_APP_KEY}&response_type=code&redirect_uri={FACEBOOK_CALLBACK_ADDRESS}*,facebook open platform will let user to login by facebook account and redirect user to your call back address with an code parameter called code.
1. In your callback address, please parse code out, and call our back service, the follow-up process of facebook will be done by facebook service. Call [facebook api](http://doc.oceanclouds.com/api/en/backservice/facebook_user.html) and fetch oceanclouds accessCode
1. Use accessCode to [fetch Ocean Context](http://doc.oceanclouds.com/api/en/ocean/api_for_application.html#Fetch App User Ocean Context).
1. If user want to logout, just remove the local storage of ocean context.


