
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
  

### 기본 객체들
- 타임리프는 기본 객체들을 제공한다.
  - ${#request}
  - ${#response}
  - ${#session}
  - ${#servletContext}
  - ${#locale}
- 그런데 #request는 HttpServletRequest 객체가 그대로 제공되기 때문에 데이터를 조회하려면 request.getParameter("data") 처럼 불편하게 접근해야 한다.
- 이런 점을 해결하기 위해 편의 객체도 제공한다.
  - HTTP 요청 파라미터 접근: param   예) ${param.paramData}
  - HTTP 세션 접근: session   예) ${session.SessionData}
  - 스프링 빈 접근: @   예) ${@helloBean.hello('Spring!')}
  

### URL 링크
- 타임리프에서 URL을 생성할 때는 @{...}문법을 사용하면 된다.
- 단순한 url
  - @{/hello} -> /hello
- 쿼리 파라미터
  - @{/hello(param1=${param1}, param2=${param2})} -> /hello?param1=data&param2=data2
  - ()에 있는 부분은 쿼리 파라미터로 처리된다.
- 경로변수
  - @{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})} -> /hello/data1/data2
  - URL 경로상에 변수가 있으면 () 부분은 경로 변수로 처리된다.
- 경로 변수 + 쿼리 파라미터
  - @{/hello/{param1}(param1=${param1}, param2=${param2})} -> /hello/data1?param2=data2
  - 경로 변수와 쿼리 파라미터를 함께 사용할 수 있다.
- 상대경로, 절대경로, 프로토콜 기준을 표현할 수 도 있다.
  - /hello: 절대 경로
  - hello: 상대 경로

### 리터럴
- 리터럴은 소스 코드상에 고정된 값을 말하는 용어이다.
- 예를 들어서 다음 코드에서 "Hello"는 문자 리터럴, 10, 20는 숫자 리터럴이다.
  ```java
  String a = "Hello"
  int a = 10 * 20
  ```
  
- 타임리프는 다음과 같은 리터럴이 있다.
  - 문자: 'hello'
  - 숫자: 10
  - 불린: true, false
  - null: null
  
- 타임리프에서 문자 리터럴은 항상 `'` (작은 따옴표)로 감싸야 한다.
  - `<span th:"'hello'">`

### 연산
- 타임리프 연산은 자바와 크게 다르지 않다. HTML안에서 사용하기 떄문에 HTML 엔티티를 사용하는 부분만 주의하자.
- 비교연산: HTML 엔티티를 사용해야 하는 부분을 주의하자
  ```
  >(gt), <(lt), >=(ge), <=(le), !(not), ==(eq), !=(neq,ne)
  ```
- 조건식: 자바의 조건식과 유시하다.
- Elvis 연산자: 조건식의 편의 버전
- No-Operation: _인 경우 마치 타임리프가 실행되지 않는 것 처럼 동작한다. 이것을 잘 활용하면 HTML의 내용 그대로 활용할 수 있다.

### 속성 값 설정
#### 타임리프 태그 속성(Attribute)
- 타임리프는 주로 HTML 태그에 th:* 속성을 지정하는 방식으로 동작한다. th:*로 속성을 적용하면 기존 속성을 대체한다. 기존 속성이 없으면 새로 만든다.

- 속성 추가
  - th:attrappend: 속성 값의 뒤에 값을 추가한다.
  - th:attrprepend: 속성 값의 앞에 값을 추가한다.
  - th:classappend: class 속성에 자연스럽게 추가한다.
- checked 처리
  - HTML에서 checked 속성은 checked 속성의 값과 상관없이 checked라는 속성만 있어도 체크가 된다. 이런 부분이 true, false 값을 주로 사용하는 개발자 입장에서는 불편하다.
  - 타임리프의 th:checked는 값이 false인 경우 checked 속성 자체를 제거한다.