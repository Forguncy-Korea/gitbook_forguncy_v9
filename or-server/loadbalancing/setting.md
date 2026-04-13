# 서버부하분산 모드 설정

로드 밸런싱을 지원하며, 로드 밸런싱을 활성화하면 높은 동시성을 처리할 수 있는 포건시의 능력이 향상될 수 있습니다.

부하 분산 기능을 사용하려면 사전에 고정 사용자 또는 무제한 동시 사용자로 사용자 라이선스를 등록해야 합니다.



![](https://help.grapecity.com.cn/download/thumbnails/80954068/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1673923298000\&api=v2)  서버관리자에 로그인하고 "설정 -> 서버부하 분산 모드 설정" 을선택하여 부하 분산 구성 페이지로 들어갑니다.

"활성화" 앞의 체크 박스를 체크한 후 로드 밸런싱 구성을 수행합니다.

<figure><img src="../../.gitbook/assets/image (399).png" alt=""><figcaption></figcaption></figure>

### 데이터베이스 설정

&#x20;MySQL 또는 SQL Server로 설정하고 데이터베이스 연결 문자열을 구성합니다.

### **레디스 설정**

Redis 서비스 주소와 비밀번호를 설정합니다.

### **기타 설정**

애플리케이션 메타데이터 및 애플리케이션 구성과 같은 정보를 저장하도록 공유 스토리지 경로를 설정합니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954068/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923298000\&api=v2)  앱을 배포합니다. 애플리케이션을 모든 서버에 배포합니다. Linux 서버에 배포하는 경우 포트 번호가 사용되지 않는지 확인하세요.

배포된 애플리케이션에는 기본 제공 데이터 테이블이 있을 수 없습니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954068/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1673923298000\&api=v2)  Nginx가 설치된 Linux 서버에서 Nginx를 구성합니다.

**Debian 기반(운영 체제는 Ubuntu)**

아래 예에는 5개의 Mosaic 서버(10.32.7.193, 10.32.7.186, 10.32.7.194, 10.32.7.187, 10.32.7.106)가 있으며 여기서 myapp 애플리케이션은 포트 9527에 배포됩니다.

1\. 다음 명령을 사용하여 새 파일을 만듭니다.

**sudo vi /etc/nginx/sites-available/myapp.conf**

2\. 생성된 "myapp.conf"에 다음 콘텐츠를 붙여 넣습니다.

여러 애플리케이션에 대한 로드 밸런싱을 구성하려면 각 애플리케이션의 .conf 파일에 있는 upstream 이름이 고유해야 합니다. 그리고 아래 예에서 포트 80과 같이 포트 번호가 점유되지 않았는지 확인하십시오.



```
upstream loadbalance {
  server 10.32.7.193:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.186:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.194:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.187:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.106:9527 weight=1 max_fails=1 fail_timeout=30s;
}
 
 
server {
  listen 80;
  listen [::]:80;
  server_name myapp;
  gzip  on;
  client_max_body_size 2048m;
  location / {
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
        add_header X-Upstream $upstream_addr;
        proxy_pass http://loadbalance;
        proxy_http_version 1.1;
  }
}
```

3\. "sites-enabled" 디렉토리에 대한 링크를 생성하여 시작 시 Nginx에서 읽는 파일을 활성화합니다.

