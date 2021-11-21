# [스프링 핵심 원리] Configuration과 바이트 조작의 마법

스프링 컨테이너는 싱글톤 레지스트리다. 따라서 스프링 빈이 싱글톤이 되도록 보장해주어야 한다. 그런데 스프링이 자바 코드까지 어떻게 하기는 어렵다. 저 자바 코드를 보면 분명 3번 호출되어야 하는게 맞다.
그래서 스프링은 클래스의 바이트코드를 조작하는 라이브러리를 사용한다.

모든 비밀은 `@Configuration`을 적용한 `AppConfig`에 있다!

```java
    @Test
    void configurationDeep() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);
        AppConfig bean = ac.getBean(AppConfig.class);
        System.out.println("bean.getClass() = " + bean.getClass());
    }
```

![image](https://user-images.githubusercontent.com/37948906/142749325-a5fc0b2e-1682-46fe-b12d-f49c1442404f.png)

순수한 클래스라면  `class hello.core.AppConfig`라고 나와야 한다.

그런데 실제로 출력해보면 예상과 다르게 클래스 명에 xxxCGUB가 붙으면서 상당히 복잡해진 것을 볼 수 있다. 이것은 내가 만든 클래스가 아니라 스프링이 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig 클래스를 상속받은 임의의 다른 클래스를 만들고, 그 다른ㄴ 클래스를 스프링 빈으로 등록한 것이다!
`bean.getClass() = class com.example.springcorebasic.AppConfig$$EnhancerBySpringCGLIB$$339f1ef1
`

![image](https://user-images.githubusercontent.com/37948906/142749394-1a08a0dd-3f91-43b1-9f4c-b09be527ef44.png)

그 임의의 다른 클래스가 바로 싱글톤이 보장되도록 해준다. 아마도 다음과 같이 바이트코드를 조작해서 작성되어 있을 것이다. (실제로는 CGLIB의 내부 기술을 사용하는데 매우 복잡하다.)

![image](https://user-images.githubusercontent.com/37948906/142749418-4f0c80ba-c08f-49be-af40-59e7844b84df.png)

@Bean이 붙은 메서드 마다 이미 스프링 빈이 존재하면 존재하는 빈을 반환하고, 스프링 빈이 없으면 생성해서 스프링 빈으로 등록하고 ㅂ나환하는 코드가 동적으로 만들어진다.
덕분에 싱글톤이 보장되느 ㄴ것이다!

> 참고: AppConfig@CGLIB은 AppConfig의 자식 타입이므로 AppConfig 타입으로 조회할 수 있다.

만약 `@Configuration`으로 적용하지 않고 `@Bean`만 적용하면 어떻게 될까?

![image](https://user-images.githubusercontent.com/37948906/142750195-022bac7f-1898-41a9-92ed-51b0e81a6188.png)

![image](https://user-images.githubusercontent.com/37948906/142750190-280223e6-6d69-4c4e-a65e-25faacf6f715.png)

![image](https://user-images.githubusercontent.com/37948906/142750211-1fd2f567-bd43-4630-b378-ac97ef198424.png)

@Configuration을 붙이면 바이트코드를 조작하는 CGLIB 기술을 사용해서 싱글톤을 보장하지만, 만약 @Bean만 적용하면 위와 같이 MemberRepository가 세 번 호출되면서  @Beean에 의해 스프링 컨테이너에 등록할 때, memberService에서 호출할 때, orderService에서 호출할 때 각 세번 호출된다.

![image](https://user-images.githubusercontent.com/37948906/142750246-5c8a1cea-0a93-4286-95ce-db26e2756985.png)

그래서 테스트트코드를 돌렸을 때에도 다른 애라고 뜸.

결국 이렇게 된애들은 스프링 컨테이너가 관리하는 애들이 아니라 new해서 매번 생성하는거랑 똑같음.

![image](https://user-images.githubusercontent.com/37948906/142750271-5ac6328a-a1f5-47ca-bb0d-669561435388.png)

여기서 인텔리제이가 알려주기도 함.

정리
- @Bean만 사용해도 스프링 빈으로 등록되지만, 싱글톤을 보장하지 않는다.
  - memberRepository 처럼 의존 관계 주입이 필요해서 메서드를 직접 호출할 때 싱글톤을 보장하지 않는다.
- 크게 고민할 것 없다. 스프링 설정 정보는 항상 `@Configuration`을 사용하자!