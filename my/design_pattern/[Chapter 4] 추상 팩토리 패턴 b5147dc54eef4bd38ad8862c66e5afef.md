# [Chapter 4] 추상 팩토리 패턴

추상 팩토리 패턴은 팩토리 메서드 패턴을 확장한 패턴이다. 

## 팩토리 메서드

- 팩토리 메서드 패턴은 객체 생성을 담당하는 클래스를 추상화하여 선언과 구현을 분리한 확장 패턴이다.

### 패턴 유사성

- 팩토리 패턴과 팩토리 메서드 패턴의 차이는 추상화
- 팩토리 메서드 패턴과 추상화 팩토리 패턴의 차이는 추상화된 그룹을 형성하고 관리하는 것

### 팩토리 패턴, 팩토리 메서드 패턴의 특징과 차이점

- 객체 생성의 종속성
    - 별도로 분리된 객체에 생성을 요청하면 객체 간의 직접적인 종속 관계를 제거할 수 있음
    - 생성 패턴은 메서드를 호출함으로써 필요한 객체를 생성한다.
- 추상화와 위임 처리
    - 팩토리 패턴과 팩토리 메서드 패턴의 차이점은 추상화
    - 하위 클래스는 추상화된 상위 클래스 상속
    - 추상 메서드는 상위 클래스에서 정의한 인터페이스에 따라 하위 클래스에서 오버라이드
    - 실제 동작하는 메서드 내용으로 구현
- 상속과 다형성
    - 추상 클래스의 상속은 하위 클래스에서 구체적 행동을 규정하는 선언
    - 추상화는 하위 클래스에 객체 지향의 다형성을 부여한다.
    - 다형성을 적용하면 여러 개의 하위 클래스를 만들어야 한다는 단점이 있다.

## 그룹

추상 팩토리는 여러 개의 팩토리 메서드를 그룹으로 묶은 것과 유사함

### 추상 클래스

팩토리 메서드는 팩토리 패턴을 추상화함으로써 클래스의 선언과 구현을 분리한다.

### 다형성을 이용한 군집 형성

- 팩토리 메서드는 추상 클래스와 하위 클래스를 1개로만 구성한다.
- 반면에 추상 팩토리는 다형성을 적극적으로 활용하여 다수의 하위 클래스를 생성하고 관리하는 것이 특징

### 공장

- 추상 팩토리는 다양한 객체 생성 과정에서 그룹화가 필요할 때 유용한 패턴

## 팩토리 그룹

- 팩토리 메서드는 추상 팩토리와 동일하게 추상화 과정을 적용할 수 있지만 단일 그룹으로 제한한다.
- 팩토리 메서드에서는 하나의 하위 클래스만 가질 수 있고, 추상 팩토리에서는 복수의 하위 클래스를 가질 수 있다.

### 가상화 관점의 추상화

- 추상 팩토리, 팩토리 메서드 모두 단일 클래스를 추상화
- 이렇게 추상화를 사용하는 이유는 구체적인 객체 생성 로직을 알지 못하기 때문
- 추상 팩토리는 복수의 팩토리 메서드 패턴을 결합하여 하나의 가상화된 객체 생성군을 형성
- 하위 클래스 생성 시 if문이나 switch문 없이 직접 2개의 객체를 생성하는 메서드로 구현
- 필요하면 메서드를 통해 생성 객체를 구분

## 공장 추가

- 추상 팩토리는 그룹을 통해 복수의 공장을 생성할 수 있음
- 추상 팩토리 사용 목적은 복수의 객체 생성을 담당하는 군집을 관리하는 것
- 유사한 객체의 생성을 그룹화하여 처리한다. 추상 팩토리는 다수의 독립적 그룹을 형성한다.

### 그룹 추가

- 팩토리 메서드는 단일 그룹을 관리하지만 추상 팩토리는 복수의 그룹을 관리할 수 있다.
- 추상 팩토리에서 새로운 그룹을 만드는 것은 객체의 생산 공장을 추가하는 것과 같다.

### 목적성

- 추상 팩토리는 특정 클래스에 의존하지 않는다. 해결해야 하는 문제에 따라 객체 생성 그룹을 형성한다.
- 목적(문제)에 따른 그룹을 변경하여 적용한다. 각각의 그룹들은 서로 호환성을 가지며 동일한 인터페이스에 의해 호출된다.

## 부품 추가

> 단순한 인터페이스만 상속할 때는 추상 클래스보다 인터페이스를 통해 구현 적용하는 것이 더 나을 수도 있다.

## 패턴 결합

### 새로운 부품

