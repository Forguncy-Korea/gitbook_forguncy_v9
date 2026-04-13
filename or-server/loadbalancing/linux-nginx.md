# Linux 서버에 Nginx 설치

이 섹션에서는 Linux 서버에 Nginx를 설치하는 단계를 설명합니다.



**Debian 기반(운영 체제는 Ubuntu)**

sudo 권한이 있는 루트가 아닌 일반 사용자를 준비하고 이 사용자를 사용하여 Linux 서버에 로그인한 후 다음 단계를 수행하십시오.

**단계**

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1673923299000\&api=v2)  Nginx를 설치합니다.

Nginx는 Ubuntu의 기본 리포지토리에서 사용할 수 있으므로 apt 패키징 시스템을 사용하여 이러한 리포지토리에서 Nginx를 설치할 수 있습니다.

최신 패키지 목록에 액세스할 수 있도록 다음 명령을 실행하여 로컬 패키지 인덱스를 업데이트합니다.

**sudo 적절한 업데이트**

다음 명령을 실행하여 nginx를 설치합니다.\
**sudo apt install nginx**

이 두 명령을 실행한 후 apt는 Nginx와 필요한 모든 종속성을 서버에 설치합니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923299000\&api=v2)  방화벽을 조정하십시오.

Nginx를 테스트하기 전에 서비스에 대한 액세스를 허용하도록 방화벽을 조정해야 합니다. Nginx는 설치 중에 자신을 ufw 서비스로 등록하여 Nginx에 직접 액세스할 수 있습니다.

다음 명령을 실행하여 ufw가 사용할 수 있는 애플리케이션 구성을 나열합니다.

**sudo ufw app list**

명령을 실행하면 다음 화면과 유사한 애플리케이션 구성 파일 목록이 표시됩니다.

출력사용 가능한 애플리케이션:\
Nginx Full\
Nginx HTTP\
Nginx HTTPS\
OpenSSH

보시다시피 Nginx에는 사용 가능한 세 가지 구성 파일이 있습니다.

* Nginx Full : 이 프로필은 포트 80(일반, 암호화되지 않은 웹 트래픽)과 포트 443(TLS/SSL 암호화된 트래픽)을 모두 엽니다.
* Nginx HTTP : 이 프로필은 포트 80(암호화되지 않은 일반 웹 트래픽)만 엽니다.
* Nginx HTTPS : 이 구성 파일은 포트 443(TLS/SSL 암호화 트래픽)만 엽니다.

구성한 트래픽을 계속 허용하는 가장 제한적인 프로필을 활성화하는 것이 좋습니다. 여기에서는 포트 80의 트래픽만 허용하면 됩니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1673923299000\&api=v2)  웹 서버를 확인하십시오.

Nginx 설치가 완료되면 Ubuntu가 Nginx를 시작하고 웹 서버가 실행됩니다.

다음 명령을 실행하여 systemd init 시스템에서 서비스가 실행 중인지 확인합니다.

**systemctl status nginx**

출력은 다음과 유사하며 서비스가 정상적으로 실행되고 있음을 나타냅니다.

```
Output● nginx.service - A high performance web server and a reverse proxy server
   Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2018-04-20 16:08:19 UTC; 3 days ago
     Docs: man:nginx(8)
 Main PID: 2369 (nginx)
    Tasks: 2 (limit: 1153)
   CGroup: /system.slice/nginx.service
           ├─2369 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
           └─2380 nginx: worker process
```

서비스가 정상적으로 실행되고 있는지 확인하는 또 다른 방법인 Nginx에서 페이지를 요청하는 방법을 사용할 수도 있습니다.

서버의 IP 주소로 이동하여 기본 Nginx 로그인 페이지를 방문하여 서비스가 제대로 작동하는지 확인할 수 있습니다.

서버의 IP 주소를 모르는 경우 다음 명령을 실행하십시오.

**ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\\/.\*$//'**

반환된 정보를 얻은 후 브라우저에서 하나씩 시도하여 액세스할 수 있는지 확인할 수 있습니다.

또는 다음 명령을 실행하여 퍼블릭 IP 주소를 가져옵니다.

**curl -4 icanhazip.com**

서버의 IP 주소를 얻은 후 브라우저의 주소에 http:// _server IP 를_ 입력하면 다음과 같은 화면이 나오며 서비스가 정상적으로 실행되고 있음을 확인할 수 있습니다.

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-6-17_13-57-7.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2" alt=""><figcaption></figcaption></figure>

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1673923299000\&api=v2)  Nginx 프로세스를 관리합니다. 다음 명령을 사용하여 Nginx 프로세스를 관리합니다.

웹 서비스 중지

**sudo systemctl stop nginx**

웹 서비스 중지 후 서비스 시작

**sudo systemctl start nginx**

웹 서비스를 다시 시작하십시오.

**sudo systemctl restart nginx**

구성 변경 후 Nginx 연결 해제 없이 다시 로드

**sudo systemctl reload nginx**

기본적으로 Nginx는 서버가 시작될 때 자동으로 시작되도록 구성되어 있습니다. 이 동작은 다음을 실행하여 비활성화할 수 있습니다.

**sudo systemctl disable nginx**

부팅 시 시작되도록 서비스를 다시 활성화합니다.

**sudo systemctl enable nginx**

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1673923299000\&api=v2)  서버 블록을 설정합니다(권장).

