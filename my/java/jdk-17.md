# JDK 17

[https://www.infoworld.com/article/3606833/jdk-17-the-new-features-in-java-17.html](https://www.infoworld.com/article/3606833/jdk-17-the-new-features-in-java-17.html)

[https://mail.openjdk.java.net/pipermail/jdk-dev/2021-September/006037.html](https://mail.openjdk.java.net/pipermail/jdk-dev/2021-September/006037.html)

### 1. Context-specific deserialization filters

`컨텍스트 별 역직렬화 필터`

데이터 스트림의 내용이 생성되는 개체, 해당 필드의 값 및 개체 간의 참조를 결정하기 때문에 신뢰할 수 없는 데이터를 역직렬화하는 것은 본질적으로 위험한 활동입니다. 라이브러리 개체 및 Java 런타임. 직렬화 공격을 비활성화하는 핵심은 임의 클래스의 인스턴스가 역직렬화되는 것을 방지하여 해당 메서드의 직접 또는 간접 실행을 방지하는 것입니다. 역직렬화 필터가 도입되었습니다.

Java 9 는 애플리케이션 및 라이브러리 코드가 들어오는 데이터 스트림을 역직렬화하기 전에 유효성을 검사할 수 있도록 합니다.`java.io.ObjectInputFilter는 역직렬화 스트림을 생성할 때 유효성 검사` 논리를 제공. 그러나 명시적으로 유효성 검사를 요청하기 위해 스트림 작성자에 의존하는 것은 제한이 있다.

더 나은 접근 방식은 모든 스트림 작성자가 참여할 필요가 없도록 스트림별 필터를 구성하는 것.

[https://openjdk.java.net/jeps/415](https://openjdk.java.net/jeps/415)

### 2. restoration of always-strict floating point semantics

`엄격한 부동소수점 의미 체계의 복원`

라이브러리의 개발 완화 포함 `java.lang.Math및java.lang.StrictMath`

[https://openjdk.java.net/jeps/306](https://openjdk.java.net/jeps/306)

### 3. Deprecation of the Security Manager

`Security Manager 사용 중단`

[https://openjdk.java.net/jeps/411](https://openjdk.java.net/jeps/411)

### 4. pattern matching for switch

`스위치를 위한 패턴 매칭`

switch표현식과 명령문이 각각 특정 작업으로 여러 패턴에 대해 테스트 할 수 있도록 합니다. JDK \(16\) 의 instanceof 연산자는 유형 패턴을 취하고 패턴 일치를 수행하도록 확장

[https://openjdk.java.net/jeps/406](https://openjdk.java.net/jeps/406)

### 5. Strong encapsulation for JDK internals

`JDK 내부의 sun.misc.Unsafe에 대한 강한 캡슐화`

단일 명령줄 옵션을 통해 내부 요소의 강력한 캡슐화를 더 이상 완화할 수 없게 만듭니다.

[https://openjdk.java.net/jeps/403](https://openjdk.java.net/jeps/403)

### 6. Removal of the Remote Method Invocation \(RMI\) Activation mechanism

`RMI 의 나머지 부분을 유지하면서 RMI(Remote Method Invocation) 활성화 메커니즘 을 제거합니다`

RMI 활성화 메커니즘은 더 이상 사용되지 않으며 JDK 15 에서 제거하기 위해 더 이상 사용되지 않습니다 .

[https://openjdk.java.net/jeps/407](https://openjdk.java.net/jeps/407)

### 7. foreign function and memory API

`foreign 기능과 메모리 API`

Java 프로그램이 Java 런타임 외부의 코드 및 데이터와 상호 운용할 수 있습니다. 외부 함수, 즉 JVM 외부의 코드를 효율적으로 호출하고 외부 메모리, 즉 JVM이 관리하지 않는 메모리에 안전하게 액세스함으로써 API는 Java 프로그램이 JNI\(Java 기본 인터페이스\). 제안된 API는 외부 메모리 액세스 API와 외부 링커 API라는 두 가지 API의 진화

외부 링커 API는 2020년 후반에 인큐베이팅 API로 Java 16을 대상으로 했습니다. API 계획의 목표에는 사용 용이성, 성능, 일반성 및 안전성이 포함됩니다.

[https://openjdk.java.net/jeps/412](https://openjdk.java.net/jeps/412)

### 8. the platform-agnostic vector API

통합된 플랫폼에 구애받지 않는 벡터 API 는 JDK 17에서 다시 인큐베이션되어 지원되는 CPU 아키텍처에서 최적의 벡터 명령으로 런타임에 안정적으로 컴파일되는 벡터 계산을 표현하는 메커니즘을 제공

[https://openjdk.java.net/jeps/414](https://openjdk.java.net/jeps/414)

### 9. new interface RandomGenerator

점프 가능한 PRNG 및 추가 클래스의 분할 가능한 PRNG 알고리즘\(LXM\)을 포함하여 의사 난수 생성기\(PRNG\)에 대한 새로운 인터페이스 유형 및 구현을 제공하는 향상된 의사 난수 생성기 . 새 인터페이스 RandomGenerator는 모든 기존 및 새 PRNG에 대해 균일한 API를 제공

[https://openjdk.java.net/jeps/356](https://openjdk.java.net/jeps/356)

