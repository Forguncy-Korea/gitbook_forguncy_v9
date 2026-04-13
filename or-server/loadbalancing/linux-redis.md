# Linux 서버에 Redis 설치 및 구성

Linux 서버에 Redis를 설치하는 방법을 소개합니다.



**Debian 기반(운영 체제는 Ubuntu)**

**단계**

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1673923299000\&api=v2)  레디스를 설치합니다.

① SSH 터미널을 통해 다음 명령을 실행하여 apt 패키지 목록을 업데이트합니다.

**sudo apt update**<br>

② 다음 명령어를 실행하여 Redis를 설치합니다.

**sudo apt install redis-server**

③설치가 완료되면 Redis 서비스가 자동으로 시작됩니다. 다음 명령을 실행하여 서비스 상태를 확인합니다.

**sudo systemctl status redis-server**

다음과 유사한 출력은 Redis가 서버에 설치되어 실행되고 있음을 나타냅니다.

```
● edis-server.service - Advanced key-value store
   Loaded: loaded (/lib/systemd/system/redis-server.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2018-10-28 05:10:45 PDT; 2h ago
     Docs: http://redis.io/documentation,
           man:redis-server(1)
  Process: 2197 ExecStop=/bin/kill -s TERM $MAINPID (code=exited, status=0/SUCCESS)
  Process: 2201 ExecStart=/usr/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 2226 (redis-server)
    Tasks: 4 (limit: 2319)
   CGroup: /system.slice/redis-server.service
           `-2226 /usr/bin/redis-server 0.0.0.0:6379
```

서버에서 IPv6이 비활성화되어 있으면 Redis 서비스가 시작되지 않습니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923299000\&api=v2)  Redis 원격 액세스를 구성합니다.

기본적으로 Redis는 원격 연결을 허용하지 않습니다. 127.0.0.1(localhost)에서 Redis를 실행하는 컴퓨터에서만 Redis 서버에 연결할 수 있습니다.

원격 호스트에서 Redis 서버에 연결하려는 경우에만 아래 단계를 따르십시오. 단일 서버 설정을 사용하고 애플리케이션과 Redis가 동일한 시스템에서 실행 중인 경우 원격 액세스를 활성화할 필요가 없습니다.

① 원격 연결을 허용하도록 Redis를 구성하려면 텍스트 편집기로 Redis 구성 파일을 엽니다. 다음 명령을 실행합니다.

**sudo vi /etc/redis/redis.conf**<br>

이 파일에서 "bind 127.0.0.1 ::1 "로 시작하는 줄을 찾아 " 127.0.0.1 "을 " "로 바꿉니다 .`0.0.0.0`

| `# IF YOU ARE SURE YOU WANT YOUR INSTANCE TO LISTEN TO ALL THE INTERFACES# JUST COMMENT THE FOLLOWING LINE.# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~bind 0.0.0.0` `::1` |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

편집이 끝나면 저장하고 닫습니다.

② 다음 명령어를 실행하여 Redis 서비스를 재시작하여 적용합니다.

**sudo systemctl restart redis-server**

③ 다음 명령을 실행하여 redis가 포트 6379의 모든 인터페이스에서 수신 대기하는지 확인합니다.

**ss -an | grep 6379**

출력은 다음과 유사합니다. "0.0.0.0"은 이 시스템의 모든 IPv4 주소를 의미합니다.

| <p><code>tcp  LISTEN 0</code>   <code>128</code>   <code>0.0.0.0:6379</code>   <code>0.0.0.0:*</code></p><p><code>tcp  LISTEN 0</code>   <code>128</code>      <code>[::]:6379</code>      <code>[::]:*</code></p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

④원격 컴퓨터에서 TCP 포트 6379를 활성화하도록 방화벽 규칙을 추가합니다.

UFW를 사용하여 방화벽을 관리하고 있고 192.168.121.0/24 서브넷에서 액세스를 허용하려는 경우 다음 명령을 실행할 수 있습니다.

**sudo ufw allow proto tcp from 192.168.121.0/24 to any port 6379**

이 시점에서 Redis 서버는 TCP 포트 6379에서 원격 연결을 수락합니다. 방화벽이 신뢰할 수 있는 IP 범위의 연결만 허용하도록 구성되어 있는지 확인해야 합니다.

