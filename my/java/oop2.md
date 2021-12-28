# 2장. 이상한 나라의 객체

`객체지향 패러다임은 지식을 추상화하고 추상화한 지식을 객체 안에 캡슐화함으로써 실세계 문제에 내제된 복잡성을 관리하려고 한다. 객체를 발견하고 창조하는 것은 지식과 행동을 구조화하는 문제다.`

## 객체, 그리고 이상한 나라

앨리스가 하는 행동에 따라 앨리스의 상태가 변한다.

앨리스의 상태를 결정하는 것은 행동이지만 `행동의 결과를 결정하는 것은 상태`다.

### 앨리스의 특징

- 앨리스는 상태를 가지며 상태는 변경 가능하다.
- 앨리스의 상태를 변경시키는 것은 앨리스의 행동이다.
    - 행동의 결과는 상태에 의존적이며 상태를 이용해 서술할 수 있다.
    - 행동의 순서가 결과에 영향을 미친다.
- 앨리스는 어떤 상태에 있더라도 유일하게 식별 가능하다.

## 객체, 그리고 소프트웨어 나라

### 객체

- 상태(state), 행동(behavior), 식별자(identity)를 지닌 실체
- 식별 가능한 개체 또는 사물
- 구별 가능한 식별자, 특징적인 행동, 변경 가능한 상태를 가진다.
- 객체는 저장된 상태와 실행 가능한 코드를 통해 구현된다.

### 상태

- 특정 시점에 객체가 가지고 있는 정보의 집합으로 객체의 구조적 특징을 표현한다.
- 객체의 상태는 객체에 존재하는 정적인 프로퍼티와 동적인 프로퍼티 값으로 구성한다.
- 객체의 프로퍼티는 단순한 값과 다른 객체를 참조하는 링크로 구분할 수 있다.

### 행동

- 외부의 요청 또는 수신된 메세지에 응답하기 위해 동작하고 반응하는 활동
- 행동의 결과로 객체는 자신의 상태를 변경하거나 다른 객체에게 메시지를 전달할 수 있다.
- 객체는 행동을 통해 다른 객체와의 협력에 참여하므로 행동은 외부에 가시적이어야 한다.

### 상태 캡슐화

- 메시지 송신자는 메시지 수신자의 상태 변경에 대해 전혀 알지 못한다 → 캡슐화
- 객체는 상태를 캡슐에 감춰둔 채 외부로 노출하지 않는다.
- 객체의 행동을 유발하는 것은 외부로부터 저낟ㄹ된 메세지지만 객체의 상태를 변경할지 여부는 객체 스스로 결정한다.

### 식별자

- 어떤 객체를 다른 객체와 구분하는데 사용하는 객체의 프로퍼티
- 값은 식별자를 가지지 않기 때문에 상태를 이용한 동등성 검사를 통해 두 인스턴스를 비교해야 한다.
- 객체는 상태가 변경될 수 있기 때문에 식별자를 이용한 동일성 검사를 통해 두 인스턴스를 비교할 수 있다.

### 요약

- 객체는 상태를 가지며 상태는 변경 가능하다.
- 객체의 상태를 변경시키는 것은 객체의 행동이다.
    - 행동의 결과는 상태에 의존적이며 상태를 이용해 서술할 수 있다.
    - 행동의 순서가 실행 결과에 영향을 미친다.
- 객체는 어떤 상태에 있더라도 유일하게 식별 가능하다.

## 행동이 상태를 결정한다.

### 상태를 먼저 결정하고 행동을 나중에 결정할 경우의 부작용

1. 상태를 먼저 결정할 경우 캡슐화가 저해된다.
    1. 공용 인터페이스에 그대로 노출되어 버릴 확률이 높아진다.
2. 객체를 협력자가 아닌 고립된 섬으로 만든다.
    1. 상태를 먼저 고려하는 방식은 협력에 적합하지 못한 객체를 창조하게 된다.
3. 객체의 재사용성이 저하된다.
    1. 다양한 협력에 참여하기 어렵다.
    

### 상태가 아니라 행동에 초점을 맞추자.

`책임주도 설계 (Responsibility Driven Design)`

`행동이 상태를 결정한다.`

## 은유와 객체

### 의인화

현실과 소프트웨어 객체 사이의 차이점

- 현실 속에서는 수동적인 존재가 소프트웨어 객체로 구현될 때는 능동적으로 변한다.