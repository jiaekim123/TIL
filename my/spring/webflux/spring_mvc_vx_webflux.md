# Spring MVC와 WebFlux의 차이점

## 개요

왜 최근에 많은 회사에서 WebFlux를 사용하고 있을까? Spring MVC에 비해 WebFlux는 어떤 점이 좋을까?

## Spring MVC

우선, WebFlux는 Event-Driven 방식이고 비동기 논블록킹 방식이다. 

Spring MVC에서는 논블록킹을 위해 주로 Completable Future를 사용해서 비동기 처리를 하는데 이 때 처리하는 방식은 사용자의 요청이 들어올 때마다 Thread를 만들어서 처리하게 된다. 하지만 트래픽이 높아져서 사용자 수가 늘어나면 많은 양의 Thread를 만들어야 하고, 이렇게 Thread를 매번 생성한다는 것은 비효율적이기 때문에 미리 사용할만큼의 Thread Pool을 만들어두고 사용한다.

요청이 들어오면 그 요청을 Queue에 쌓고, 이것을 Thread pool의 Thread들이 요청을 가져가서 처리하는데 만약 트래픽이 급속도로 증가해서 Thread Pool 최대 크기를 초과하게 되면 Queue에 요청들은 계속 쌓이는데 요청은 처리하지 못하고 있는 상태인 `Thread Pool Hell` 현상이 발생한다.

## Thread Pool Hell

그럼 이 방법을 해결하기 위해 우리는 어떻게 해야 할까? 우선 Spring MVC에서는 아래와 같은 방법을 먼저 고려한다.

- thread pool 크기를 조정 (서버를 늘린다)
    - thread pool이 꽉찼다면 thread pool을 늘리면 되잖아? 라는 생각을 할 수 있지만, thread pool을 늘리는데에는 한계가 있다. 적정 스레드 풀 개수라는 것이 있는데 이는 `CPU 수 * (CPU 목표 사용량) * (1+대기 시간/서비스 시간)` 으로 결정된다.
    - 자세한 내용은
    - 즉 core 하나당 처리할 수 있는 thread는 정해져 있고, 코어가 적은데 thread pool만 무작정 늘리면 context switching으로 성능저하가 일어날 수 있기 때문에 서버를 늘리지 않는 이상 thread pool은 한계가 있다. ~~(물론 돈이 많아서 무한히 서버를 늘린다면 말리지 않는다)~~
- 요청에 대한 응답 속도를 빠르게 한다.
    - 요청이 빨리 처리 되면 트래픽이 몰려서 queue에 메세지가 쌓여도 금방 처리할 수 있기 때문에 우선적으로 요청에 대한 응답속도를 개선하는 것을 먼저 생각한다. 그렇다면 응답속도는 어떻게 개선할 수 있을까?
        - DB I/O를 줄이기 위해 서버 캐시를 도입하는 것?
        - JPA 쿼리 최적화?
- 원랜 빠르던 친구가 갑자기 느려졌나? 원인 분석
    - DB에 급격한 요청이 늘어나 성능 저하가 있었나?
    - Network에 문제가 생겼나?

thread pool이 꽉차서 감당할 수 있는 요청수를 넘으면 급격히 성능저하가 일어나 지연이 발생한다. ~~(이걸 우리팀에선 줄섰다고 표현한다.)~~

WebFlux는 요런 문제를 해결하기 위해 Event-Driven 방식을 도입했다.

## WebFlux

요런 문제를 해결하기 위해 WebFlux는 반응형 프로그래밍을 지원하고 있고, 비동기(asynchronous) 및 이벤트 중심(event-driven) 방식의 논블록킹(non-blocking) 방식으로 이러한 기능들을 지원하고 있다.

![image](https://user-images.githubusercontent.com/81155786/144877642-3a9cd99a-29df-4feb-a862-1b22d5f8d11d.png)

위와 같이, 이벤트 루프가 계속 돌며 요청이 발생하면 그에 맞는 핸들러에게 처리를 맡기고 call back 메서드를 통해서 응답을 반환받아 사용자에게 반환한다.

반응형 프로그래밍은 변경 사항에 대한 반응을 중심으로한 프로그래밍 모델인데 publisher-subscriber pattern(observer pattern)으로 이루어져있다.

반응형 프로그래밍에서는 request를 요청하고, 다른 작업을 수행한다. 데이터를 사용할 수 있게 되면 콜백 기능에 대한 데이터 정보와 함께 response를 받는다. 콜백 함수에서는 어플리케이션과 사용자의 요구에 따라 응답을 처리한다.

이 때 주의해야 할 점은, 빠른 consumer가 producer를 압도하지 않도록 event의 속도를 조절하는 것이 중요하다.

반응형 웹 프로그래밍은 스트리밍 데이터가 있는 어플리케이션과 데이터를 소비하고 사용자에게 스트리밍하는 클라이언트에게 적합하다. 전통적인 CRUD 어플리케이션 개발에는 적합하지 않고 데이터가 많은 Facebook이나 Twitter에는 적합할 수 있다.

## Spring MVC vs Spring WebFlux

![image](https://user-images.githubusercontent.com/81155786/144877711-f5608337-bbb0-4a03-9311-52d66b0807e9.png)

결국 Spring WebFlux는 Spring MVC의 병렬 버전이라고도 할 수 있고, Non-Blocking Reactive Programming을 지원한다.

다음에는 실제 WebFlux 구현코드가 SpringMVC와 어떤 차이가 있는지 알아보자.

## 참고 문서

- [https://docs.spring.io/spring-framework/docs/5.0.0.M5/spring-framework-reference/html/web-reactive.html](https://docs.spring.io/spring-framework/docs/5.0.0.M5/spring-framework-reference/html/web-reactive.html)
- [https://www.baeldung.com/spring-webflux](https://www.baeldung.com/spring-webflux)
- [https://velog.io/@dyllis/Spring-MVC-vs-WebFlux](https://velog.io/@dyllis/Spring-MVC-vs-WebFlux)
- [https://howtodoinjava.com/spring-webflux/spring-webflux-tutorial/](https://howtodoinjava.com/spring-webflux/spring-webflux-tutorial/)