모든 것이 올바르게 설정되었는지 확인하려면 redis-cli 유틸리티를 사용하여 원격 시스템에서 Redis 서버에 ping을 시도할 수 있습니다. 다음 명령을 실행합니다.

**redis-cli -h \<REDIS\_IP\_ADDRESS> ping**

이 명령은 PONG 응답을 반환해야 합니다.

PONG

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E7%BB%93%E6%9D%9F.png?version=1\&modificationDate=1673923299000\&api=v2)

**RPM 기준 (운영체제는 CentOS, RedHat 7.6)**

**단계**

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1673923299000\&api=v2)  레디스를 설치합니다.

Redis를 설치하기 전에 먼저 EPEL(Extra Packages for Enterprise Linux) 리포지토리를 서버의 패키지 목록에 추가해야 합니다. EPEL은 많은 오픈 소스 애드온 패키지를 포함하는 패키지 저장소이며 대부분 Fedora 프로젝트에서 유지 관리합니다.<br>

① 다음 명령을 실행하여 yum을 사용하여 EPEL을 설치합니다.

**sudo yum install epel-release**

②EPEL 설치가 완료되면 다음 명령을 실행하여 yum을 사용하여 Redis를 설치합니다.

**sudo yum install redis -y**

③ 설치가 완료되면 다음 명령어를 실행하여 Redis 서비스를 시작합니다.

**sudo systemctl start redis.service**

④서버가 시작될 때 Redis가 자동으로 시작되도록 하려면 다음 명령을 실행할 수 있습니다.

**sudo systemctl enable redis**

⑤ 다음 명령을 실행하여 Redis의 상태를 확인합니다.

**sudo systemctl status redis.service**

출력은 다음과 유사하며 Redis가 정상적으로 실행되고 있음을 나타냅니다.

```
Output● redis.service - Redis persistent key-value database
   Loaded: loaded (/usr/lib/systemd/system/redis.service; disabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/redis.service.d
           └─limit.conf
   Active: active (running) since Thu 2018-03-01 15:50:38 UTC; 7s ago
 Main PID: 3962 (redis-server)
   CGroup: /system.slice/redis.service
           └─3962 /usr/bin/redis-server 127.0.0.1:6379
```

⑥ Redis가 실행되는지 확인한 후 다음 명령어를 실행하여 설정을 테스트합니다.

**redis-cli ping**

명령을 실행한 후 응답으로 PONG이 출력되어야 합니다. 이 경우 이제 서버에서 Redis를 실행하고 보안을 강화하도록 구성을 시작할 수 있습니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923299000\&api=v2)  Redis를 바인딩하고 방화벽 보호를 사용합니다.

Redis를 보호하는 효과적인 방법은 실행 중인 서버를 보호하는 것입니다. 즉, Redis가 로컬 호스트 또는 사설 IP 주소에만 바인딩되고 서버에 실행 중인 방화벽이 있는지 확인하는 것입니다.

이 튜토리얼을 사용하여 Redis 클러스터를 설정하기로 선택한 경우 어디에서나 연결을 허용하도록 구성 파일을 업데이트해야 합니다. 이는 localhost 또는 개인 IP에 바인딩하는 것보다 덜 안전합니다.

①이 문제를 해결하기 위해 먼저 다음 명령을 실행하여 Redis 구성 파일을 편집용으로 열어야 합니다.

**sudo vi /etc/redis.conf**

②이 파일에서 bind로 시작하는 줄을 찾고 주석 처리되지 않았는지 확인하십시오.

bind 127.0.0.1

Redis를 다른 IP 주소에 바인딩해야 하는 경우(예: 별도의 호스트에서 Redis에 액세스하기 위해) 프라이빗 IP 주소에 바인딩하는 것이 좋습니다. 퍼블릭 IP 주소에 바인딩하면 외부 세계에 대한 Redis 인터페이스의 노출이 증가하기 때문입니다.

bind 0.0.0.0

전제 조건을 충족하고 서버에 방화벽을 설치했으며 다른 호스트에서 Redis에 연결하지 않으려는 경우 Redis에 대한 추가 방화벽 규칙을 추가할 필요가 없습니다.

Redis 서버의 기본 독립 실행형 설치는 루프백 인터페이스(127.0.0.1 또는 localhost)에서만 수신하므로 기본 포트에서 들어오는 트래픽에 대해 걱정할 필요가 없습니다.