**sudo ln -s /etc/nginx/sites-available/myapp.conf /etc/nginx/sites-enabled/**

4\. 테스트 파일에서 구문 오류를 확인합니다.

**sudo nginx -t**

5\. Nginx를 다시 시작합니다.

**sudo systemctl restart nginx**

6\. Nginx를 다시 시작한 후 Nginx를 통해 애플리케이션에 액세스할 수 있습니다. [http:// _nginx\_server\_ip_ / _appname_](http://nginx_server_ip/appname)

**RPM 기반(운영체제는 CentOS, RedHat 7.6)**

아래 예에는 5개의 Mosaic 서버(10.32.7.193, 10.32.7.186, 10.32.7.194, 10.32.7.187, 10.32.7.106)가 있으며 여기서 myapp 애플리케이션은 포트 9527에 배포됩니다.

1\. 다음 명령을 사용하여 새 파일을 만듭니다.

**sudo vi /etc/nginx/conf.d/myapp.conf**<br>

2\. 생성된 "myapp.conf"에 다음 코드를 붙여넣습니다.



```
upstream loadblance {
  server 10.32.7.193:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.186:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.194:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.187:9527 weight=1 max_fails=1 fail_timeout=30s;
  server 10.32.7.106:9527 weight=1 max_fails=1 fail_timeout=30s;
}
 
 
server {
  listen 80;
  listen [::]:80;
  server_name myapp;
  gzip  on;
  client_max_body_size 2048m;
  location / {
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;
        add_header X-Upstream $upstream_addr;
        proxy_pass http://loadblance;
        proxy_http_version 1.1;
  }
}
```

3\. 파일에 구문 오류가 있는지 테스트합니다.

**sudo nginx -t**

4\. Nginx를 다시 시작합니다.

**sudo systemctl restart nginx**

5\. Nginx를 다시 시작한 후 Nginx를 통해 애플리케이션에 액세스할 수 있습니다. http:// _Nginx 서버 IP_ / _애플리케이션 이름_

![](https://help.grapecity.com.cn/download/thumbnails/80954068/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1673923298000\&api=v2)  (선택사항) 애플리케이션을 HTTPS 웹사이트로 게시하려면 Nginx가 설치된 서버에서 다음 단계를 수행해야 합니다.

**1. SSL 인증서를 생성합니다.**

TLS/SSL은 공개 인증서와 개인 키의 조합을 사용하여 작동합니다. SSL 키는 서버에서 비공개로 유지됩니다. 클라이언트로 전송되는 콘텐츠를 암호화하는 데 사용됩니다. SSL 인증서는 콘텐츠를 요청하는 모든 사람과 공개적으로 공유됩니다. 연결된 SSL 키로 서명된 콘텐츠를 해독하는 데 사용할 수 있습니다.

클라우드 공급자로부터 Https 인증서를 구입하고 공익 기관에서 제공하는 Https 인증서를 사용할 수 있습니다. 테스트 환경에서는 자체 서명된 인증서를 사용합니다.

단일 명령으로 OpenSSL을 사용하여 자체 서명된 키와 인증서 쌍을 만들 수 있습니다.

**sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/nginx-selfsigned.key -out /etc/ssl/certs/nginx-selfsigned.crt**

* openssl : OpenSSL 인증서, 키 및 기타 파일을 만들고 관리하기 위한 기본 명령줄 도구입니다.
* req : 이 하위 명령은 X.509 인증서 서명 요청(CSR) 관리를 사용하도록 지정합니다. "X.509"는 SSL 및 TLS가 키 및 인증서 관리를 위해 따르는 공개 키 인프라 표준입니다. 새 X.509 인증서를 생성하려고 하므로 이 하위 명령을 사용하고 있습니다.
* -x509 : 평소처럼 인증서 서명 요청을 생성하는 대신 자체 서명된 인증서를 만들고 싶다고 유틸리티에 알려 이전 하위 명령을 추가로 수정합니다.
* -nodes : 인증서를 보호하기 위해 암호 사용을 건너뛸 수 있는 옵션입니다. 서버가 시작될 때 사용자 개입 없이 파일을 읽을 수 있으려면 Nginx가 필요합니다. 재부팅할 때마다 암호를 입력해야 하므로 암호를 사용하면 이런 일이 발생하지 않습니다.
* -days 365 : 인증서의 유효 기간입니다. 여기서 1년이 설정됩니다.
* -newkey rsa:2048 : 새 인증서와 새 키를 동시에 생성하도록 지정하여 2048비트 길이의 RSA 키를 생성합니다.`rsa:2048`
* -keyout : 생성된 개인 키 파일이 저장되는 경로입니다.
* -out : 생성된 인증서가 저장되는 경로.

이 명령을 실행하면 인증서와 키가 생성됩니다. 그 사이에 다음과 같이 프롬프트에 따라 필수 정보를 입력해야 합니다.



```
OutputCountry Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:New York
Locality Name (eg, city) []:New York City
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Bouncy Castles, Inc.
Organizational Unit Name (eg, section) []:Ministry of Water Slides
Common Name (e.g. server FQDN or YOUR name) []:server_IP_address
Email Address []:admin@your_domain.com
```

그 중 Common Name은 가장 중요한 정보로 서버와 연결된 도메인 이름을 입력하거나 서버의 공인 IP 주소를 입력해야 합니다.

명령 실행이 완료되면 생성된 두 파일이 해당 디렉토리 에 배치됩니다 .

OpenSSL을 사용할 때 클라이언트와 " [완벽한 순방향 보안"을](https://en.wikipedia.org/wiki/Forward_secrecy) 협상하는 데 사용되는 Diffie-Hellman 그룹도 만들어야 합니다 .

다음 명령을 실행합니다.

**sudo openssl dhparam -out /etc/nginx/dhparam.pem 4096**

명령이 실행된 후 구성에서 사용할 수 있는 " /etc/nginx/dhparam.pem " 에 DH 그룹이 생성됩니다 .

**2. SSL을 사용하도록 Nginx를 구성합니다.**

① 다음 명령을 실행하여 "self-signed.conf"라는 새 파일을 생성합니다.

**sudo vim /etc/nginx/snippets/self-signed.conf**

②"self-signed.conf" 파일에 다음 내용을 추가합니다.

ssl\_certificate /etc/ssl/certs/nginx-selfsigned.crt;ssl\_certificate\_key\
/etc/ssl/private/nginx-selfsigned.key;

③ 다음 명령을 실행하여 "ssl-params.conf"라는 새 파일을 생성합니다.

**sudo vi /etc/nginx/snippets/ssl-params.conf**

④ "ssl-params.conf" 파일에 다음 내용을 복사합니다.



```
ssl_protocols TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_dhparam /etc/nginx/dhparam.pem;
ssl_ciphers ECDHE-RSA-AES256-GCM-SHA512:DHE-RSA-AES256-GCM-SHA512:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-SHA384;
ssl_ecdh_curve secp384r1; # Requires nginx >= 1.1.0
ssl_session_timeout  10m;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off; # Requires nginx >= 1.5.9
ssl_stapling on; # Requires nginx >= 1.3.7
ssl_stapling_verify on; # Requires nginx => 1.3.7
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# Disable strict transport security for now. You can uncomment the following
# line if you understand the implications.
# add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;
add_header X-XSS-Protection "1; mode=block";
```

⑤ 다음 명령을 실행하여 파일을 백업합니다.

데비안 기반 :

&#x20;       **sudo  cp    /etc/nginx/sites-available/myapp.conf /etc/nginx/sites-available/myapp.conf.bak**

RPM 기반:<br>

**sudo cp /etc/nginx/conf.d/myapp.conf /etc/nginx/conf.d/myapp.conf.bak**

⑥ 다음 명령을 실행하여 구성 파일을 엽니다.

데비안 기반 :

**sudo vi /etc/nginx/sites-available/myapp.conf**

RPM 기반:\
**sudo vi /etc/nginx/conf.d/myapp.conf**

⑦ 구성 파일에서 포트 443과 ssl을 사용하도록 두 개의 listen 문을 업데이트하고 이전 단계에서 생성한 "self-signed.conf" 및 "ssl-params.conf" 두 파일을 포함합니다.



```
upstream loadblance {
    server 10.32.7.193:9527 weight=1 max_fails=0 fail_timeout=600;
    server 10.32.7.186:9527 weight=1 max_fails=0 fail_timeout=600;
    server 10.32.7.194:9527 weight=1 max_fails=0 fail_timeout=600;
    server 10.32.7.187:9527 weight=1 max_fails=0 fail_timeout=600;
    server 10.32.7.106:9527 weight=1 max_fails=0 fail_timeout=600;
}
 
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    include snippets/self-signed.conf;
    include snippets/ssl-params.conf;
    gzip  on;
     client_max_body_size 2048m;
    server_name forguncyapp.com www.forguncyapp.com;
    location / {
         proxy_set_header   Upgrade $http_upgrade;
         proxy_set_header   Connection keep-alive;
         proxy_set_header   Host $host;
         proxy_cache_bypass $http_upgrade;
         proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
         proxy_set_header   X-Forwarded-Proto $scheme;
         proxy_set_header Host $host;
          add_header X-Upstream $upstream_addr;
       proxy_pass http://loadblance;
       proxy_http_version 1.1;
   }
}
```

**3. 방화벽을 조정합니다.**

① 사용 가능한 구성 파일을 보려면 다음 명령을 실행합니다.

**sudo ufw 앱 목록**

출력은 다음과 같습니다.

출력사용 가능한 애플리케이션:\
Nginx Full\
Nginx HTTP\
Nginx HTTPS\
OpenSSH

② 다음 명령을 실행하여 현재 설정을 확인합니다.

**sudo ufw status**

다음 출력은 웹 서버를 통해 HTTP 트래픽만 허용됨을 보여줍니다.

OutputStatus: active\
\
To Action From\
\-- ------ ----\
OpenSSH ALLOW Anywhere\
Nginx HTTP ALLOW Anywhere\
OpenSSH(v6) ALLOW Anywhere(v6)\
Nginx HTTP(v6) ALLOW Anywhere(v6)

③ 다음 명령을 실행하여 HTTPS 트래픽이 웹 서버를 통과하도록 허용합니다.

**sudo ufw allow 'Nginx Full'**\
**sudo ufw delete allow 'Nginx HTTP'**

④ 현재 설정을 보려면 다음 명령을 실행하십시오 .

**sudo ufw status**

출력은 다음과 같습니다.

OutputStatus: active\
\
To Action From\
\-- ------ ----\
OpenSSH ALLOW Anywhere\
Nginx Full ALLOW Anywhere\
OpenSSH(v6) ALLOW Anywhere(v6)\
Nginx Full(v6) ALLOW Anywhere(v6)

**4. Nginx 업데이트를 활성화합니다.**

① 다음 명령을 실행하여 모든 파일에 구문 오류가 있는지 확인합니다.

**sudo nginx -t**

출력은 다음과 같이 정상이며 구문 오류가 없음을 나타냅니다.

Outputnginx: \[warn] "ssl\_stapling" ignored, issuer certificate not found nginx: the configuration file /etc/nginx/nginx.conf syntax is ok nginx: configuration file /etc/nginx/nginx.conf test is successful

② 다음 명령을 실행하여 Nginx를 다시 시작합니다.

**sudo systemctl restart nginx**

**5. 암호화를 테스트합니다.**

웹 브라우저를 열고 주소 표시줄에 서버의 도메인 이름 또는 IP를 입력합니다 .

https://서버\_도메인\_or\_IP

생성한 인증서는 브라우저의 신뢰할 수 있는 인증 기관 중 하나에서 서명하지 않았으므로 다음과 유사한 경고가 표시될 수 있습니다.
