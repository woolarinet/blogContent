---
title: '엔터티, 속성, 관계'
category: 'Database'
desc: '엔터티, 속성, 관계에 대해 좀 더 깊은 공부'
date: '2021-08-16'
thumb: 'database'
---

# 엔터티
- 사람, 장소, 물건, 사건, 개념 등의 명사에 해당한다.
- 업무상 관리가 필요한 관심사에 해당한다.
- 저장이 되기 위한 어떤 것(Things)이다.
- 집합에 속하는 개체들의 특성을 나타내는 속성(Attribute)을 갖는다.
- ex) 수학, 영어, 국어라는 인스턴스들은 과목이라는 하나의 엔터티에 속한다.

즉, 엔터티란 **업무에 필요하고 유용한 정보를 저장하고 관리하기 위한 집합적인 것**으로 설명할 수 있다고 한다. 

## 특징
- 반드시 업무에 필요하고 관리하고자 하는 정보여야 한다.
  - ex) 병원 - 환자 (O) / 환자 - 일반회사 (X)
- 유일 식별자에 의해 식별이 가능해야 한다.
  - 유일 식별자는 그 엔터티의 고유한 이름이다. 두 개 이상의 엔터티를 대변하면 그 식별자는 잘못 설계된 것
  - ex) 사원번호 - (O) / 사원이름 (X)
- 두 개 이상의 인스턴스의 집합이어야 한다.
  - 엔터티의 특징 중 하나는 집합개념이기 때문에 인스턴스가 한 개 뿐이라면, 엔터티 성립이 되지 않는다.
- 업무 프로세스에 의해 이용되어야 한다.
  - 업무프로세스에 의해 CRUD가 발생하지 않는 고립된 엔터티는 제거하거나 누락된 프로세스가 있는지 살펴보고 추가하여야 한다.
- 반드시 속성이 있어야 한다.
  - 속성을 포함하지 않고 이름만 가지고 있는 경우 혹은 주식별자만 존재하고 일반속성은 전혀 없는 경우는 적절한 엔터티라고 할 수 없다. (*예외적으로 관계엔터티는 주식별자 속성만 갖고 있어도 엔터티로 인정*)
- 다른 엔터티와 최소 한 개 이상의 관계가 있어야 한다.
  - 기본적으로 엔터티가 도출되었다는 것은 해당 업무내에서 업무적인 연관성을 가지고 다른 엔터티와의 연관의 의미를 가지고 있음을 나타낸다.
  - 예외적인 경우도 존재한다.
    - 통계를 위한 엔터티
    - 코드를 위한 엔터티
    - 시스템 처리를 위한 엔터티

## 분류
### 유무에 따른 분류
- 유형엔터티
  - 물리적인 형태가 있으며 안정적이고 지속적으로 활용되는 엔터티
  - ex) 사원, 물품, 강사 등
- 개념엔터티
  - 관리해야 할 개념적 정보로 구분이 되는 엔터티
  - ex) 보험상품
- 사건 엔터티
  - 업무를 수행함에 따라 발생되는 엔터티
  - ex) 주문, 청구 등

### 발생시점에 따른 분류
- 기본엔터티
  - 업무에 원래 존재하는 정보로서 독립적 생성이 가능하고 타 엔터티의 부모역할
  - ex) 사원, 부서, 고객 등
- 중심엔터티
  - 기본엔터티로부터 발생되고 업무에 있어 중심적인 역할
  - ex) 계약, 청구, 주문 등
- 행위 엔터티
  - 두 개 이상의 부모엔터티로부터 발생되고 자주 내용이 바뀌거나 데이터량이 증가
  - 주문목록, 사원변경이력 등

## 명명
- 업무에서 사용하는 용어를 사용한다.
- 가능하면 약어를 사용하지 않는다.
- 단수명사를 사용한다.
- 모든 엔터티에서 유일하게 이름이 부여되어야 한다.
- 생성의미대로 이름을 부여한다.

위의 네번째까지는 대체적으로 잘 지켜지지만 다섯 번째 원칙이 잘 지켜지지 않는 경우가 빈번하게 발생한다고 한다.(사실 나도...) 특히 행위엔터티의 경우에 꽤 많이 발생한다고 하는데, 예를 들어 고객이 어떤 제품을 주문했을 때, 고객제품이라고 명명한다면.. 고객이 주문한 제품인지 고객의 제품인지 의미가 애매모호해질 수 있다. 이 경우에 프로젝트에서 팀원 간 의사소통의 오류로 문제를 야기할 수 있게 된다.

# 속성
- 사전적인 정의로써는 **사물의 성질, 특징, 본질적인 성질** 이다.
- 데이터 모델링 관점에서는 **더 이상 분리되지 않는 최소의 데이터 단위**로 정의할 수 있다.
- 각 인스턴스 들은 속성의 집합으로 설명될 수 있고 하나의 속성은 하나의 인스턴스에만 존재할 수 있다.
- 속성은 관계로 기술될 수 없고 자신이 속성을 가질 수도 없다.
- ex) 사원이라는 하나의 인스턴스는 각 속성에 대해 한 개의 속성값만을 가질 수 있다. 사원의 이름은 홍길동이고 주소는 강남구이며 전화번호는 123-4567, 직책은 대리이다.
  - 여기서 이름, 주소, 전화번호, 직책은 속성이고 그에 대한 값들은 속성 값이다.
  - 따라서 속성값은 각 엔터티가 가지는 속성들의 구체적인 내용이라 할 수 있다.

