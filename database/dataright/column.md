# 열 권한

필드 권한은 사용자가 액세스할 수 있는 필드를 제어하는 열 권한입니다. 테이블에 필드 권한을 설정하면 다른 사용자가 다른 필드에 액세스하고 편집할 수 있는 다른 권한을 가질 수 있으므로 데이터 보호 역할을 할 수 있습니다.

필드 권한은 여러 권한 부여 항목 간의 관계인 권한 부여 항목 집합의 컬렉션으로, 권한 있는 프로젝트 중 하나의 조건을 충족하는 경우 적절한 권한이 있습니다.&#x20;

다른 사용자가 다른 필드에 대해 서로 다른 작업 권한을 가질 수 있도록 필드 권한을 설정하여 데이터 보호 효과를 얻을 수 있습니다.

![](https://help.grapecity.com.cn/download/thumbnails/72356919/%E6%AD%A5%E9%AA%A41.png?version=1\&modificationDate=1648092598000\&api=v2) 데이터 테이블을 열고 오른쪽 열 테이블 설정에서 열 권한 편집을 선택합니다.

![](<../../.gitbook/assets/image (1953).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356919/%E6%AD%A5%E9%AA%A42.png?version=1\&modificationDate=1648092598000\&api=v2) 열 권한 활성화 선택하고 권한 부여 추가를 클릭하여 권한 부여된 사용자, 조건 및 허용된 작업을 설정합니다.

권한 제어 행을 선택하고 권한 항목 삭제를 클릭하여 삭제합니다.

![](<../../.gitbook/assets/image (880).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356919/%E6%AD%A5%E9%AA%A43.png?version=1\&modificationDate=1648092598000\&api=v2) 권한 부여를 설정하는 사용자입니다. 기본값은 모든 사용자이며 드롭다운 화살표를 클릭하여 로그인 사용자, 작성자, 작성자의 관리, 사용자 역할, 사용자 유형 필드의 상위를 선택합니다.

예를 들어 로그인한 사용자에게 권한을 부여하도록 선택합니다.

![](<../../.gitbook/assets/image (494).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356919/%E6%AD%A5%E9%AA%A44.png?version=1\&modificationDate=1648092598000\&api=v2) 조건을 충족하는 레코드가 필터링되어 권한이 있는 사용자가 작업을 수행할 수 있도록 조건을 설정합니다.

다음 그림과 같이 팝업 열 권한 조건 편집 대화 상자에서  "구매자가 로그인한 사용자"로 설정하는 등의 조건을 설정하려면 클릭합니다.![](https://help.grapecity.com.cn/download/thumbnails/72356919/image2019-2-19_9-55-13.png?version=1\&modificationDate=1648092598000\&api=v2)

![](<../../.gitbook/assets/image (589).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356919/%E6%AD%A5%E9%AA%A45.png?version=1\&modificationDate=1648092598000\&api=v2) 모두 보기 및 편집 가능을 포함하여 허용되는 작업을 설정합니다. 필드 사용 권한에서 필드를 삭제할 수 없습니다. ![image2019-2-19\_9-55-13.png](http://help.grapecity.com.cn/download/attachments/30247413/image2019-2-19_9-55-13.png?version=1\&modificationDate=1550541313000\&api=v2)을 클릭하면 대화 상자에 모든 필드와 테이블의 하위 테이블이 나열되고 개별 필드 및 하위 테이블에 대한 작업 권한이 설정됩니다.

아래 그림과 같이 로그인한 사용자를 현재 사용자로 설정하면 모든 필드를 보고 편집할 수 있습니다.

![](<../../.gitbook/assets/image (966).png>)

다음 그림과 같이 고객 이름 필드를 제외한 모든 레코드를 보고 편집할 수 있는 모든 사용자로 설정된 권한을 추가합니다.

![](<../../.gitbook/assets/image (55).png>)

![](https://help.grapecity.com.cn/download/thumbnails/72356919/%E6%AD%A5%E9%AA%A46.png?version=1\&modificationDate=1648092598000\&api=v2) 설정이 완료되면 페이지를 실행하고 사용자 유재석 사용하여 로그인하면 관리자 역할이기 때문, 로그인하면 고객 이름 필드를 포함한 모든 주문 데이터를 볼 수 있습니다.

![](<../../.gitbook/assets/image (1541).png>)

조세호로 로그인하면 관리자 역할이 아니기 때문에 로그인 후 모든 주문을 볼 수 있지만 다른 고객의 주문 은 고객이름을 볼 수 없다.

![](<../../.gitbook/assets/image (963).png>)

{% hint style="info" %}
뷰는 필드 권한을 설정할 수도 있지만 허용되는 작업은 모두 보거나 볼 수 있는 필드를 설정할지 여부만 설정할 수 있습니다.
{% endhint %}
