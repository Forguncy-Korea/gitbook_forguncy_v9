# 기본유형

서버단 명령을 만들 때 기본 형식 및 배열 형식을 포함 하는 형식의 파라미터를 설정할 수 있습니다. 이 섹션에서는 문자열, 숫자, 날짜 또는 기타 간단한 형식의 데이터와 같은 기본 형식의 파라미터에 대해 설명합니다.

<figure><img src="../../../.gitbook/assets/image (114).png" alt=""><figcaption></figcaption></figure>

## 기본형식의 파라미터&#x20;

서버단 명령을 만들 때 서버단 명령 편집 대화 상자에서 파라마터 탭을 선택하고 파라미터 추가를 클릭한 다음 기본 매개 변수 유형을 기반으로 파라미터 이름 및 파라미터 유형을 설정할 수 있습니다.

예를 들어 직원을 추가하는 서버단 명령을 만듭니다.

![](https://help.grapecity.com.cn/download/thumbnails/72357211/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1648092603000\&api=v2) "직원 추가"라는 이름의 서버단 명령을 만든 다음 파라미터를 설정하고 다음 그림과 같이 3가지 기본 형식의 파라미터를 추가합니다.

![](<../../../.gitbook/assets/image (182).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357211/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1648092603000\&api=v2) 파라미터에 대한 데이터 유효성 검사를 설정합니다. 다음 그림과 같이 파라미터 뒤에 있는 데이터 유효성 검사 아이콘을 클릭하여 팝업 데이터 유효성 검사 대화 상자에서 설정합니다.![](https://help.grapecity.com.cn/download/thumbnails/72357211/image2020-9-17_14-33-36.png?version=1\&modificationDate=1648092603000\&api=v2)

![](<../../../.gitbook/assets/image (210).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357211/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1648092603000\&api=v2) 파라미터를 설정한 후 명령을 편집하고 데이터 테이블 작업 명령을 선택합니다. 작업 유형은 추가, 필드 추가, 필드 값 설정에 대한 해당 매개 변수입니다.

![](<../../../.gitbook/assets/image (1631).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357211/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1648092603000\&api=v2) 설정이 완료되면 이 서버단 명령을 호출할 수 있습니다.

예를 들어 페이지에서 셀 범위를 선택하고 버튼을 설정합니다. 버튼의 명령을 편집하고 "서버단 명령 호출"으로 명령을 선택한 다음 서버단 명령 뒤에 있는 드롭다운을 클릭하고 드롭다운 목록에서 "직원 추가"라는 서버단 명령을 선택합니다.

"직원 추가" 서비스 쪽 명령에 파라미터가 있기 때문에 모든 파라미터가 자동으로 나열되므로 파라미터를 설정해야 하며 파라미터는 페이지의 해당 셀입니다. 그런 다음 "호출이 성공한 후 데이터 다시 로드"를 선택하여 데이터를 추가한 후 페이지가 새로 고쳐지고 추가된 데이터가 표시됩니다.

![](<../../../.gitbook/assets/image (1436).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357211/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1648092603000\&api=v2) 실행 후 페이지의 주문날짜 텍스트상자 외  값을 입력한 후 직원 추가 버튼을 클릭하면 매개 변수의 데이터 유효성 검사에 설정된 오류 경고가 표시되는 경고 상자가 나타납니다.

![](<../../../.gitbook/assets/image (543).png>)

주문날짜 텍스트 상자에 값을 입력한 다음 직원 추가 버튼을 클릭하면 페이지에 설정된 값이 데이터 테이블에 데이터를 추가하는 데이터 테이블 작업 명령을 실행하는 서비스 쪽으로 전달됩니다.

![](<../../../.gitbook/assets/image (1123).png>)

