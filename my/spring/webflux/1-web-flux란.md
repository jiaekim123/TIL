## WebFlux란?

Spring5에 새로 추가된 모듈로 클라이언트와 서버에서 반응형 프로그래밍 개발을 도와주는 모듈이다. 

## 반응형 프로그래밍이란?

비동기(asynchronous) 및 이벤트 중심(event-driven) 방식의 논블록킹(non-blocking) 어플리케이션

수평이 아닌 수직으로(즉, 클러스터링을 통해서가 아니라 JVM 내에서) 확장하기 때문에 적은 수의 쓰레드가 필요하다는 특징이 있다.

반응형 어플리케이션(reactive application)의 핵심은 producer가 consumer를 넘지 않는 메커니즘으로 동작한다.

blocking code를 작성하는 것은 마치 Java8의 CompletableFuture를 사용하여 람다 표현식을 통해 후속 조치 (follow-up action)을 하는 것과 비슷하다.

## Spring MVC vs WebFlux

왜 최근에 많은 회사에서 WebFlux를 사용하고 있을까? Spring MVC에 비해 WebFlux는 어떤 점이 좋을까?

우선, WebFlux는 앞에서 말했던 것과 같이 Event-Driven 방식이고 비동기 논블록킹 방식이다. 

Spring MVC에서는 논블록킹을 위해 주로 Completable Future를 사용해서 비동기 처리를 하는데 이 때 처리하는 방식은 사용자의 요청이 들어올 때마다 Thread를 만들어서 처리하게 된다. 하지만 트래픽이 높아져서 사용자 수가 늘어나면 많은 양의 Thread를 만들어야 하고, 이렇게 Thread를 매번 생성한다는 것은 비효율적이기 때문에 미리 사용할만큼의 Thread Pool을 만들어두고 사용한다.

요청이 들어오면 그 요청을 Queue에 쌓고, 이것을 Thread pool의 Thread들이 요청을 가져가서 처리하는데 만약 트래픽이 급속도로 증가해서 Thread Pool 최대 크기를 초과하게 되면 Queue에 요청들은 계속 쌓이는데 요청은 처리하지 못하고 있는 상태인 `Thread Pool Hell` 현상이 발생한다.

이런 현상을 해결하기 위해 Event Driven 방식이 고려되고 있는데, Event Driven 방식의 WebFlux의 구조는 다음과 같다.

![image](https://user-images.githubusercontent.com/37948906/144868145-64db08a5-ab04-41e1-8745-a22d39d8f90e.png)

위와 같이, 이벤트 루프가 계속 돌며 요청이 발생하면 그에 맞는 핸들러에게 처리를 맡기고 call back 메서드를 통해서 응답을 반환받아 사용자에게 반환한다.

따라서, Spring MVC는 트래픽이 몰리면 Thread Pool 이 꽉차서 더 이상 사용자의 요청을 처리할 수 없는 상황이 발생할 수도 있지만, Event Loop를 계속 해서 도는 WebFlux 구조에서는 Thread Pool이 꽉차는 현상은 없다. (그저 이벤트 루프를 계속 돌뿐..)

## 참고 문서

- [https://docs.spring.io/spring-framework/docs/5.0.0.M5/spring-framework-reference/html/web-reactive.html](https://docs.spring.io/spring-framework/docs/5.0.0.M5/spring-framework-reference/html/web-reactive.html)
- [https://www.baeldung.com/spring-webflux](https://www.baeldung.com/spring-webflux)
- [https://velog.io/@dyllis/Spring-MVC-vs-WebFlux](https://velog.io/@dyllis/Spring-MVC-vs-WebFlux)