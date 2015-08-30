#Publish your back service

## Find a Cloud Hosting place for your back service
You may provide your back service from below famous cloud hosting company
* [AWS](http://aws.amazon.com/ec2/)
* [linode](https://www.linode.com/)
* [QingCloud](https://www.qingcloud.com/)
* [AliYun](http://www.aliyun.com/)


## Provide API via HTTPS

Oceanclouds demand it's back service **MUST** use HTTPS to provide it's service. The certificate should be signed by a known Certificate Authority (Otherwise browser will block the ajax https call).

There are some free sign authority.
* https://buy.wosign.com/free
* https://www.startssl.com/?app=1

If you are using nginx for your back-end proxy, you can read the manual from http://nginx.org/en/docs/http/configuring_https_servers.html .
