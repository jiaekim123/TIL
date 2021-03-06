# 4장. 역할, 책임, 협력

## 객체의 협력

- 중요한 것은 개별 객체가 아니라 객체들 사이에 이뤄지는 협력이다.
- 훌륭한 객체지향 설계란 겉모습은 아름답지만 협력자들을 무시하는 오만한 객체를 창조하는 것이 아니라 조화를 이루며 적극적으로 상호작용하는 협력적인 객체를 창조하는 것

## 협력

### 요청하고 응답하며 협력하는 사람들

- 협력은 한 사람이 다른 사람에게 도움을 요청할 때 시작
- 일을 처리한 후 요청한 사람에게 필요한 지식이나 서비스를 제공하는 것으로 요청에 응답

## 책임

책임은 객체에 의해 정의되는 응집도 있는 행위의 집합

어떤 객체가 어떤 요청에 대해 대답해줄 수 있거나, 적절한 행동을 할 의무가 있는 경우 해당 객체가 책임을 가진다고 말한다.

### 객체의 책임 분류

- ‘객체가 무엇을 알고 있는가(knowing)’
    - 개인적인 정보에 관해 아는 것
    - 관련된 객체에 대해 아는 것
    - 자신이 유도하거나 계산할 수 있는 것에 관해 아는 것
- ‘객체가 무엇을 할 수 있는가(doing)’
    - 객체를 생성하거나 계산하는 등의 스스로 하는 것
    - 다른 객체의 행동을 시작하는 것
    - 다른 객체의 활동을 제어하고 조절하는 것

- 책임은 객체의 외부에 제공해 줄 수 있는 정보(아는 것의 측면)과 외부에 제공해 줄 수 있는 서비스(하는 것의 측면)의 목록이다.
- 책임은 객체의 공용 인터페이스(public interface)를 구성한다.

## 책임과 메시지

객체가 다른 객체에게 주어진 책임을 수행하도록 요청을 보내는 것을 메시지 전송이라고 한다.

- 송신자: 협력을 요청하는 객체
- 수신자: 요청을 처리하는 객체

`책임과 메시지의 수준이 같지 않다.`

객체지향 설계는 협력에 참여하기 위해 어떤 객체가 어떤 책임을 수행하고 어떤 객체로부터 메시지를 수신할 것인지를 결정하는 것으로부터 시작한다.

## 역할

### 1. 책임의 집합이 의미하는 것

역할은 협력 내에서 다른 객체로 대체할 수 있음을 나타내는 일종의 표식이다.

역할은 객체지향 설계의 단순성(simplicity), 유연성(flexivility), 재사용성(reusability)을 뒷받침하는 핵심 개념이다.

### 2. 협력의 추상화

역할의 가장 큰 가치는 하나의 협력 안에 여러 종류의 객체가 참여할 수 있게 함으로써 협력을 추상화할 수 있다는 것

### 3. 대체가능성

역할은 다른 객체에 의해 대체 가능하다.

객체가 역할을 대체하기 위해서는 행동이 호환돼야 한다

객체가 역할에 주어진 책임 이외에 다른 책임을 수행할 수 있다

객체는 역할이 암시하는 책임보다 더 많은 책임을 가질 수 있다. 따라서 객체의 타입과 역할 사이에 일반화/특수화 관계가 성립

`역할의 대체 가능성은 행위 호환성을 의미하고, 행위 호환성은 동일한 책임의 수행을 의미한다.`

## 객체의 모양을 결정하는 협력

- 데이터는 단지 객체가 행위를 수행하는데 필요한 재료
- 객체가 존재하는 이유는 행위를 수행하며 협력에 참여하기 위해서이다.
- 객체지향에서 중요한 것은 정적인 클래스가 아니라 협력에 참여하는 동적인 객체
- 클래스는 단지 시스템에 필요한 객체를 표현하고 생성하기 위해 프로그래밍 언어가 제공하는 구현 메커니즘

