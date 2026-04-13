# Single Sign-On

Single Sign-On을 사용하면 사용자가 다른 응용 프로그램이나 웹 사이트에서 포건시 페이지로 이동할 때 원래 응용 프로그램의 사용자 이름으로 포건시 웹 사이트에 자동으로 로그인할 수 있습니다.

<figure><img src="../../.gitbook/assets/image (2089).png" alt=""><figcaption></figcaption></figure>

### **GetUserToken 인터페이스**

#### 요청방법

Post

#### 매개변수&#x20;

| 매개변수   | 설명            |
| ------ | ------------- |
| 사용자 이름 | 로그인하려면 사용자 이름 |
| 비밀번호   | 싱글 사인온 암호.    |

#### 반환 값&#x20;

토큰을 성공적으로 획득한 경우 반환된 문자열은 "오류:"로 시작하지 않고, 토큰을 얻지 못한 경우 반환된 문자열은 오류 메시지이며 "오류:"로 시작하므로 디버깅 및 위치 지정에 편리합니다.



## SSO 설정&#x20;

포건시 빌더에서 \[파일>옵션>기타 응용 프로그램과 통합]을 선택하여 암호를 보고 설정합니다.&#x20;

<figure><img src="../../.gitbook/assets/image (2090).png" alt=""><figcaption></figcaption></figure>

Single Sign-On을 설정한 후 타사(이동할 수 없는 유형) 애플리케이션에 다음 코드를 작성합니다.

```
var baseUrl = "http://localhost:25979/Forguncy";
var userName = "administrator";
var password = "7FBqkHeV!4Rw";  // 이 비밀번호는 싱글 사인온 비밀번호입니다.
HttpWebRequest rq = HttpWebRequest.Create(baseUrl + "/SSO/GetUserToken") as HttpWebRequest;
rq.Method = WebRequestMethods.Http.Post;
rq.Accept = "application/json";
rq.ContentType = "application/json";
var loginStr = "{userName:\"" + userName + "\", password:\"" + password + "\"}";
var data = Encoding.UTF8.GetBytes(loginStr);
using (Stream stream = rq.GetRequestStream())
{
    stream.Write(data, 0, data.Length);
}
var response = rq.GetResponse();
var token = new StreamReader(response.GetResponseStream()).ReadToEnd();
if(token.StartsWith("Error:"))
{
    MessageBox.Show(token);
    return;
}
Process.Start(baseUrl + "?token=" + token);
```



{% hint style="info" %}


* **이 기능은 Windows 도메인 사용자는 이 기능을 사용할 수 없습니다.**
* **로그인해야 하는 사용자 이름은 포건시 애플리케이션에 이미 존재해야 합니다.**
* **보안을 위해 싱글 사인온 암호는 유출을 방지하기 위해 JavaScript 코드에 나타나지 않아야 합니다.**
{% endhint %}

### **공개된 비밀번호를 직접 수정**

아래 그림과 같이 서버의 "C:\Users\Public\Documents\ForguncyServer\ _Application Name_ " 디렉토리 에서 "Config.xml" 구성 파일을 찾아 Password 값을 열고 편집합니다.

<figure><img src="../../.gitbook/assets/image (2091).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
* **Password 노드에 값이 없으면 빌더를 열 때 암호가 임의로 생성된다는 의미입니다.**
* **수정 후 사이트를 다시 시작해야 합니다.**
{% endhint %}
