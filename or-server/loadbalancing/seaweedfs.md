# SeaweedFS 설치 및 배포

각 애플리케이션 서버와 공유 스토리지 서버에 SeaweedFS를 설치해야 합니다.

이 섹션에서는 SeaweedFS를 설치하고 배포하는 방법을 설명합니다.



## **SeaweedFS 다운로드** <a href="#id-an-zhuang-bu-shu-seaweedfs2.-xia-zai-seaweedfs" id="id-an-zhuang-bu-shu-seaweedfs2.-xia-zai-seaweedfs"></a>

공유 스토리지 서버와 애플리케이션 서버에서 각각 다음 명령을 실행해야 합니다.

bash 명령을 실행합니다.

```
pushd /tmp/

curl -s https://api.github.com/repos/chrislusf/seaweedfs/releases/latest \
| grep "browser_download_url.*linux_amd64\.tar\.gz" \
| cut -d : -f 2,3 \
| tr -d \" \
| wget -qi -

tarball="$(find . -name "*linux_amd64.tar.gz")"

tar -xzf $tarball

chmod +x weed

sudo mv weed /usr/local/bin/

popd

location="$(which weed)"
echo "weed binary location: $location"

version="$(weed version)"
echo "weed binary version: $version"
```

명령을 실행하면 "/usr/local/bin/"에 실행 파일 "weed"가 있습니다.



### 공유 스토리지 서버에 SeaWeedFS 설치&#x20;

" /etc/systemd/system/ " 경로 에서 seaweed.service 파일을 생성하고 파일에 다음 콘텐츠를 추가합니다.

-dir 매개변수에 유의하십시오. 이 경로를 가진 폴더가 존재하는지 확인해야 합니다.<br>

| <p><code>[Unit]Description=Seaweed Server</code></p><p><code>After=network.target</code> </p><p></p><p><code>[Service]</code></p><p><code>Type=simple</code></p><p><code>Restart=on-failure</code></p><p><code>User=root</code></p><p><code>ExecStart=/usr/local/bin/weed server -dir=storage -filer=true</code></p><p><code>KillSignal=SIGTERM</code></p><p><code>[Install]</code></p><p><code>WantedBy=multi-user.target</code></p> |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\
다음 명령을 실행하여 systemd 데몬을 다시 로드하고 부팅 시 서비스를 활성화하고 SeaweedFS 서비스를 시작합니다.<br>

**sudo systemctl daemon-reload**\
**sudo systemctl enable seaweed.service**\
**sudo service seaweed start**

위의 명령을 실행하면 SeaweedFS 서비스가 시작됩니다.



### 애플리케이션 서버에 SeaweedFS설치&#x20;

![](https://help.grapecity.com.cn/download/thumbnails/80954154/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1673923299000\&api=v2)  sshfs가 설치되어 있는지 확인하십시오. 설치되지 않은 경우 아래 단계를 따르십시오.

* RPM 기반(운영체제는 CentOS, 낙찰 Kylin) Linux를 사용하는 경우 다음 명령을 실행하십시오.

**`sudo yum install epel-release -y`**<br>

**`sudo yum install sshfs -y`**

* RedHat 7.6 Linux를 사용하는 경우 다음 명령을 실행합니다.

**sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y**

**sudo yum install sshfs -y**

* Debian 기반(운영 체제는 Ubuntu) Linux를 사용하는 경우 다음 명령을 실행합니다.

**`sudo apt-get install sshfs -y`**

![](https://help.grapecity.com.cn/download/thumbnails/80954154/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923299000\&api=v2)  다음 명령을 실행하여 Mosaic 애플리케이션이 공유 스토리지에 액세스할 수 있도록 마운트를 생성합니다.

**sudo mkdir /mnt/weed**

![](https://help.grapecity.com.cn/download/thumbnails/80954154/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1673923299000\&api=v2)  "/etc/fuse.conf" 파일을 편집하고 주석을 제거하거나 다음을 추가합니다.

user\_allow\_other

![](https://help.grapecity.com.cn/download/thumbnails/80954154/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1673923299000\&api=v2)  "/etc/systemd/system/"에 "seaweedfuse.service" 파일을 만들고 다음 콘텐츠를 추가합니다.

-dir 매개변수에 유의하십시오. 이 경로를 가진 폴더가 존재하는지 확인해야 합니다. 이 경로는 공유 스토리지 경로를 구성할 때 사용해야 합니다.

SHARED\_STORAGE\_SERVER\_IP를 실제 스토리지 서버 IP 주소로 바꿔야 합니다.

| <p><code>[Unit]</code></p><p><code>Description=Seaweed FUSE</code></p><p><code>Before=ForguncyServerService.service</code></p><p><code>After=network.target</code> </p><p></p><p><code>[Service]</code></p><p><code>Type=simple</code></p><p><code>Restart=on-failure</code></p><p><code>User=root</code></p><p><code>ExecStart=/usr/local/bin/weed mount -filer=SHARED_STORAGE_SERVER_IP:8888</code> <code>-dir=/mnt/weed -filer.path=/</code></p><p><code>KillSignal=SIGTERM</code> </p><p></p><p><code>[Install]</code></p><p><code>WantedBy=multi-user.target</code></p> |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\
![](https://help.grapecity.com.cn/download/thumbnails/80954154/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1673923299000\&api=v2)  다음 명령을 실행하여 systemd 데몬을 다시 로드하고 부팅 시 서비스를 활성화하고 SeaweedFS 서비스를 시작합니다.

**sudo systemctl daemon-reload**\
**sudo systemctl enable seaweedfuse.service**\
**sudo service seaweedfuse start**

위의 명령을 실행하면 SeaweedFS 서비스가 시작됩니다.
