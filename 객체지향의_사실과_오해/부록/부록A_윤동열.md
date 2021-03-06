# 부록 A 추상화 기법

## 추상화 기법

* 추상화는 도메인의 복잡성을 단순화하고 직관적인 멘탈모델을 만드는 데 사용할 수 있는 가장 기본적인 인지수단
* 추상화 기법
  1. 분류와 인스턴스화
     * 분류는 객체의 구체적인 세부 사항을 숨기고 인스턴스 간에 공유하는 공통적인 특성을 기반으로 범주를 형성하는 과정이다. 분류의 역은 범주로부터 객체를 생성하는 인스턴스화 과정
  2. 일반화와 특수화
     * 범주의 차이를 숨기고 범주 간에 공유하는 공통적 특성을 강조한다. 일반화의 역을 특수화라고 한다.
  3. 집합과 분해
     * 집합은 부분과 관련된 세부 사항을 숨기고 부분을 사용해서 전체를 형성하는 과정을 가리킨다. 집합의 반대 과정은 전체를 부분으로 분리하는 분해 과정이다.



## 분류와 인스턴스화

### 개념과 범주

* 완전히 동일하지는 않지만 유사한 특성을 바탕으로 개념을 분류

  > 승용차, 버스, 트럭 -> 자동차라는 개념

* 개념(타입)이란 속성과 행위가 유사한 객체에 공통적으로 적용되는 관념이나 아이디어

* 세상에 존재하는 객체에 개념을 적용하는 과정을 분류라고 한다

* 분류는 객체와 타입 간의 관계를 나타낸 것.

* 어떤 객체가 타입의 정의에 부합할 경우 그 객체는 해당 타입으로 분류되며 자동으로 타입의 인스턴스가 된다.



### 타입

* 객체를 타입에 따라 분류하기 위해서는 객체가 타입에 속하는지 여부를 확인할 수 있어야 한다.

* 세 가지 관점에서의 정의가 필요하다.

  1. 심볼: 타입을 가르키는 이름이나 명칭

     > 자동차

  2. 내연: 타입의 완전한 정의, 내연의 의미를 이용해 객체가 타입에 속하는지 여부를 확인할 수 있다.

     > 원동기를 동력원으로 해서 주행하는 기계

  3. 외연: 타입에 속하는 모든 객체들의 집합.

     > 승용차, 택시, 버스 ···



### 외연과 집합

* 타입의 외연은 타입에 속하는 객체들의 집합으로 표현한다.
* 집합 = 외연
* 대부분의 객체지향 프로그래밍 언어들은 단일 분류만을 지원한다.
* 다중 분류와 동적 분류 관점에서 도메인 모뎅의 초안을 만든 후 실제 구현에 적합하도록 단일 분류와 정적 분류 방식으로 객체들의 범주를 재조정하는 편이 분석과 구현 간의 차이를 메울 수 있는 가장 현실적인 방법이다.



### 클래스

* 객체지향 프로그래밍 언어를 이용해 타입을 구현하는 가장 보편적인 방법은 클래스를 이용하는 것이다.
* 타입 != 클래스
* 코드를 재사용하는 용도로 쓰인다.



## 일반화와 특수화

### 범주의 계층

* 린네의 계층 구조(종 속 과 목 강 문 계)는 좀 더 세부적인 범주가 계층의 하위에 위치하고 좀 더 일반적인 범주가 계층의 상위에 위치한다.

* 계층의 상위에 위치한 범주를 계층의 하위에 위치한 범주의 일반화라고 하고, 계층의 하위에 위치한 범주는 계층의 상위에 위치한 범주의 특수화라고 한다.

  > 고양이(하위) - 포유류(상위)



### 서브타입

* 더욱 일반적인 타입: 슈퍼타입

* 더 특수한 타입: 서브 타입

* 슈퍼타입: 서브타입의 일반화 || 서브타입: 슈퍼타입의 특수화

* 어떤 타입이 다른 타입의 서브타입이 되기 위해서는 **100% 규칙**과 **Is-a 규칙**을 준수해야한다

  * **100% 규칙**: 슈퍼타입의 정의가 100% 서브타입에 적용되어야 한다. 서브타입은 속성과 연관관계 면에서 슈퍼타입과 100% 일치해야 한다.

  *  **Is-a 규칙**: 서브타입의 모든 인스턴스는 슈퍼타입 집합에 포함되어야 한다. (일반화 관계)

    > Subtype is a Supertype



### 상속

* 프로그래밍 언어를 이용해 일반화와 특수화 관계를 구현하는 가장 일반적인 방법은 클래스 간의 상속을 사용하는 것이다.
* 서브타이핑
  * 서브클래스가 슈퍼클래스를 대체할 수 있는 경우
  * 설계의 유연성이 목적
  * 인터페이스 상속
  * 대체 가능
* 서브 클래싱
  * 서브클래스가 슈퍼클래스를 대체할 수 없는 경우
  * 코드의 중복 제거와 재사용이 목적
  * 구현 상속
* 여러 클래스로 구성된 상속 계층에서 수신된 메시지를 이해하는 기본적인 방법은 클래스 간 위임을 사용하는 것이다.
  * 어떤 객체의 클래래스가 수신된 메시지를 이해할 수 없으면 메시지를 부모클래스로 위임



## 집합과 분해

### 계층적인 복잡성

* 복잡성은 계층의 형태를 띤다.
* 단순한 형태로 부터 복잡한 형태로 진화하는 데 걸리는 시간은 그 사이에 존재하는 '안정적인 형태'의 수와 분포에 의존한다.
* 안정적인 형태의 부분으로 부터 전체를 구축하는 행위를 **집합**.
* 전체를 부분으로 분할하는 행위를 **분해**라고 한다.
* 집합: 추상화 메커니즘인 동시에 캡슐화 메커니즘



### 합성 관계

* 객체와 객체 사이의 전체-부분 관계를 구현하기 위해 합성 관계를 사용한다.
* 부분을 전체 안에 캡슐화 함으로써 인지 과부하를 방지
* 주문 - 주문항목, 수량 ···
* 상품 - 가격 ···
* 상품 - 주문 : 연관관계
* 합성관계는 생명주기 측면에서 연관 관계보다 더 강하게 객체들을 결합한다.



### 패키지

* 소프트웨어를 전체적인 구조를 표현하기 위해 관련된 클래스 집합을 하나의 논리적인 단위로 묶는 구성 요소를 **패키지** 또는 **모듈** 이라고 한다.
* 전체적인 구조를 이해하기 위해 한 번에 고려해야 하는 요소의 수를 줄일 수 있다.
