# 6장. 객체 지도

`유일하게 변하지 않는 것은 모든 것이 변한다는 사실뿐이다.`

길을 직접 알려주는 방법이 가능하고 해결 방법 지향적인 접근법이라면 지도를 이용하는 방법은 ‘구조적이고 문제 지향적인 접근법’이다. 지도는 길을 찾는데 필요한 구체적인 기능이 아니라 길을 찾을 수 이는 ‘구조’를 제공한다.

객체지향은 자주 변경되는 기능이 아니라 안정적인 구조를 기반으로 시스템을 구조화한다.

## 기능 설계 대 구조 설계

- 성공적인 소프트웨어들이 지닌 공통적인 특징은 훌륭한 기능을 제공하는 동시에 사용자가 원하는 새로운 기능을 빠르고 안정적으로 추가할 수 있다는 것
- 단순하며 유지보수하기 쉬운 설계는 사용자의 변하는 요구사항을 반영할 수 있도록 쉽게 확장 가능한 소프트웨어를 창조할 수 이는 기반이 된다.
- 불확실한 미래의 변경을 예측하고 이를 성급하게 설계에 반영하는 것은 불필요하게 복잡한 설계를 낳을 뿐이다. 우리는 미래를 예측할 수 없다. 단지 대비할 수 있을 뿐이다.
- 변경을 예측하는 것이 아니라 변경을 수용할 수 있는 선택의 여지를 설계에 마련해 놓는 것이다.
- `변경의 여지를 남겨 놓는 가장 좋은 방법은 자주 변경되는 기능이 아닌 안정적인 구조를 중심으로 설게하는 것`
- 안정적인 객체 구조는 변경을 수용할 수 있는 유연한 소프트웨어를 만들 수 있는 기반을 제공한다.

## 두 가지 재료: 기능과 구조

- 기능: 사용자의 목표를 만족시키기 위해 책임을 수행하는 시스템의 행위로 표현
- 구조: 사용자나 이해관계자들이 도메인에 관해 생각하는 개념과 개념들 간의 관계로 표현

일반적으로 기능을 수집하고 표현하기 위한 기법 → 유스케이스 모델링

구조를 수집하고 표현하기 위한 기법 → 도메인 모델링

## 안정적인 재료: 구조

### 도메인 모델

- 대상을 단순화하여 표현한 것
- 소프트웨어 개발과 관련된 이해관계자들이 도메인에 대해 생각하는 관점

### 멘탈 모델

- 사용자 모델, 디자인 모델, 시스템 이미지로 구분
- 사용자 모델
    - 사용자가 제품에 대해 가지고 있는 개념들의 모습
- 디자인 모델
    - 설게자가 마음 속에 갖고 있는 시스템에 대한 개념화
- 시스템 이미지
    - 최종 제품
    

### 도메인의 모습을 담을 수 있는 객체지향

- 표현적 차이
    - 소프트웨어 객체는 현실 객체를 모방한 것이 아니라 은유적 기반으로 재창조한 것
    - 객체와 현실 객체의 의미적 거리를 표현적 차이 혹은 의미적 차이라고 한다.
    - 은유에 투영해야 하는 대상 → 사용자가 도메인에 대해 생각하는 개념들 → 도메인모델


### 불안전한 기능을 담는 안정적인 도메인 모델

- 도메인 모델을 기반으로 소프트웨어의 구조를 설계하면 변경에 유연하게 대응할 수 있는 탄력적인 소프트웨어를 만들 수 있다.


## 불안정한 재료: 기능

### 유스케이스

- 사용자의 목표를 달성하기 위해 사용자와 시스템 간에 이뤄지는 상호작용의 흐름을 텍스트로 정리한 것
- 특성
    - 유스케이스는 사용자와 시스템 간의 상호작용을 보여주는 텍스트다.
    - 유스케이스는 하나의 시나리오가 아니라 여러 시나리오들의 집합이다.
    - 유스케이스는 단순한 피처(feature) 목록과 다르다.
    - 유스케이스는 사용자 인터페이스와 관련된 세부 정보를 포함하지 말아야 한다.
    - 유스케이스는 내부 설계와 관련된 정보를 포함하지 않는다.
    

`유스케이스는 설계 기법도, 객체지향 기법도 아니다.` 

유스케이스는 단지 사용자가 바라보는 시스템의 외부 관점만을 표현한다.

## 재료 합치기: 기능과 구조와 통합

### 도메인 모델, 유스케이스, 그리고 책임 주도 설계

- 도메인 모델에 명시된 정기예금이나 계좌와 같은 개념을 스스로 상태와 행위를 관리하는 자율적인 객체로 간주한다
- 책임 할당의 기본 원칙은 책임을 수행하는 데 필요한 정보를 가진 객체에게 그 책임을 할당하는 것

### 기능 변경을 흡수하는 안정적인 구조


`안정적인 도메인 모델을 기반으로 시스템의 기능을 구현하라.`

도메인 모델과 코드를 밀접하게 연결시키기 위해 노력하라.