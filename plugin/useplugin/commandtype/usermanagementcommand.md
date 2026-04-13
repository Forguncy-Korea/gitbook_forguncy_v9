# 사용자 관리 기능 (UserManagementCommand)

워크플로우 명령을 통해 워크플로우를 전송할 수 있으므로 전체 프로세스를 보다 정밀하게 제어할 수 있습니다. 워크플로우 명령 플러그인에는 워크플로우 명령 및 워크플로우 처리명령이 포함됩니다.

### 플러그인 다운로드&#x20;

버전에 맞는 플러그인을 다운로드 합니다.

<table><thead><tr><th width="139">버전 </th><th>다운로드 링크 </th></tr></thead><tbody><tr><td>v 9.0</td><td><a href="https://forguncy-korea.github.io/attached_files/Plugin_Files/V9_Plugin/UserManagementCommands.zip">UserManagementCommands.zip</a></td></tr><tr><td>v 7. 1</td><td><a href="https://forguncy-korea.github.io/attached_files/Plugin_Files/V7.1_Plugin_20211223/UserManagementCommands.zip">UserManagementCommands.zip</a></td></tr><tr><td>v 7. 0</td><td><a href="https://forguncy-korea.github.io/attached_files/Plugin_Files/V7_Plugin_20210722/UserManagementCommands.zip">UserManagementCommands.zip</a></td></tr><tr><td>v 6. 1</td><td><a href="https://forguncy-korea.github.io/attached_files/Plugin_Files/V6.1_Plugin_20201111/UserManagementCommands.zip">UserManagementCommands.zip</a></td></tr></tbody></table>

### 사용 방법&#x20;

1. 플러그인을 다운로드합니다.
2. Forguncy Builder에서 설치하고 Forguncy Builder를 다시 실행합니다.
3. 서버단 명령에서 아래와 같이 사용자 관리 명령을 사용할 수 있습니다.

<figure><img src="../../../.gitbook/assets/image (552).png" alt=""><figcaption></figcaption></figure>

### 사용 예제&#x20;

아래 예제를 통해 사용자 추가 명령을 사용하는 방법을 확인할 수 있습니다.&#x20;

1.  &#x20;페이지에 사용자이름, 전체이름, 비밀번호, 이메일을 입력할 수 있는 텍스트 상자와 사용자 추가 버튼을 생성합니다.&#x20;

    <figure><img src="../../../.gitbook/assets/image (236).png" alt=""><figcaption></figcaption></figure>
2. 프로젝트 탐색기에서 서버단 명령에 오른쪽 마우스 버튼을 클릭하여 "서버단 명령 생성하기"를 클릭합니다.

<figure><img src="../../../.gitbook/assets/image (400).png" alt=""><figcaption></figcaption></figure>

3\. 서버단 명령 실행 대화상자에서 아래와 같이 파라미터를 추가합니다.

<figure><img src="../../../.gitbook/assets/image (985).png" alt=""><figcaption></figcaption></figure>

4\. 명령에서 "명령편집"을 클릭하여 서버단 명령 실행 대화상자를 열어줍니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (1472).png" alt=""><figcaption></figcaption></figure>

5\. 서버단 명령 실행 대화상자에서 3번에서 생성한 파라미터들을 아래와 같이 지정해줍니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (1318).png" alt=""><figcaption></figcaption></figure>

6\. "사용자 추가"버튼을 클릭한후, 우측 셀유형에 "명령편집"을 클릭합니다.

&#x20;    명령창에서 "서버단 명령 호출" 명령을 선택하고, 파라미터 이름과 값을 설정해줍니다.

<figure><img src="../../../.gitbook/assets/image (1909).png" alt=""><figcaption></figcaption></figure>

7\. 실행하여 사용자이름, 전체이름, 비밀번호, 이메일을 입력한 후, 사용자 추가버튼을 클릭합니다.

Forguncy 서버 관리자 개발에서 사용자 추가가 된 것을 확인할 수 있습니다.&#x20;

<figure><img src="../../../.gitbook/assets/image (986).png" alt=""><figcaption></figcaption></figure>
