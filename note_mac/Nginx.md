## Nginx 配合 Let's Encrypt SSL/TLS 开启HTTPS

#### Ubuntu 系统

1. 下载 Let's Encrypt 客户端

   1. 添加`certbot`仓库:

      ```shell
      add-apt-repository ppa:certbot/certbot
      ```

   2. 安装`certbot`:

      ```shell
      apt-get update
      apt-get install python-certbot-nginx
      ```

   现在，Let's Encrypt 就可以使用了。

2. 配置`NGINX`

   certbot 可以自动查找NGINX的配置文件中的`server`语句块中的`server_name`语句包含的域名来生成SSL/TLS证书，以下例子中，假设域名为`www.example.com`

   1. 假设你的NGINX刚刚安装，没有进行配置，你需要在`/etc/nginx/conf.d`目录下创建一个配置文件`www.example.com.conf`.

   2. 通过`server_name`命令，指明你的域名。  

      ```shell
      server {
          listen 80 default_server;
          listen [::]:80 default_server;
          root /var/www/html;
          server_name example.com www.example.com;
      }
      ```

   3. 保存文件，并运行以下命令验证配置文件，然后重启NGINX

      ```shell
      nginx -t && nginx -s reload
      ```

3. 获取SSL/TLS证书

   1. 运行以下命令生成证书

      ```shell
      sudo certbot --nginx -d example.com -d www.example.com
      ```

   2. 根据提示配置你的HTTPS设置，它需要的邮箱地址并同意他们的协议

      当配置完成后，NGINX会重载新的设置。`certbot`会提示你证书生成成功，并指出证书在服务器的位置。

      ```shell
      Congratulations! You have successfully enabled https://example.com and https://www.example.com 
      
      -------------------------------------------------------------------------------------
      IMPORTANT NOTES: 
      
      Congratulations! Your certificate and chain have been saved at: 
      /etc/letsencrypt/live/example.com/fullchain.pem 
      Your key file has been saved at: 
      /etc/letsencrypt/live/example.com//privkey.pem
      Your cert will expire on 2017-12-12.
      ```

      > **Note**:证书有效期为90天，我们可以配置自动定期生成证书。

      如果你注意到NGINX配置，你会发现`certbot`已经修改了它。

      ```shell
      server {
          listen 80 default_server;
          listen [::]:80 default_server;
          root /var/www/html;
          server_name  example.com www.example.com;
      
          listen 443 ssl; # managed by Certbot
      
          # RSA certificate
          ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem; # managed by Certbot
          ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem; # managed by Certbot
      
          include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
      
          # Redirect non-https traffic to https
          if ($scheme != "https") {
              return 301 https://$host$request_uri;
          } # managed by Certbot
      }
      ```

4. 自动重新生成Let's Encrypto证书。

   我们可以添加一个`cron`任务到你的`crontab`文件来完成自动化生成。

   1. 打开`crontab`文件

      ```shell
      crontab -e
      ```
      
   2. 添加以下命令至文件中，这个例子是每天12点运行`/usr/bin/certbot renew --quiet`命令，该命令的作用是如果证书30天内过期则重新生成，`—quit`表示不产生输出。
   
      ```shell
      0 12 * * * /usr/bin/certbot renew --quiet
      ```
   
   3. 保存并关闭文件。所有安装的证书将会自动重新生成，并自动重载生效。





#### 80 端口被禁时，手动生成证书

通过使用dns txt记录来完成验证

命令

```
certbot certonly --manual --preferred-challenge dns -d bzdmx.goodgoodstudy.gq
```

然后根据提示添加DNS TXT记录，添加后命令行回车确认即可生成证书。

```
root@ecs-sn3-medium-2-linux-20191218140512:~# certbot certonly --manual --preferred-challenge dns -d bzdmx.goodgoodstudy.gq
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator manual, Installer None
Cert is due for renewal, auto-renewing...
Renewing an existing certificate
Performing the following challenges:
dns-01 challenge for bzdmx.goodgoodstudy.gq

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
NOTE: The IP of this machine will be publicly logged as having requested this
certificate. If you're running certbot in manual mode on a machine that is not
your server, please ensure you're okay with that.

Are you OK with your IP being logged?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: y

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name
_acme-challenge.bzdmx.goodgoodstudy.gq with the following value:

iLBaJNHW5uh5ZwBprSMmHjTkzC401XfdiskUTA7Mwjw

Before continuing, verify the record is deployed.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Press Enter to Continue
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/bzdmx.goodgoodstudy.gq/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/bzdmx.goodgoodstudy.gq/privkey.pem
   Your cert will expire on 2020-07-02. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

