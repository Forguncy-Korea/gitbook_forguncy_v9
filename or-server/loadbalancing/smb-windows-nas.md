# SMB를 통해 Windows에서 NAS 폴더에 액세스

SMB를 통해 Windows에서 NAS 폴더에 액세스하는 방법을 소개합니다.



### 전제조건&#x20;

* 파일 시스템이 생성되었습니다.
* 마운트 지점이 생성되었습니다(도메인 계정에 대한 읽기 및 쓰기 권한 부여 또는 익명에 대한 읽기 및 쓰기 권한 부여).



### SMB를 통해 Windows에서 NAS폴더에 엑세스&#x20;

![](https://help.grapecity.com.cn/download/thumbnails/80954141/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1673923299000\&api=v2)  도메인 사용자를 사용하여 nas 액세스 권한을 부여하는 경우 ForgancyUserService 서비스의 시작 계정을 해당 도메인 사용자로 변경하십시오.

①Windows 시작 메뉴를 열고 "제어판->시스템 및 보안->관리 도구->서비스"를 선택하고 목록에서 "ForguncyServerService"를 찾아 두 번 클릭합니다.

<figure><img src="../../.gitbook/assets/image (546).png" alt=""><figcaption></figcaption></figure>

②대화 상자에서 "로그인" 탭을 선택하고 "이 계정"을 선택한 다음 도메인 사용자 이름과 암호를 입력합니다.

<figure><img src="../../.gitbook/assets/image (892).png" alt=""><figcaption></figcaption></figure>

③ 이동식 그리드 서버를 재시작합니다. "ForgancyServerService"를 마우스 오른쪽 버튼으로 클릭하고 "다시 시작"을 선택합니다.

<figure><img src="../../.gitbook/assets/image (874).png" alt=""><figcaption></figcaption></figure>

![](https://help.grapecity.com.cn/download/thumbnails/80954141/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1673923299000\&api=v2)  SMB 공유를 활성화하면 UNC 경로(예: \\\10.32.4.4\fgcnas)를 얻습니다. 이 경로는 공유 스토리지 경로를 구성할 때 사용해야 합니다.
