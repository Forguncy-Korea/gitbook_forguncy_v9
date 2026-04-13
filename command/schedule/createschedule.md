# 예약된 작업 만들기

포건시는 일정 작업을 정의하고 정기적으로 앱이 일부 백그라운드 작업을 수행하도록 트리거할 수 있도록 지원합니다. 이 섹션에서는 예약된 작업을 만드는 방법에 대해 설명합니다.



예약 작업 기능은 개체 관리자에 기본적으로 표시되지 않는 고급 기능입니다 . 프토젝트 탐색기의 ![](https://help.grapecity.com.cn/download/thumbnails/80946065/image2023-1-9_11-57-14.png?version=1\&modificationDate=1673923165000\&api=v2)버튼을 클릭하면,  개체선택 창이작뜹니다.  개체선택창에서  예약작업의 확인란을 선택하고 "확인"을 클릭하면 개체 관리자에 예약된 작업이 표시됩니다.

<figure><img src="../../.gitbook/assets/image (97).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (1449).png" alt=""><figcaption></figcaption></figure>

## 예약된 작업 만들기&#x20;

다음은 서비스 쪽 가져오기 내보내기 CSV를 만드는 예약된 작업을 예로 들어 예약된 작업을 만드는 자세한 작업을 보여 드리겠습니다.

&#x20;![](https://help.grapecity.com.cn/download/thumbnails/72357087/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1648092601000\&api=v2)  왼쪽의 예약 작업의 탭을 마우스 오른쪽 버튼으로 클릭하고 예약된 작업 생성을 선택하면 예약된 작업을 만드는 대화 상자가 나타납니다.

폴더 만들기를 선택하여 폴더에 예약된 작업을 만들 수도 있습니다.

또는 리본 메뉴에서 \[만들기]>\[예약작업]에서 대화상자를 뜨게 할 수 있습니다.

![](<../../.gitbook/assets/image (1181).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357087/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1648092601000\&api=v2)  예약 작업 만들기에서 편집합니다. 일반 설정을 사용하면 나중에 자신 또는 다른 사용자가 쉽게 유지 관리할 수 있도록 예약된 작업의 이름과 설명을 설정할 수 있습니다.

![](<../../.gitbook/assets/image (1437).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357087/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1648092601000\&api=v2) 타이밍 작업의 트리거를 편집하여 이 타이밍 작업에 대한 트리거를 설정합니다.

![](<../../.gitbook/assets/image (1463).png>)

* 일정 계획: 실행 빈도를 한 번/매시간/일별/주별/월별로 설정하고 고급 설정에서 만료 날짜를 설정할 수 있습니다.
* 실행된 예약 작업: 명령에서 사용할 수 있는 다음 매개 변수가 자동으로 생성됩니다. 매개 변수 이름은 수정을 지원합니다.

![](<../../.gitbook/assets/image (247).png>)

* 실행된 서버단 명령: 명령에서 사용할 수 있는 다음 매개 변수가 자동으로 생성됩니다. 매개 변수 이름은 수정을 지원합니다.

![](<../../.gitbook/assets/image (883).png>)

* 실행된 외부 테이블이 저장 되면 명령에서 사용할 수 있는 다음 매개 변수가 자동으로 생성됩니다. 매개 변수 이름은 수정을 지원합니다.

![](<../../.gitbook/assets/image (1471).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357087/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1648092601000\&api=v2) 예약된 작업에 의해 실행되는 명령을 편집합니다. \[명령 편집] 하이퍼링크를 클릭하고 예약된 작업 편집 대화 상자를 표시하고 CSV 가져오기/내보내기를 선택합니다.

CSV 작업을 선택하고 데이터 테이블 및 CSV의 파일 경로를 선택합니다.

![](<../../.gitbook/assets/image (65).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357087/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1648092601000\&api=v2) 설정이 완료되면 개체 관리자의 예약된 작업 탭에서 생성된 예약된 작업을 볼 수 있습니다.

예약된 작업 앞에 있는 작업을 클릭하여 확장합니다.![](https://help.grapecity.com.cn/download/thumbnails/72357087/image2020-9-18_16-6-47.png?version=1\&modificationDate=1648092601000\&api=v2)계획 작업을 관리하고 일반/트리거/명령을 두 번 클릭하면 해당 탭이 직접 팝업되므로 보고 수정할 수 있습니다.

예약된 작업을 선택하고 마우스 오른쪽 버튼을 클릭하면 예약된 작업을 사용하지 않도록 설정하거나 예약된 작업을 복사하도록 선택할 수 있는 마우스 오른쪽 버튼 클릭 메뉴가 나타납니다.

![](<../../.gitbook/assets/image (75).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72357087/%E6%AD%A5%E9%AA%A46.png?version=1\&modificationDate=1648092601000\&api=v2) 설정이 완료되면 앱을 실행하거나 게시하고 설정된 시간에 CSV 내보내기 명령을 실행합니다.

## 예약된 작업 테스트 하기&#x20;

예약된 작업을 만든 후 빌더에서 직접 타이밍 작업을 테스트할 수 있으며 앱을 실행하지 않고도 예약된 작업의 설정을 테스트할 수 있습니다.

![](<../../.gitbook/assets/image (463).png>)

테스트 버튼을 클릭하면 예약된 작업의 명령이 즉시 실행됩니다. 예약된 작업에서 명령을 실행하면 테스트 결과가 표시됩니다. 결과에는 "반환 코드" 및 "로그"가 포함됩니다.

![](<../../.gitbook/assets/image (743).png>)

&#x20;**빌더에서 정기적으로 명령 실행**&#x20;

이 옵션을 선택하면 빌더에서 예약된 작업에 대한 명령이 예약되고 기본적으로 선택되지 않습니다.

![](<../../.gitbook/assets/image (1261).png>)