**한 개의 엔터티는 두 개 이상의 인스턴스의 집합, 한 개의 엔터티는 두 개 이상의 속성을 포함, 한 개의 속성은 한 개의 속성값을 갖는다** 라고 말할 수 있다.

## 특징
- 반드시 업무에서 필요하고 관리하고자 하는 정보이어야 한다.
- 정규화 이론에 따라 정해진 주식별자에 함수적 종속성을 가져야 한다.
- 하나의 속성에는 한개의 값만을 가진다. 한 속성에 여러 값이 있는 경우는 별도의 엔터티를 이용해 분리한다.

## 분류
### 특성에 따른 분류
- 기본속성
  - 업무로부터 추출한 모든 속성, 엔터티에 가장 일반적이고 많은 속성을 차지한다.
  - 식별을 위한 번호, 다른 속성을 계산하거나 영향ㅇ르 받아 생성된 속성을 제외한 모든 속성
  - 코드로 정의한 속성은 속성값이 원래 속성을 나타내지 못하므로 기본속성이 되지 않는다.
- 설계속성
  - 업무상 필요한 데이터 이외에 데이터 모델링을 위해, 업무를 규칙화하기 위해 속성을 새로 만들거나 변형하여 정의하는 속성
  - 대개 코드로 정의한 속성 (코드성 속성)이 이에 속한다.
  - 일련번호와 같은 속성은 단일 식별자를 부여하기 위해 모델 상에서 새로 정의하는 설계속성이다.
- 파생속성
  - 다른 속성에 영향을 받아 발생하는 속성. 보통 계산된 값들이 이에 속한다.
  - 다른 속성에 영향을 받기 때문에 데이터 정합성을 유지하기 위해 가급적 적게 정의하는 것이 좋다.
  - 파생속성은 그 속성이 가지고 있는 계산방법에 대해 반드시 어떤 엔터티에 어떤 속성에 의해 영향을 받는지 정의가 되어야 한다.
### 엔터티 구성방식에 따른 분류
- PK속성
  - 엔터티를 식별할 수 있는 속성
- FK 속성
  - 다른 엔터티와의 관계에서 포함된 속성
- 일반속성
  - 엔터티에 포함되어 있고, PK, FK에 포함되지 않은 속성

  &nbsp;
#### 이 외에도 세부 의미를 쪼갤 수 있는지에 따라 단순형, 복합형으로 분류할 수 있다.
    - 주소 - 시, 구 , 동 번지 : 복합형
    - 나이, 성별 - 단순형

#### 또한, 속성 하나의 값에 동일한 성질의 여러 값이 나타나는 경우가 있는데, 이 경우를 다중값속성이라하고 한개의 값만 가지는 경우를 단일값이라 한다.
    - 전화번호 - 집, 휴대폰, 회사 : 다중값
#### 이 경우엔 1차 정규화를 하거나 별도의 엔터티를 만들어 관계로 연결해야한다.

## 도메인
- 각 속성이 가질 수 있는 값의 범위를 도메인이라 한다 :)

## 명명
- 업무에서 사용하는 이름을 부여한다.
    - 아무리 일반적인 용어여도 업무에 사용되지 않으면 속성의 명칭으로 사용하지 않는 것이 좋다.
- 서술식 속성명은 사용하지 않는다.
  - 명사형을 이용하며 수식어가 많이 붙지 않도록 작성한다. 소유격도 사용하지 않는다.
- 약어사용은 가급적 제한한다.
  - 지나치게 약어를 많이 사용하면 팀원 간의 의사소통도 제약을 받으며 시스템 운영 시에도 불편을 초래한다.
- 전체 데이터 모델에서 유일성을 확보하는 것이 좋다.
  - 모든 속성의 이름은 유일하게 작성하는 것이 좋다. 대량의 속성을 정의하는 경우에는 물론 어려울 수 있지만 최대한 유일하게 작성하는 것이 데이터 정합성을 유지하는데 큰 도움이 된다.
  - 또한, 반정규화를 적용할 때 속성명의 충돌을 해결하여 안정적으로 적용할 수 있게 된다.

# 관계
- 상호 연관성이 있는 상태
- 인스턴스 사이의 논리적인 연관성으로서 존재의 형태나 행위로서 서로에게 연관성이 부여된 상태
- 엔터티간 연관성을 표현하기 때문에 엔터티의 정의나 속성 및 관계의 정의에 따라서 영향을 받는다.

