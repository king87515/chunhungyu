---
title: HTTPS Certbot 申請憑證
tags:
  - web
  - https
categories:
  - website網頁設計
---

categories: `website網頁設計`
tags: `web` `https`

# Certbot
- cerbot vedio
    - https://www.youtube.com/watch?v=snXIDRrZMpM
- certbot blog
    - https://blog.miniasp.com/post/2021/02/11/Create-SSL-TLS-certificates-from-LetsEncrypt-using-Certbot
- certbot command
    - https://hackmd.io/@c36ICNyhQE6-iTXKxoIocg/H1xmt263v
<!--more-->
- nodejs certbot pem setting
    - https://qops.blogspot.com/2019/02/certbot-letsencrypt-nodeexpress-https.html
- xampp certbot pem setting
    - https://pixlilen.pixnet.net/blog/post/1494496-%E5%AE%89%E8%A3%9Dlet%27s-encrypt%E5%85%8D%E8%B2%BB%E6%86%91%E8%AD%89%E5%9C%A8xampp%E4%B8%8A_2019?utm_source=PIXNET&utm_medium=Hashtag_article

* 申請流程
```powershell=
# create acount
certbot certonly --manual -m [mail] -d [domain name]
# 申請 Let's Encrypt 免費憑證，通過驗證項目才能「證明」你真的擁有該域名！
certbot certonly --manual --preferred-challenges http -m ntouigl@gmail.com -d igl.cse.ntou.edu.tw
# 刪除憑證
certbot delete --cert-name igl.cse.ntou.edu.tw
```

```ps=
# 顯示目前 certbot 管理的憑證
certbot certificates
```

* Xampp hHttpd-ssl.conf
```config
SSLCertificateFile "C:/Certbot/live/[domain name]/cert.pem"
SSLCertificateKeyFile "C:/Certbot/live/[domain name]/privkey.pem"
SSLCertificateChainFile "C:/Certbot/live/[domain name]/chain.pem"
SSLCACertificateFile "C:/Certbot/live/[domain name]/fullchain.pem"
```

* Xampp httpd.conf
    * 建議將`#NameVirtualHost *:443`寫在this block之下方，新版的xampp應該可以直接放在檔案最下方
    ```config
    DocumentRoot "${LSSROOT}/public"
    <Directory "${LSSROOT}/public">
        #省略
    </Directory>
    ```
    
```config
#NameVirtualHost *:443
<VirtualHost *:443>
ServerName [domainName]
DocumentRoot "[rootPath]"
SSLEngine on
SSLCertificateFile "C:/Certbot/live/[domain name]/cert.pem"
SSLCertificateKeyFile "C:/Certbot/live/[domain name]/privkey.pem"
SSLCertificateChainFile "C:/Certbot/live/[domain name]/chain.pem"
SSLCACertificateFile "C:/Certbot/live/[domain name]/fullchain.pem"
</VirtualHost>
```
* ps: `DocumentRoot`
    * Lab web : `"${LSSROOT}/public"`
    * cgw : `"C:/xampp/htdocs/dashboard/CGW2021"`
    * heping : `"C:/xampp/htdocs"`

## 申請頻率限制說明
- https://letsencrypt.org/zh-tw/docs/rate-limits/
- 取得你的註冊網域所申請過的憑證列表 https://crt.sh/


## Update
```powershell=
#執行前先關閉專案
certbot certonly --standalone --renew-by-default -d [domain_name]
```
<!-- 
certbot certonly --standalone --renew-by-default -d cgw2021.cse.ntou.edu.tw
certbot certonly --standalone --renew-by-default -d igl.cse.ntou.edu.tw 
-->