Nginx 웹 서버를 사용할 때 서버 블록을 사용하여 구성 세부 정보를 캡슐화하고 단일 서버에서 여러 도메인을 호스트할 수 있습니다. [예를 들어 example.com](http://example.com/) 이라는 도메인 이름을 만들려면 고유한 도메인 이름을 만들 수 있습니다.

Nginx에는 기본적으로 "/var/www/html" 디렉토리의 문서를 제공하도록 구성된 서버 블록이 활성화되어 있습니다.

① 다음 명령을 실행하여 example.com에 대한 디렉토리를 생성하고 -p 플래그를 사용하여 필요한 모든 상위 디렉토리를 생성합니다.

**sudo mkdir -p /var/www/example.com/html**

② $USER 환경 변수를 사용하여 다음 명령을 실행하여 디렉터리 소유권을 할당합니다 .

**sudo chown -R $USER:$USER /var/www/example.com/html**

③ umask 값을 수정하지 않았다면 네트워크 루트 디렉토리의 권한이 정확해야 합니다. 다음 명령을 실행하여 권한이 올바른지 확인할 수 있습니다.

**sudo chmod -R 755 /var/www/example.com**

④ 다음 명령을 실행하여 nano를 사용하여 샘플 "index.html" 페이지를 생성합니다.

**nano /var/www/example.com/html/index.html**

그리고 "index.html"에 다음 내용을 추가하고 저장하고 닫습니다.

```
<html>
    <head>
        <title>Welcome to Example.com!</title>
    </head>
    <body>
        <h1>Success!  The example.com server block is working!</h1>
    </body>
</html>
```

⑤ 다음 명령어를 실행하여 새로운 서버 블록을 생성합니다.

**sudo vi /etc/nginx/sites-available/example.com**

다음 내용을 "example.com"에 붙여넣고 루트의 디렉터리 및 도메인 이름을 업데이트합니다.

```
server {
        listen 80;
        listen [::]:80;
 
        root /var/www/example.com/html;
        index index.html index.htm index.nginx-debian.html;
 
        server_name example.com www.example.com;
 
        location / {
                try_files $uri $uri/ =404;
        }
}
```

⑥ 다음 명령을 실행하여 시작 시 Nginx에서 읽은 사이트 활성화 디렉토리에 대한 링크를 생성하여 파일을 활성화합니다.

**sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/**

두 개의 서버 블록이 이제 사용 가능하며 listen 및 server\_name 지시문을 기반으로 요청에 응답하도록 구성됩니다.

* example.com: example.com 및 www.example.com에 대한 요청에 응답합니다.
* 기본: 다른 두 모듈과 일치하지 않는 포트 80의 모든 요청에 ​​응답합니다.

⑦다른 서버 이름을 추가하여 발생할 수 있는 해시 버킷 메모리 문제를 방지하려면 "/etc/nginx/nginx.conf" 파일에서 값을 조정해야 합니다.

다음 명령을 실행하여 파일을 엽니다.

**sudo vi /etc/nginx/nginx.conf**

"server\_names\_hash\_bucket\_size" 지시문을 찾아 "#" 기호를 삭제하여 줄의 주석 처리를 제거하고 저장한 후 닫습니다.

```
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

⑧ 다음 명령을 실행하여 Nginx 파일에 구문 오류가 없는지 확인합니다.

**sudo nginx -t**

이제 Nginx가 도메인 이름을 제공합니다. http://example.com으로 이동하여 이를 테스트 할 수 있으며 다음과 같이 표시됩니다.

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-4-9_8-26-40.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A46.png?version=1\&modificationDate=1673923299000\&api=v2)  중요한 Nginx 파일 및 디렉토리에 익숙해지십시오.

**콘텐츠**

* /var/www/html: 실제 웹 콘텐츠(기본적으로 이전에 본 기본 Nginx 페이지로만 구성됨)는 /var/www/html 디렉토리에서 제공됩니다. Nginx 구성 파일을 변경하여 변경할 수 있습니다.

**서버 구성**

* /etc/nginxNginx 구성 디렉토리. 모든 Nginx 구성 파일은 여기에 있습니다.
* /etc/nginx/nginx.conf: 기본 Nginx 구성 파일입니다. Nginx 전역 구성을 변경하도록 수정할 수 있습니다.
* /etc/nginx/sites-available/: 각 사이트 서버 블록을 저장할 수 있는 디렉토리입니다. Nginx는 사이트 활성화 디렉토리에 연결되지 않은 경우 이 디렉토리에 있는 구성 파일을 사용하지 않습니다. 일반적으로 모든 서버 블록 구성은 이 디렉토리에서 수행된 다음 다른 디렉토리에 연결하여 활성화됩니다.
* /etc/nginx/sites-enabled/: 활성화된 각 사이트 서버 블록이 저장되는 디렉토리입니다. 일반적으로 이러한 파일은 사이트 사용 가능 디렉터리에서 찾을 수 있는 구성 파일에 연결하여 생성됩니다.
* /etc/nginx/snippets: 이 디렉토리에는 Nginx 구성의 다른 위치에 포함될 수 있는 구성 스니펫이 포함되어 있습니다.

**서버 로그**

* /var/log/nginx/access.log: Nginx가 다르게 구성되지 않는 한 웹 서버에 대한 모든 요청이 이 로그 파일에 기록됩니다.
* /var/log/nginx/error.log: 모든 Nginx 오류가 이 로그에 기록됩니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E7%BB%93%E6%9D%9F.png?version=1\&modificationDate=1673923299000\&api=v2)

**RPM 기준 (운영체제는 CentOS, RedHat 7.6, 낙찰 기린)**

**단계**

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1673923299000\&api=v2)  리포지토리 패키지 목록을 업데이트합니다. 다음 명령을 실행합니다.

**sudo yum -y update**

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923299000\&api=v2)  EPEL(Enterprise Linux용 추가 패키지)을 설치합니다.

Nginx는 CentOS 패키지와 함께 제공되는 표준 리포지토리에서 사용할 수 없으므로 서버에 EPEL 리포지토리를 설치해야 합니다. EPEL은 무료로 사용할 수 있으며 Yum과 함께 설치할 수 있는 많은 오픈 소스 패키지를 제공합니다.

EPEL을 설치하려면 Yum 패키지 관리자를 사용하여 다음 명령을 실행합니다.

**sudo yum install -y epel-release**

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-4-9_8-26-59.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1673923299000\&api=v2)  Nginx를 설치합니다.

단계![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923299000\&api=v2)NET에서 Nginx 저장소가 서버에 추가되었습니다. 이제 다음 yum 명령을 실행하여 Nginx를 설치할 수 있습니다.

**sudo yum –y install nginx**

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-4-9_8-27-13.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1673923299000\&api=v2)  Nginx 서비스를 시작합니다. Nginx를 설치한 후 자동으로 시작되지 않으므로 Nginx 서비스를 시작하려면 다음 명령을 실행해야 합니다.

**sudo systemctl start nginx**

다음 명령을 실행하여 상태를 확인하십시오.

**sudo systemctl status nginx**

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-4-9_8-27-25.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

녹색 텍스트는 " active(running) "이어야 합니다. 표시되지 않으면 Nginx 인스턴스가 성공적으로 시작되지 않았을 수 있습니다.

참고: 이미 Apache 서버를 실행 중인 경우 Nginx를 시작하기 전에 비활성화해야 합니다. **sudo service httpd stop** 명령을 사용하십시오 . Apache를 비활성화하면 현재 호스팅되는 모든 웹사이트가 종료됩니다.

Apache가 비활성화된 경우에도 서버 재부팅 중에 자동으로 시작될 수 있습니다. 다음 명령을 실행하여 자동 시작을 비활성화합니다.

**sudo systemctl disable httpd**

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1673923299000\&api=v2)  서버가 시작될 때 Nginx가 자동으로 시작되도록 구성합니다. 다음 명령을 실행합니다.

**sudo systemctl enable nginx**

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A46.png?version=1\&modificationDate=1673923299000\&api=v2)  트래픽을 허용하도록 방화벽을 구성합니다.

CentOS 7에는 기본적으로 방화벽이 활성화되어 있으며 포트 80 및 443에 대한 액세스를 차단합니다. Nginx의 모든 인바운드 HTTPS 및 HTTP 패킷을 차단합니다.

HTTP 및 HTTPS 통신을 허용하려면 다음 명령을 실행하십시오.

**firewall-cmd --zone=public --permanent --add-service=http**\
**firewall-cmd --zone=public --permanent --add-service=https**\
**firewall-cmd --reload**

각 명령 다음에 "success"가 표시되어 명령이 올바르게 실행되었음을 나타냅니다.

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-4-9_8-28-11.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A47.png?version=1\&modificationDate=1673923299000\&api=v2)  Nginx 설치를 확인합니다.

Nginx가 제대로 실행되고 있는지 확인하는 가장 쉬운 방법은 서버의 공용 IP 주소를 방문하는 것입니다. 웹 브라우저를 열고 " http:// _서버 IP 또는 도메인 이름_ " 을 방문하십시오.

다음 명령을 실행하여 서버의 공용 IP 주소를 찾으십시오.

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-4-9_8-28-25.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

브라우저에서 액세스하면 아래와 같이 Nginx 시작 인터페이스가 표시됩니다.

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954096/image2021-4-9_8-28-38.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

![](https://help.grapecity.com.cn/download/thumbnails/80954096/%E6%AD%A5%E9%AA%A48.png?version=1\&modificationDate=1673923299000\&api=v2)  SELinux를 구성합니다.

다음 명령을 실행하여 SElinux가 활성화되었는지 확인하십시오.

**getenforce**

Enforcing이 출력되면 SELinux가 활성화되어 있고 Nginx가 http 요청을 보낼 수 있는 권한을 활성화해야 함을 의미합니다. 다음 명령을 실행합니다.

**sudo setsebool -P httpd\_can\_network\_connect 1**

<br>

**" 테스트 실패" 오류를 수정하는 방법**

nginx.conf 파일 에 대해 " 테스트 실패 " 오류 메시지가 표시되면 IP 주소 문제일 수 있습니다.

기본적으로 Nginx 서비스는 IPv4와 IPv6 모두에서 수신 대기합니다. 서버가 IPv6를 지원하지 않으면 테스트에 실패합니다. 이는 기본 구성 파일을 수정하여 수정할 수 있습니다.

기본 구성 파일 "/etc/nginx/nginx.conf"를 열고 **다음** 줄을 찾아 주석 처리한 다음 앞에 "#"을 추가합니다.

```
# listen [::]:80 default_server;
```

파일을 저장하고 닫은 후 다음 명령을 실행하여 Nginx 서비스를 다시 시작합니다.

**sudo systemctl reload nginx**

서비스를 재시작한 후 브라우저에서 서버 IP 주소에 접속하면 Nginx 페이지가 표시되어야 합니다.

<br>

**Nginx 구성 파일 및 루트 디렉토리**

Nginx 구성 파일의 위치와 기본 Nginx 서버 루트 디렉터리를 알아야 합니다.

추가 서버 블록

Apache에서 관리자는 가상 호스트를 사용하여 여러 웹 사이트를 실행합니다. Nginx를 사용하면 서버 블록을 사용하여 단일 서버에서 여러 웹 사이트를 실행할 수 있습니다.

확장자가 .conf인 새 구성 파일을 만들어 추가 서버 블록을 추가합니다. 이 파일을 "/etc/nginx/conf.d"에 넣으면 Nginx가 시작될 때마다 로드됩니다.

기본 Nginx 서버 루트 디렉터리

기본 Nginx 서버 루트 디렉토리는 "/usr/share/nginx"입니다. 이는 "/etc/nginx/conf.d/default.conf"에 있는 기본 서버 블록 구성 파일에 지정됩니다.

웹 파일을 포함하는 기본 서버 문서 루트 디렉토리는 "usr/share/nginx/html"입니다.

글로벌 구성

전역 구성은 "/etc/nginx/nginx.conf"에서 기본 Nginx 구성 파일을 수정하여 조정할 수 있습니다. 기본적으로 다음 세 가지를 인식할 수 있습니다.

*
  * 이벤트는 Nginx가 일반적으로 연결을 처리하는 방법을 정의하는 전역 설정입니다.
  * HTTP는 서버가 HTTP 및 HTTPS 연결을 처리하는 방법을 정의합니다.
  * 서버는 HTTP 컨텍스트에서 정의됩니다. 서버 포트, 문서 루트 등을 지정합니다.

Nginx 관리

nginx 중지

**sudo systemctl stop nginx**

Nginx 서비스를 다시 시작하십시오.

**sudo systemctl restart nginx**

nginx를 새로 고침

**sudo systemctl reload nginx**

서버 시작 시 Nginx 자동 시작 비활성화

**sudo systemctl disable nginx**

<br>

<br>

새 디렉토리 구성

여러 웹사이트를 호스팅하는 경우 표준 명명 규칙을 따르는 것이 가장 좋습니다. cPanel의 표준 이름 지정 및 디렉터리 생성을 사용합니다.

**sudo mkdir -p /var/www/yourdomain.com/public\_html**

그런 다음 구성을 테스트하는 데 도움이 되는 인덱스 페이지를 만듭니다.

**sudo nano /var/www/yourdomain.com/public\_html/index.html**

테스트를 위해 index.html에 텍스트 줄을 입력하고 파일을 저장한 후 닫습니다.

데이터에 온라인으로 액세스할 수 있도록 다음 명령을 실행하여 Linux 파일 권한을 변경하십시오.

**sudo chmod 755 /var/www/yourdomain.com/public\_html**

그런 다음 index.html 페이지를 온라인으로 열 수 있습니다.
