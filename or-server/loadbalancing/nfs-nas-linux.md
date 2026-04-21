# NFS를 통해 NAS 폴더를 Linux에 마운트

부팅 시 NFS 파일 시스템의 자동 마운트를 허용하도록 Linux 구성 파일을 수정하는 방법을 설명합니다 .



### 전제조건&#x20;

* 파일 시스템이 생성되었습니다.
* 마운트 지점이 생성되었습니다.
* NFS 클라이언트가 설치되었습니다.
*

    * RPM 기반 OS를 사용하는 경우 **udo yum install nfs-utils 를** 실행하십시오 .
    * Debian 기반 OS를 사용하는 경우 **sudo apt-get update && sudo apt-get install nfs-common 을** 수행합니다 .
    *   동시에 시작된 NFS 요청 수를 늘립니다. 동시에 시작된 NFS 요청 수를 128로 변경하려면 다음 명령을 실행하십시오.<br>

        **sudo echo "옵션 sunrpc tcp\_slot\_table\_entries=128" >> /etc/modprobe.d/sunrpc.conf**\
        **sudo echo "옵션 sunrpc tcp\_max\_slot\_table\_entries=128" >> /etc/modprobe.d/sunrpc.conf**



### NFS를 통해 NAS폴더를 Linux에 마운트

시작 시 NFS 파일 시스템을 자동으로 마운트하도록 /etc/fstab 파일을 구성하는 것이 좋습니다. 자동 마운트를 설정하도록 " /etc/rc.local " 파일을 구성할 수도 있습니다 .

**단계**

자동 연결을 구성합니다.

* (권장) " /etc/fstab " 파일을 열고 다음 명령을 추가합니다.

> CentOS 6.x에서 자동 마운트를 구성하는 경우 chkconfg netfs on 명령을 사용하여 부팅 시 netfs 서비스가 실행되도록 합니다.



* NFSv4 호환 파일 시스템을 마운트해야 하는 경우 다음 명령을 추가하십시오.

&#x20;      **file-system-id.region.nas.com:/ /mount-point nfs vers=4 0 0**<br>

* NFSv3 호환 파일 시스템을 마운트해야 하는 경우 다음 명령을 추가합니다.

&#x20;       **file-system-id.region.nas.com:/ /mount-point nfs vers=3 0 0**

* " /etc/rc.local " 파일을 열고 다음 명령을 추가합니다.



> **/etc/rc.local 파일을 구성하기 전에 /etc/rc.local 및 /etc/rc.d/rc.local 파일에 대한 실행 권한이 있는지 확인하십시오.**
>
> **예를 들어 CentOS 7.x에서는 사용자에게 기본적으로 실행 권한이 부여되지 않습니다. /etc/rc.local 파일을 구성하기 전에 사용자에게 실행 권한을 할당해야 합니다.**



* NFSv4 호환 파일 시스템을 마운트해야 하는 경우 다음 명령을 추가하십시오.

&#x20;  **sudo mount -t nfs -o vers=4 file-system-id.region.nas.com:/ /mount-point**<br>

* NFSv3 호환 파일 시스템을 마운트해야 하는 경우 다음 명령을 추가하십시오.

&#x20;   **sudo mount -t nfs -o vers=3 file-system-id-xxxx.region.nas.com:/ /mount-point**

재부팅 Linux 인스턴스를 실행합니다.

마운트 결과를 보려면 **mount-l** 명령을   실행하십시오 .

아래 이미지는 성공적인 마운트의 예입니다.

<figure><img src="https://help.grapecity.com.cn/download/attachments/80954127/image2021-4-9_8-36-10.png?version=1&#x26;modificationDate=1673923299000&#x26;api=v2&#x26;effects=border-simple,blur-border" alt=""><figcaption></figcaption></figure>

파일 시스템이 마운트된 후 **df-h** 명령을 사용하여 파일 시스템의 용량을 볼 수 있습니다.