- 새로운 부품인 Engine을 추가해야 한다.
- 추상 클래스 그룹으로 분리된 모든 클래스에 Engine 부품 관련 코드를 삽입해야 한다.
- 추상 클래스의 경우 그룹을 나누는 것은 쉽지만 서브 생성 객체를 추가하는 것은 어렵다.

### 단점

- 추상 팩토리의 경우 새로운 종류의 군을 추가하는 것이 쉽지 않습니다.
- 모든 서브 클래스들이 동시에 변경돼야  하는 추상 팩토리의 특징
- 확장성 부분에서 좋지 않습니다.
- 추상 팩토리는 팩토리 메서드와 매우 비슷하지만 관리할 그룹이 많다는 차이가 있다.
- 계층의 크기가 커질수록 복잡한 문제가 발생한다.

## 관련 패턴

### 빌더 패턴

- 추상 팩토리는 인터페이스를 이용하여 객체를 조립한다.
- 빌더는 더 크고 복잡한 객체를 조립하거나 생성할 때 사용한다.

### 팩토리 메서드 패턴

- 생성 패턴을 그룹화하여 단위별로 객체 생성하여 객체를 조립하는 과정에서 팩토리 메서드 패턴이 응용된다.

### 복합체 패턴

- 추상 팩토리는 추상화를 통해 객체 생성을 조립한다. 조합된 객체는 복합 객체의 특징을 가진다.

### 싱글턴 패턴

- 유일한 객체 생성이 필요하다면 일부 객체는 싱글턴 방식으로 변경할 수 있따.

## 정리

- 추상 팩토리 패턴은 팩토리 메서드 패턴을 포함하며 팩토리 부분을 추상화해 그룹으로 확장합니다.
- 생성된 그룹을 통해 전체를 쉽게 변경할 수도 있습니다.
- 추상 팩토리는 객체 생성 과정이 프로세스 공정과 같다. 같은 방식으로 생성할 때 적용하면 좋은 패턴이다.

## 코드

```java
package design.pattern.abst;

public abstract class CarFactory {
    public abstract TireProduct createTire();
    public abstract DoorProduct createDoor();
}

```

```java
package design.pattern.abst;

public abstract class DoorProduct {
    public abstract String makeAssemble();
}
```

```java
package design.pattern.abst;

public class KoreaCarFactory extends CarFactory{
    public KoreaCarFactory() {

    }

    @Override
    public TireProduct createTire() {
        return new KoreaTireProduct();
    }

    @Override
    public DoorProduct createDoor() {
        return new KoreaDoorProduct();
    }
}
```

```java
package design.pattern.abst;

public class KoreaDoorProduct extends DoorProduct {
    @Override
    public String makeAssemble() {
        return "문이 열립니다.";
    }
}
```

```java
package design.pattern.abst;

public class KoreaTireProduct extends TireProduct {
    public KoreaTireProduct() {
    }

    @Override
    public String makeAssemble() {
        return "국산 타이어";
    }
}
```

```java
package design.pattern.abst;

public class StateCarFactory extends CarFactory {
    @Override
    public TireProduct createTire() {
        return new StateTireProduct();
    }

    @Override
    public DoorProduct createDoor() {
        return new StateDoorProduct();
    }
}
```

```java
package design.pattern.abst;

public class StateDoorProduct extends DoorProduct {

    @Override
    public String makeAssemble() {
        return "Door is open";
    }
}
```

```java
package design.pattern.abst;

public class StateTireProduct extends TireProduct {
    public StateTireProduct() {
    }

    @Override
    public String makeAssemble() {
        return "미국산 타이어";
    }
}
```

```java
package design.pattern.abst;

public abstract class TireProduct {
    public abstract String makeAssemble();
}
```

```java
package design.pattern.abst;

import org.junit.jupiter.api.Assertions;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.*;

class CarFactoryTest {

    @Test
    @DisplayName("한국산 차 만들기")
    void makeKoreaCar(){
        // given
        CarFactory carFactory = new KoreaCarFactory();
        TireProduct tireProduct = carFactory.createTire();
        DoorProduct doorProduct = carFactory.createDoor();

        // when
        String tire = tireProduct.makeAssemble();
        String door = doorProduct.makeAssemble();

        // then
        assertEquals("국산 타이어", tire);
        assertEquals("문이 열립니다.", door);
    }

    @Test
    @DisplayName("미국산 차 만들기")
    void makeStateCar(){
        // given
        CarFactory carFactory = new StateCarFactory();
        TireProduct tireProduct = carFactory.createTire();
        DoorProduct doorProduct = carFactory.createDoor();

        // when
        String tire = tireProduct.makeAssemble();
        String door = doorProduct.makeAssemble();

        // then
        assertEquals("미국산 타이어", tire);
        assertEquals("Door is open", door);
    }
}
```