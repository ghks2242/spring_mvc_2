# spring_mvc_2
스프링 MVC2 편 

---
### 타임리프 특징
* 서버사이드 HTML 렌더링 (SSR)
* 네츄럴 템플릿
* 스프링 통합 지원

#### 네츄럴 템플릿
타임리프는 순수 HTML 최대한 유지하는 특징이 있다.
타임리프로 작성한 파일은 HTML 을 유지하기 떄문에 웹 브라우저에서 파일을 직접 열어도 내용을 확인할 수 있고,
서버를 통해 뷰템플릿을 거치면 동적으로 변경된 결과를 확인할 수 있다.

JPS를 포함한 다른 뷰 템플릿들은 해당 파일을 열면 예를들면 JSP 파일 자체를 그대로 웹 브라우저에서 열어보면 
JSP 코드와 HTML 소스가 뒤죽박죽 섞여서 웹 브라우저에서 정상적인 HTML 결과를 확인할수 없다. 
오직 서버를 통해서 JSP 가 렌더링 되고 HTML 응답 결과를 받아야 화면을 확인할 수 있다

반면  타임리프로 작성된 파을은 해당파일을 그대로 웹 브라우저에서 열어도 정상적인 HTML 결과를 확인할수있다. 물론 이 경우 동적으로 결과가 렌더링되지는 않는다
하지만 HTML 마크업 결과가 어떻게 되는지 파일만 열어도 바로 확인할수있다.
이렇게 순수 HTML 그대로 유지하먄서 뷰 템플릿도 사용할수있는 타임리프 특징을 네츄럴 템플릿 이라한다.

#### 스프링 통합 지원
타임리프는 스프링과 자연스럽게 통합되고 스프링의 다양한기능을 편리하게 사용할수있게 지원한다 

### 타임리프 기본 기능

#### 타임리프 선언
<html xmlns:th="http://www.thymleaf.org">

# 타임리프 기본 표현식

    • 간단한 표현:
        ◦ 변수 표현식: ${...}
        ◦ 선택 변수 표현식: *{...}
        ◦ 메시지 표현식: #{...}
        ◦ 링크 URL 표현식: @{...}
        ◦ 조각 표현식: ~{...}
    • 리터럴
        ◦ 텍스트: 'one text', 'Another one!',…
        ◦ 숫자: 0, 34, 3.0, 12.3,…
        ◦ 불린: true, false
        ◦ 널: null
        ◦ 리터럴 토큰: one, sometext, main,…
    • 문자 연산:
        ◦ 문자 합치기: +
        ◦ 리터럴 대체: |The name is ${name}|
    • 산술 연산:
        ◦ Binary operators: +, -, *, /, %
        ◦ Minus sign (unary operator): -
    • 불린 연산:
        ◦ Binary operators: and, or
        ◦ Boolean negation (unary operator): !, not
    • 비교와 동등:
        ◦ 비교: >, <, >=, <= (gt, lt, ge, le)
        ◦ 동등 연산: ==, != (eq, ne)
    • 조건 연산:
        ◦ If-then: (if) ? (then)
        ◦ If-then-else: (if) ? (then) : (else)
        ◦ Default: (value) ?: (defaultvalue)
    • 특별한 토큰:
        ◦ No-Operation: _


---
## Escape
HTML 문서는 '<', '>' 같은 특수 문자를 기반으로 정의된다. 
따라서 뷰 템플릿으로 HTML 화면을 생성할때는 출력하는 데이터에 이러한 특수문자가있는것을 주의해서 사용해야한다

    # 변경전
    "Hello Spring"

    # 변경후
    "Hello <b>Spring</b>"
    
    <b> 태그를 사용해서 Spring 이라는 단어가 진하게 나오도록 해보자

* 웹 브라우저 : Hello <b>Spring</b>
* 소스보기 : Hello &lt;b&gt;Spring&lt;/b&gt;

개발자가 의도한것은 <b> 태그가있으면 부분을 강조하는 것이다 
그런데 <b> 태그가 그대로 나온다 소스보기를하면 '<' 태그가 &lt; 로 변경된것을 확인할수잇다

### HTML 
웹 브라우저는 '<' 를 HTML 태그의 시작으로 인식한다 따라서 '<' 를 태그의 시작이 아니라 문자로 표현할수있는 방법이 필요한데,
이것을 HTML 엔티티라고 한다. 그리고 이렇게 HTML 에서 사용하는 특수문자를 HTML 엔티티로 변경하는것을 이스케이프(escape) 라한다
타임리프가 제공하는 th:text, [[...]] 는 기본적으로 이스케이프를 제공한다


---
## SpringEL 의 다양한 표현식 사용
### Object
* user.username : user의 username 을 프로퍼티 접근 -> user.getUsername() 
* user['username'] : 위와 동일 -> user.getUsername()
* user.getUsername : user 의 getUsername() 을 직접호출

### List
* users[0].username : List 의 첫번째 회원을 찾고 username 프로퍼티 접근 -> list.get(0).getUsername()
* users[0].['username'] : 위와 동일
* users[0].getUsername() : List 의 첫번째 회원을 찾고 메서드 직접 호출

### Map
* userMap['userA'].username : Map 에서 userA 를 찾고 username 프로퍼티 접근 -> map.get("userA").getUsername();
* userMap['userA']['username'] :  위와 동일
* userMap['userA'].getUsername() : Map 에서 userA 찾고 메서드 직접호출
* userMap.userA.username : 위와 동일


### 지역변수선언
th:with 을 사용하면 지역변수를 선언하여 사용할수있다 지역변수는 선언된 태그안에서만 사용가능하다

    <h1>지역변수 - (th:with)</h1>
    <div th:with="first=${users[0]}">
    <p>처음 사람 이름은 <span th:text="${first.username}"></span></p>
    </div>


--- 
## 기본객체들
타임리프는 기본 객체들을 제공한다.
* ${#request}
* ${#response}
* ${#session}
* ${#servletContext}
* ${#locale}
그런데 #request 는 HttpServletRequest 객체가 그대로 제공되기 떄문에 데이터를 조회하려면 
request.getParameter("data") 처럼 불편하게 접근해야한다.

## thymeleaf-3.1.x부터는 보안성과 명확성을 위해 위 객체들을 Expression Utility Object (#request, #session, 등)로 직접 접근할 수 없게 되었습니다

이런 점을 해결하기 위한 편의 객체도 제공한다.
* HTTP 요청 파라미터 접근 param
    * ex) ${param.paramData}
* HTTP 세션 접근 session
    * ex) ${session.sessionData}
* 스프링 빈 접근 @
    * ex) ${@helloBean.hello('Spring!')}