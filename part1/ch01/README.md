# 오늘날의 보안

성능, 확장성, 가용성, 보안과 같은 소프트웨어의 비기능적 특징은 시간이 지남에 따라 단기,장기적 영향을 미친다.

보안의 여러가지 개념과 앞으로 배울 내용들을 간단하게 정리

## 1.1 스프링 시큐리티: 개념과 장점

스프링 시큐리티는 다른 종류의 프레임워크와 마찬가지로 원하는 기능을 더 적은 코드로 구현할 수 있게 해준다.

스프링 시큐리티는 **애플리케이션 수준**의 보안을 구현할 수 있게 해주는 프레임워크다. 그러나 스프링 시큐리티를 이해하고

사용하는 것은 개발자의 책임이다. 

보안 구성의 복잡도가 높아지면 유지 보수 비용 및 성능에 영향을 미칠 수 있다.

## 1.2 소프트웨어 보안이란?

보안은 계층별(애플리케이션, 네트워크, 스토리지, 배포 등등)로 적용해야 하며 각기 다른 접근 방식이 필요하다.

스프링 시큐리티는 애플리케이션 수준의 보안을 구현한다. 11P 그림참고
```
인증 - 어플리케이션 사용자 식별

권한 부여 - 인증된 사용자가 특정 기능과 데이터 이용 권리가 있는지 확인.
```
```
저장 데이터 - 스토리지에 저장된 데이터

전송 데이터 - 현 위치에서 다른 위치로 교환중인 데이터

데이터 보안은 데이터의 유형에 따라 다르게 적용해야 한다.
```

실행 중인 애플리케이션은 내부 메모리도 관리해야 한다 내부 힙 메모리에 중요한 정보를 장시간 보관한다면

보안 취약성의 원인이 된다.

## 1.3 보안이 중요한 이유는 무엇인가?

취약성을 예방하는 것이 공격 받은 후 대처하는 것보다 비용이 적게 든다.

## 1.4 웹 애플리케이션의 일반적인 보안 취약성

해커는 공격을 시작하기 전 애플리케이션의 취약성을 파악하고 공략한다.

***일반적인 취약성 목록***
 
* 인증과 권한 부여의 취약성

인증의 취약성이 있다는 것은 사용자가 악의를 가지고 다른 사람의 기능이나 데이터에 접근할 수 있다는 의미이다.

* 세션 고정이란?

특정 사용자의 세션 ID 값을 탈취해서 특정 사용자의 권한을 부여받는 것을 말한다. 기존 세션이 재사용될 때 발생

* XSS(교차 사이트 스크립팅)이란?

악성 스크립트를 만들고 웹 사이트에 노출시켜서 사이트를 이용하는 사용자들이 악성 스크립트에 노출되는 것을 말한다. 

(원치 않는 스크립트를 실행시키게 함)

* CSRF(사이트 간 요청 위조)란?

특정 작업을 호출하는 URL 을 추출해서 외부에서 재사용한다.

사용자가 자신의 계정에 로그인한 후 위조된 코드가 포함된 페이지에 접근하면 악성 코드가 사용자를 대신해서 동작한다.

* 웹 애플리케이션의 주입(Injection) 취약성 이해

클라이언트 스크립트, SQL, XPath, OS 명령, LDAP 등이 주입의 대상이 된다.

* 민감한 데이터의 노출 처리하기

공개 정보가 아닌 민감한 정보는 절대 로그에 기록하지 말아야 한다.
```
[오류] 요청의 서명이 잘못되었습니다. 사용할 올바른 키는 x 입니다.
[경고] 사용자 이름 x 와 암호 y 를 이용하여 로그인하지 못했습니다. 사용자 이름 x 의 암호는 z 입니다.
```

애플리케이션 예외가 발생했을 때 서버가 클라이언트에 반환하는 정보에 주의해야 한다. 세부 정보를 노출하지 말자!

(아이피 주소, 애플리 케이션 내부 구조를 유추할 수 있는 오류 메시지 로그 등등)

클라이언트로 반환되는 응답이 특정 입력이 무엇인지 추측하게 도와줘서는 안 된다.

(무차별 대입 공격에 취약)

```
"상태" : 401,
"메시지" : "사용자 이름이 올바르지 않음"

"상태" : 401,
"메시지" : "암호가 올바르지 않음" 
<- 이런 식으로 특정 입력이 무엇인지 추측하는 정보를 반환하면 안 된다.

"메시지" : "사용자 이름 또는 암호가 올바르지 않음"
<- 두 가지 상황에 대해 동일한 메시지를 보여주자! 
```

* 메서드 접근 제어 부족이란?

호출할 메서드가 접근 제어 되지 않는 것을 말한다. 엔드 포인트로 컨트롤러를 호출해서 리포지토리를 사용 중 

새로운 컨트롤러(리포지토리에 접근하는)를 만들면서 권한 부여 로직을 적용하지 못할 수 있다.

이런 경우 접근 제어를 리포지토리에서 수행하게 해서 일괄적으로 처리하게 하면 효율적으로 관리할 수 있다.

* 알려진 취약성이 있는 종속성 이용

오픈 소스를 사용하면서 취약성 있는 종속성을 발견한다면 악용되었는지 조사하고 필요한 조치를 취하자.

## 1.5 다양한 아키텍처에 적용된 보안

아키텍처는 스프링 시큐리티 구성을 선택할 때 큰 영향을 미친다. 아키텍처에 맞는 보안을 구축해야 한다.

* MSA 가 아닌 일체형 웹 애플리케이션 설계

HTTP 요청을 수신하고 HTTP 응답을 클라이언트에 보내는 일반 서블릿 흐름에서 

세션이 있다면 세션 취약성, HTTP 세션에 저장하는 값 정보, CSRF 가능성을 고려해서 만들어야 한다.

* 백엔드/프런트 엔드 분리를 위한 보안 설계

```
수직 확장성 - 요청이 증가하면 더 많은 메모리와 처리 성능이 추가된다. (서버 자체를 늘리는 것)

수평 확장성 - 요청이 증가하면 애플리케이션 인스턴스를 더 시작한다. (필요한 서버를 필요한 만큼 추가)
```

* OAuth 2 흐름 이해

권한 부여 서버와 리소스 서버를 별도로 만들고 사용자가 백엔드 리소스를 호출할 때 권한 부여 서버에서 

토큰(사무실 사원 카드) 를 갱신해서 인증 및 권한 부여를 수행한다.

* API 키, 암호화 서명, IP 검증을 이용해 요청 보안

두 백엔드 구성 요소 간에 요청이 있을때 이 접근법이 필요할 수 잇다.