③ 다른 호스트에서 Redis에 액세스하려면 firewall-cmd 명령을 사용하여 firewalld 구성을 일부 변경해야 합니다. 마찬가지로 호스트가 프라이빗 IP 주소를 사용하여 호스트에서 Redis 서버에 액세스하도록 허용하여 서비스에서 사용하는 호스트 수를 제한해야 합니다.

1\) 다음 명령을 실행하여 방화벽 정책에 전용 Redis 영역을 추가합니다.

**sudo firewall-cmd --permanent --new-zone=redis**

2\) 그런 다음 열려는 포트를 지정하여 다음 명령을 실행하십시오. Redis는 기본적으로 포트 6379를 사용합니다.

**sudo firewall-cmd --permanent --zone=redis --add-port=6379/tcp**

3\) 그런 다음 다음 명령을 실행하여 방화벽을 통과하고 Redis에 액세스할 수 있는 사설 IP 주소를 지정합니다.

**sudo firewall-cmd --permanent --zone=redis --add-source=client\_server\_private\_IP**

4\) 다음 명령을 실행한 후 방화벽을 다시 로드하여 새 규칙을 적용합니다.

**sudo firewall-cmd --reload**

이 구성에서 방화벽은 클라이언트의 IP 주소에서 패킷을 볼 때 사설 Redis 영역의 규칙을 해당 연결에 적용합니다. 다른 모든 연결은 기본 공개 영역에서 처리됩니다. 기본 영역의 서비스는 모든 연결에 적용되며 명시적으로 일치하지 않는 서비스로 제한되지 않으므로 규칙이 해당 영역에 자동으로 적용되므로 Redis 영역에 다른 서비스(예: SSH)를 추가할 필요가 없습니다. 연결.

5\) Iptables를 사용하여 방화벽을 설정하도록 선택한 경우 다음 명령을 실행하여 Redis가 사용 중인 포트에 대한 보조 호스트 액세스 권한을 부여해야 합니다.

**sudo iptables -A INPUT -i lo -j ACCEPT**\
**sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT**\
**sudo iptables -A INPUT -p tcp -s client\_servers\_private\_IP/32 --dport 6379 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT**\
**sudo iptables -P INPUT DROP**

배포판에서 제공하는 메커니즘을 사용하여 Iptables 방화벽 규칙을 저장 해야 합니다. Iptables에 대한 자세한 내용은 Iptables [essentials 가이드를](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands) 참조하십시오 .

모든 방화벽 도구가 작동합니다. 알 수 없는 사람이 서버에 액세스할 수 없도록 방화벽을 가동하고 실행하는 것이 중요합니다. 다음으로 강력한 암호로만 액세스할 수 있도록 Redis를 구성합니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1673923299000\&api=v2)  Redis 암호를 구성합니다.

Redis에 대한 암호를 이미 구성한 경우. 이 섹션의 지침에 따라 더 안전한 암호를 설정할 수 있습니다. 이 섹션에서는 아직 설정하지 않은 경우 데이터베이스 서버 비밀번호를 설정하는 방법에 대해 설명합니다.

Redis 암호를 구성하면 내장 보안 기능 중 하나인 auth 명령이 활성화됩니다. 이 기능을 사용하려면 클라이언트가 데이터베이스에 액세스하기 전에 인증해야 합니다. 바인드 설정과 마찬가지로 비밀번호는 Redis의 구성 파일 " /etc/redis.conf " 에서 직접 구성됩니다 .

① 다음 명령을 실행하여 파일을 다시 엽니다.

**sudo vi /etc/redis.conf**

②"보안" 섹션을 찾은 다음 다음과 같이 표시되는 주석이 있는 지침을 찾습니다.

\# requirepass fobared

"#"을 제거하여 주석을 제거한 다음 "fobared"를 강력한 암호를 설정한 것으로 변경하십시오. 직접 작성하는 대신 apg 또는 pwgen과 같은 도구를 사용하여 생성할 수 있습니다.

암호를 생성하는 애플리케이션을 설치하지 않으려면 다음 명령을 사용할 수 있습니다.

이 명령을 입력할 때마다 동일한 비밀번호가 생성됩니다. 다른 암호를 생성하려면 따옴표로 묶인 단어를 다른 단어나 구로 변경하십시오.

**echo "digital-ocean" | sha256sum**