## 협력을 따라 흐르는 객체의 책임

올바른 객체를 설계하기 위해서느 먼저 깔끔한 협력을 설계해야 한다.

### 협력을 설계하는 방법

1. 설계에 참여하는 객체들이 주고받을 요청과 응답 흐름을 결정
2. 결정된 요청과 응답의 흐름은 객체가 협력에 참여하기 위해 수행될 책임이 된다.
3. 책임은 객체가 외부에 제공하게될 행동이다.
4. 행동을 결정한 후에 그 행동을 수행하는데 필요한 데이터를 고민한다.
5. 데이터와 행동이 결정되면 클래스의 구현 방법을 결정한다.

### 객체의 행위에 초점 맞추기

- 협력이라는 실행 문맥 안에서 책임을 분배한다.
- 객체가 가져야 하는 상태와 행위에 대해 고민하기 전에 그 객체가 참여할 문맥인 협력을 정의하라
- 객체를 충분히 협력적으로 만든 후에 협력이라는 문맥 안에서 객체를 충분히 자율적으로 만드는 것

## 객체지향 설계 기법

역할, 책임, 협력의 관점에서 어플리케이션을 설계하는 세 가지 기법

### 1. 책임 주도 설계(Responsibility Driven Design)

`협력에 필요한 책임들을 식별하고 적합한 객체에게 책임을 할당하는 방식`

- 작은 규모의 책임으로 분할하여 각 책임은 책임을 수행할 적절한 객체에게 할당
- 책임을 여러 종류의 객체가 수행할 수 있다면 협력자는 객체가 아니라 추상적인 역할로 대체
- 책임 주도 설계는 개별적인 객체의 상태가 아니라 객체의 책임과 상호작용에 집중

`책임주도 설계로 시스템을 설계하는 절차`

- 시스템이 사용자에게 제공해야 하는 기능인 시스템 책임을 파악한다.
- 시스템 책임을 더 작은 책임으로 분할한다.
- 분할된 책임을 수행할 수 있는 적절한 객체 또는 역할을 찾아 책임을 할당한다.
- 객체가 책임을 수행하는 중에 다른 객체의 도움이 필요한 경우 이를 책임질 적절한 객체 또는 역할을 찾는다.
- 해당 객체 또는 역할에게 책임을 할당함으로써 두 객체가 협력하게 한다.

### 2. 디자인 패턴(Design Pattern)

`전문가들이 반복적으로 사용하는 해결 방법을 정의해 놓은 설계 템플릿의 모음`

- 패턴은 전문가들이 특정 문제를 해결하기 위해 이미 식별해 놓은 역할, 책임, 협력의 모음이다.

`디자인 패턴은 책임-주도 설계의 결과를 표현한다.`

- 책임 주도 설계가 객체의 역할, 책임, 협력을 고안하기 위한 방법과 절차를 제시한다면 디자인패턴은 책임 주도 설계의 결과를 표현한다.
- 디자인패턴은 반복적으로 발생하는 문제와 그 문제에 대한 해법의 쌍으로 정의된다.
- 대표 책) GOF의 디자인패턴 - 23개의 디자인 패턴

### 3. 테스트 주도 개발(Test Driven Development)

`테스트 주도 개발은 테스트를 먼저 작성하고 테스트를 통과하는 구체적인 코드를 추가하면서 애플리케이션을 완성해가는 방식`

- 테스트 주도 개발의 기본 흐름은 실패하는 테스트를 먼저 작성하고, 테스트를 통과하는 가장 간단한 코드를 작성한 후 (이 시간 동안 중복이 있어도 무방하다). 리팩터링을 통해 중복을 제거하는 것이다.
- 테스트 주도 개발은 객체가 이미 존재한다고 가정하고 객체에게 어떤 메시지를 전송할 것인지에 관해 먼저 생각하라고 충고한다.