## 분류
- 존재형태에 의해 관계가 형성되거나, "주문한다"와 같은 행위에 의해 형성되는 관계가 있다.
- UML (Unified Modeling Labguage)에는 클래스다이어그램의 관계 중 연관관계와 의존관계가 있다.
  - 연관관계는 항상 이용하는 관계로 앞에서 언급한 존재적 관계에 해당하고, 의존관계는 행위적 관계에 해당한다.
  - ERD에서 존재, 행위에 의한 관계를 구분하지 않았다면 클래스다이어그램에서는 이것을 구분하여 표현한다.
  - 연관관계는 실선, 의존관계는 점선으로 표현한다.
## 표기법
### 관계명
- 관계의 이름을 나타낸다.
- 엔터티가 관계에 참여하는 형태를 지칭하며 두개의 관계명을 가지고 있으며 이러한 각 관계명에 의해 두가지 관점으로 표현될 수 있다.
  - ex) 포함한다 <-> 소속된다
- 엔터티에서 관계가 시작되는 편을 관계시작점이라 부르고 받는 편을 관계끝점이라고 부른다.
  - 두 점 모두 관계이름을 가져야하며 참여자의 관점에 따라 관계이름이 능동적이거나 수동적으로 명명된다.
- 다음과 같은 명명규칙을 따른다.
  - 애매한 동사를 피한다. (ex) 관련이 있다, -이다, -한다 등)
  - 현재형을 표현한다. (ex) -할 것이다 (X) -한다 (O))
### 관계차수
- 두 엔터티 간 관계에서 참여자의 수를 표현하는 것
- 1:1, 1:M, M:N이 있으며 한 개의 관계가 존재하느냐 아니면 두 개 이상의 관계가 존재하는 지를 파악하는 것이 중요하다.
- Crow's Foot 모델에서는 선을 이용하여 표현한다. 한 개가 참여하는 경우는 실선을 그대로 유지하고 다수가 참여하는 경우는 까마기발과 같은 모양으로 그려준다.
  - 1:1

    ![erd.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Database/entity/1.png)
      - 각 엔터티는 관계를 맺는 다른 엔터티에 대해 하나의 관계만을 가지고 있다.
  - 1:M

    ![erd.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Database/entity/2.png)
      - 각 엔터티는 관계를 맺는 다른 엩터티에 대해 하나 이상의 관계를 가지고 있다. 반대 방향은 단지 하나만의 관계를 갖는다.
  - M:N

    ![erd.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Database/entity/3.png)
      - 관계엔터티의 엔터티에 대해 하나이상의 관계를 갖고 있고 반대도 동일하다.
      - M:N 관계로 표현된 데이터 모델은 두개의 주식별자를 상속받은 관계엔터티를 이용하여 3개의 엔터티로 구분하여 표현한다.
### 관계선택사양
- 필수참여관계와 선택참여관계가 존재한다.
  - 지하철출발과 지하철문닫힘: 필수참여
  - 난동객에게 알리는 지하철안내: 선택참여
- 설계단계에서 필수참여와 선택참여는 개발시점에 업무 로직과 직접적으로 관련된 부분이므로 반드시 고려되어야 한다.
  - 선택참여관계는 ERD에서 관계를 나타내는 선에서 선택참여하는 엔터티 쪽을 원으로 표시한다. 필수참여는 아무런 표시를 하지 않는다.
  - 만약 관계가 표시된 양쪽 엔터티에 모두 선택참여관계가 표시된다면 (0:0), 이 관계는 잘못된 확률이 높다.

    ![erd.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Database/entity/4.png)
- 관계선택사양은 어떻게 설정했는지에 따라 참조무결성 제약조건의 규칙이 바뀌게 되므로 주의 깊게 모델링을 해야한다!
## 관계의 정의 및 읽는 방법
### 관계 체크사항
- 두 개의 엔터티 사이에 관심있는 연관규칙이 존재하는지
- 두 개의 엔터티 사이에 정보의 조합이 발생되는지
- 업무기술서 등에 관계연결에 대한 규칙이 서술되어 있는지
- 업무기술서 등에 관계연결을 가능하게 하는 동사가 있는지
### 관계 읽기
데이터 모델을 읽을 때에는 먼저 관계에 참여하는 기준 엔터티를 하나 또는 각각으로 읽고 대상 엔터티의 개수를 읽고 관계선택사양과 관계명을 읽는다.

  ![erd.png](https://raw.githubusercontent.com/woolarinet/blog_content/main/images/Database/entity/5.png)

- 위의 사항에서 뒷부분만 의문문으로 만들면 바로 관계를 도출하기 위한 질문 문장으로 만들 수 있다.
- 고객과 개발자사이 혹은 개발자사이에서 대화를 하며 관계를 완성해 나갈 수 있다.
- 이러한 방법은 엔터티간 관계설정뿐 아니라 업무의 흐름도 되는 효과적인 방법이 될 수 있다.

## 마치며
#### 직접 쇼핑몰의 ERD를 그려본게 이해하는데에 정말 큰 도움이 된다. 이제 식별자부터 시작해서 데이터 모델링의 더 깊은 부분을 파헤쳐봐야겠다 ㅋ

## Reference
- SQL 개발자 기본서