이 명령으로 생성된 암호는 매우 강력하고 매우 긴 암호이며 Redis가 요구하는 암호 유형과 정확히 일치합니다. 다음과 같이 이 명령으로 생성된 암호를 "requirepass"의 새 값으로 복사하여 붙여넣습니다.

requirepass _password\_copied\_from\_output_

더 짧은 암호를 생성하려면 대신 아래 명령을 실행하십시오. 마찬가지로 다음과 같은 암호를 생성하지 않도록 따옴표로 묶인 단어를 변경합니다.

**echo "digital-ocean" | sha1sum**

③ 비밀번호를 설정한 후 파일을 저장하고 닫은 후 다음 명령을 실행하여 Redis를 다시 시작합니다.

**sudo systemctl restart redis.service**

④ 다음 명령을 실행하여 암호가 유효한지 테스트합니다.

**redis-cli**

⑤ 다음은 Redis 비밀번호가 유효한지 테스트하는 일련의 명령어입니다. 첫 번째 명령은 인증 전에 키를 일부 값으로 설정하려고 시도합니다.

**set key1 10**

이 명령은 아직 인증되지 않았기 때문에 작동하지 않으므로 Redis는 아래와 같이 오류를 반환합니다.

출력(오류) NOAUTH 인증이 필요합니다.

다음 명령을 실행하여 Redis 구성 파일에 지정된 암호를 사용하여 인증합니다.

**auth&#x20;**_**your\_redis\_password**_

Redis는 다음과 같이 인증되었음을 확인합니다.

출력OK

그런 다음 이전 명령을 다시 실행하면 성공해야 합니다.

**set key1 10**\
OutputOK

다음 명령을 실행하여 Redis에 새 키 값을 쿼리합니다.

**get key1**\
OutputOK

다음 명령을 실행하여 redis-cli를 종료합니다. "exit"를 사용할 수도 있습니다.

**quit**

위의 명령을 실행한 후에는 권한이 없는 사용자가 Redis에 액세스하기 어려워야 합니다. SSL 또는 VPN 없이 원격으로 Redis에 연결하는 경우 외부 사용자는 여전히 암호화되지 않은 암호를 볼 수 있습니다.

다음으로 악의적인 공격자로부터 Redis를 보호하기 위해 Redis 명령의 이름을 바꾸는 방법을 살펴보겠습니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1673923299000\&api=v2)  위험한 명령의 이름을 바꿉니다.

Redis에 내장된 또 다른 보안 기능을 사용하면 위험한 것으로 간주되는 특정 명령의 이름을 바꾸거나 완전히 비활성화할 수 있습니다. 권한이 없는 사용자가 실행할 때 이러한 명령을 사용하여 데이터를 재구성, 파기 또는 지우는 데 사용할 수 있습니다.

일부 알려진 위험한 명령은 다음과 같습니다.

* `FLUSHDB`
* `FLUSHALL`
* `KEYS`
* `PEXPIRE`
* `DEL`
* `CONFIG`
* `SHUTDOWN`
* `BGREWRITEAOF`
* `BGSAVE`
* `SAVE`
* `SPOP`
* `SREM` `RENAME` `DEBUG`

명령을 비활성화하거나 이름을 바꿀지 여부는 사이트 조건에 따라 다릅니다. 잠재적으로 남용될 수 있는 명령을 절대 사용하지 않을 것임을 알고 있으면 비활성화할 수 있습니다. 그렇지 않으면 이름을 바꿔야 합니다.

인증 암호와 마찬가지로 이름 바꾸기 또는 비활성화 명령도 " /etc/redis.conf " 파일 의 섹션에서 구성 됩니다.

① Redis 명령을 활성화 또는 비활성화하려면 다음 명령을 실행하고 편집을 위해 구성 파일을 다시 여십시오.

**sudo vi /etc/redis.conf**

지침: 다음은 예입니다. 이해하기 쉬운 명령을 비활성화하거나 이름을 바꾸도록 선택해야 합니다. 이러한 명령을 직접 검토하고 " redis.io/Commands " 에서 오용될 수 있는 상황을 식별할 수 있습니다 .

명령을 비활성화하거나 종료하려면 다음과 같이 빈 문자열로 이름을 바꾸십시오.

\
\#It is also possible to completely kill a command by renaming it into

\#an empty string:

\
rename-command FLUSHDB ""\
rename-command FLUSHALL ""\
rename-command DEBUG ""

