
## 타임리프 - 기본 기능
### 타임리프 특징
- 서버 사이드 HTML 렌더링(SSR)
- 네츄럴 템플릿
- 스프링 통합 지원

#### 서버 사이드 HTML 랜더링(SSR)
- 타임리프는 백엔드 서버에서 BTML을 동적으로 렌더링 하는 용도로 사용한다.

#### 네츄럴 템플릿
- 타임리프는 순수 HTML을 최대한 유지하는 특징이 있다.
- 타임리프로 작성한 파일은 HTML을 유지하기 때문에 웹 브라우저에 파일을 직접 열어도 내용을 확인할 수 있고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.
- JSP를 포함한 다른 뷰 템플릿들은 해당 파일을 열면, 예를 들어서 JSP 파일 자체를 그대로 웹 브라우저에서 열어보면 JSP 소스코드와 HTML이 뒤죽박죽 얶여서 웹 브라우저에서 정상적인 HTML 결과를 확인할 수 없다. 오직 서버를 통해서 JSP가 렌더링 되고 HTML 으답 결과를 받아야 확인을 할 수 있다.
- 반면에 타임리프로 작성된 파일은 해당 파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할 수 있다. 물론 이 경우 동적으로 결과가 렌더링 되지는 않는다. 하지만 HTML 마크업 결과가 어떻게 되는지 파일만 열어도 바로 확인할 수 있다.
- 이렇게 순수 HTML을 그대로 유지하면서 뷰 템플릿도 사용할 수 있는 타임리프의 특징을 네츄럴 템플릿 이라 한다.

#### 스프링 통합 지원
- 타임리프는 스프링과 자연스럽게 통합되고, 스프링의 다양한 기능을 편리하게 사용할 수 있게 지원한다. 이 부분은 스프링 통합과 폼 장에서 자세히 알아보겠다.

#### 타임리프 사용 선언
```javascript
 <html xmlns:th="http://www.thymeleaf.org">
```

### 텍스트 - text, utext
#### Escape
- HTML 문서는 <,> 같은 특수 문자를 기반으로 정의된다. 따라서 뷰 템플릿으로 HTML 화면을 생성할 때는 출력하는 데이터에 이러한 특수 문자가 있는 것을 주의해서 사용해야 한다.
#### HTML 엔티티
- 웹 브라우저는 <를 HTML 테그의 시작으로 인식한다. 따라서 <를 테그의 시작이 아니라 문자로 표현할 수 있는 방법이 필요한데, 이것을 HTML 엔티티라 한다. 그리고 이렇게 HTML에서 사용하는 특수 문자를 HTML 엔티티로 변경하는 것을 이스케이프(escape)라 한다. 그리고 타임리프가 제공하는 th:text, [[...]]는 기본적으로 이스케이프(escape)를 제공한다.
    ```
    <  →  &lt;
    >  →  &gt;
    ```
  
#### Unescape
- 타임리프는 다음 두 기능을 제공한다.
    ```
    th:text  →  th:utext
    [[...]]  →  [(...)]
    ```

### 변수 - SpringEL
- 타임리프에서 변수를 사용할 때는 변수 표현식을 사용한다. 변수 표현식: ${...}
#### SprigEL 다양한 표현식 사용
- Object
  - user.username: user의 username을 프로퍼티 접근 -> user.getUsername()
  - user.['username']: 위와 같음 -> user.getUsername()
  - user.getUsername(): user의 getUsername()을 직접 호출
- List
  - users[0].username: List에서 첫 번째 회원을 찾고 username 프로퍼티 접근 -> list.get(0).getUsername()
  - users[0]['username']: 위와 같음
  - users[0].getUsername(): List에서 첫 번째 회원을 찾고 메서드 직접 호출
- Map
  - userMap['userA].username: Map에서 userA를 찾고, username 프로퍼티 접근 -> map.get("userA").getUsername()
  - userMap['userA']['username']: 위와 같음
  - userMap['userA'].getUsername(): Map에서 userA를 찾고 메서드 직접 호출
  
