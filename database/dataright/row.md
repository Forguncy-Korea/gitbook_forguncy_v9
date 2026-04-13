# 행 권한

테이블에 행 권한을 설정하면 다른 사용자가 테이블의 데이터에 액세스, 편집 및 삭제할 수 있는 다른 권한이 있을 수 있으므로 데이터 보호 역할을 할 수 있습니다.

행 권한은 여러 권한 부여 항목 간의 관계인 권한 부여 항목 집합의 컬렉션으로, 권한 있는 항목의 조건을 충족하는 경우 적절한 권한이 있습니다.

서로 다른 사용자가 다른 사용 권한 작업 테이블의 데이터를 가질 수 있도록 다른 사용자에 대해 서로 다른 행 권한을 설정합니다.

![](https://help.grapecity.com.cn/download/thumbnails/72356890/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1648092598000\&api=v2) 데이터 테이블을 열고 오른쪽 열 테이블 설정에서 행 권한 설정을 선택합니다.

![](<../../.gitbook/assets/image (77).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356890/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1648092598000\&api=v2) 행 권한 활성화에 체크하고 권한 항목 추가를 클릭합니다.

![](<../../.gitbook/assets/image (429).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356890/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1648092598000\&api=v2) 권한 부여를 설정하는 사용자입니다. 기본값은 모든 사용자이며 드롭다운 화살표를 클릭하여 로그인 사용자, 작성자, 작성자의 관리자를 선택합니다.

예를 들어 로그인한 사용자에게 권한 부여를 선택합니다

![](<../../.gitbook/assets/image (1921).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356890/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1648092598000\&api=v2) 행 필터링을 설정하면 조건을 충족하는 레코드가 필터링되어 권한이 있는 사용자가 작업을 수행할 수 있습니다.

다음 그림과 같이 팝업 행 권한 설정 대화 상자에서 "구매자가 로그인한 사용자"와 같은 조건을 설정하려면 클릭합니다.![](https://help.grapecity.com.cn/download/thumbnails/72356890/image2019-2-19_9-55-13.png?version=1\&modificationDate=1648092598000\&api=v2)

![](<../../.gitbook/assets/image (964).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356890/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1648092598000\&api=v2) 보기, 편집 및 삭제를 포함하여 허용되는 작업을 설정합니다.

![](https://help.grapecity.com.cn/download/thumbnails/72356890/%E6%AD%A5%E9%AA%A46.png?version=1\&modificationDate=1648092598000\&api=v2) 데이터 테이블에 하위 테이블이 있는 경우 기본 테이블 사용 권한의 하위 테이블 설정을 선택하여 하위 테이블이 기본 테이블의 사용 권한 설정과 일치하도록 할 수 있습니다.

![](https://help.grapecity.com.cn/download/thumbnails/72356890/%E6%AD%A5%E9%AA%A47.png?version=1\&modificationDate=1648092598000\&api=v2) 설정이 완료되면 페이지를 실행하고 사용자 A  로그인하면 로그인 한 후 구매자가 A 인 주문만 볼 수 있으며 편집 및 삭제 작업을 수행 할 수 있습니다.

{% hint style="info" %}
뷰는 행 권한을 설정할 수도 있지만 허용되는 작업은 볼 수 있는지 여부만 설정할 수 있습니다.
{% endhint %}