명령의 이름을 바꾸려면 아래와 같이 다른 이름을 지정하십시오.

rename-command CONFIG ""&#x20;

rename-command SHUTDOWN SHUTDOWN\_MENOT&#x20;

rename-command CONFIG ASC12\_CONFIG

②변경 사항을 저장하고 파일을 닫은 후 다음 명령을 실행하여 Redis를 다시 시작하여 변경 사항을 적용합니다.

**sudo systemctl restart redis.service**

③ 다음 명령을 실행하여 새 명령을 테스트합니다.

**redis-cli**

④ 앞에서 정의한 비밀번호를 사용하여 인증하려면 다음 명령을 실행하십시오 .

**auth&#x20;**_**your\_redis\_password**_

OutputOK

⑤ CONFIG 명령의 이름을 ASC12\_CONFIG로 변경했다고 가정하면 CONFIG 명령을 사용하려는 시도는 실패해야 합니다.

**config get requirepass**

출력(오류) ERR unknown 명령 'config'

이름이 바뀐 명령을 호출하면 성공해야 합니다(대소문자 구분 안 함).

**asc12\_config get requirepass**

Output1) "requirepass"&#x20;

2\) "your\_redis\_password"

⑥ 다음 명령을 실행하여 redis-cli를 종료합니다.

**exit**

이미 Redis 명령줄을 사용하고 있는 경우 Redis를 다시 시작하면 다시 인증해야 합니다. 그렇지 않고 명령을 입력하면 다음 오류가 발생합니다.

출력NOAUTH 인증이 필요합니다.

![](https://help.grapecity.com.cn/download/thumbnails/80954117/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1673923299000\&api=v2)  데이터 디렉토리 소유권 및 파일 권한을 설정합니다.

소유권과 권한을 변경하여 Redis 설치의 보안 프로필을 높입니다 . 여기에는 Redis에 액세스해야 하는 사용자만 해당 데이터를 읽을 수 있는 권한을 갖도록 하는 것이 포함됩니다. 기본적으로 이 사용자는 redis 사용자입니다.

① 상위 디렉토리의 긴 목록에서 Redis 데이터 디렉토리를 grep-ing하여 이를 확인할 수 있습니다 . 명령과 해당 출력은 다음과 같습니다.<br>

**ls -l /var/lib | grep redis**

Outputdrwxr-xr-x 2 redis redis 4096 8월 6일 09:32 redis

② 보시다시피 redis 데이터 디렉토리는 redis 사용자가 소유하고 있으며, redis 그룹에는 2차 접근 권한이 부여되어 있습니다. 이 소유권 설정은 안전하지만 폴더의 권한(755로 설정)은 안전하지 않습니다. redis 사용자만 폴더와 해당 콘텐츠에 액세스할 수 있도록 하려면 권한 설정을 770으로 변경합니다.<br>

**sudo chmod 770 /var/lib/redis**<br>

③ 변경해야 할 또 다른 권한은 Redis 구성 파일의 권한입니다. 기본적으로 파일 권한은 644이고 루트가 소유하며 루트 그룹은 2차 소유권을 가집니다.<br>

**ls -l /etc/redis.conf**<br>

출력-rw-r--r-- 1 루트 루트 30176 2014년 1월 14일 /etc/redis.conf<br>

④ 644 권한이 공개됩니다. 구성 파일에는 4단계에서 구성한 암호화되지 않은 암호가 포함되어 있으므로 보안 문제가 발생합니다. 즉, 구성 파일의 소유권과 권한을 변경해야 합니다. 이상적으로는 redis 사용자가 소유하고 redis 그룹이 2차 소유권을 가져야 합니다. 따라서 다음 명령을 실행합니다.<br>

**sudo chown redis:redis /etc/redis.conf**<br>

그런 다음 파일 소유자만 파일을 읽거나 쓸 수 있도록 권한을 변경합니다.

**sudo chmod 600 /etc/redis.conf**

다음 명령을 실행하여 새 소유권 및 권한을 확인하십시오.

**ls -l /etc/redis.conf**

Outputtotal 40\
-rw------- 1 redis redis 29716 Sep 22 18:32 /etc/redis.conf

⑤마지막으로 다음 명령을 실행하여 Redis를 다시 시작합니다.

**sudo systemctl restart redis.service**
