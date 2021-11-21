# [스프링 핵심원리] @Configuration과 싱글톤

![image](https://user-images.githubusercontent.com/37948906/142748971-45ca5242-756e-4186-a91d-f7918a8812bd.png)

결과적으로 각각 다른 2개의 MemoryMemberRepository 가 생성되면서 싱글톤이 깨지는 것처럼 보인다. 스프링 컨테이너는 이 문제를 어떻게 해결할까?

![image](https://user-images.githubusercontent.com/37948906/142749013-be500bc4-e42e-468d-99f5-9be2461b93cd.png)

![image](https://user-images.githubusercontent.com/37948906/142749026-fa49da92-19a5-4741-b0a8-a01c57d61a55.png)

![image](https://user-images.githubusercontent.com/37948906/142749109-c4f0ec4f-fbca-4e4f-91ff-3da21e5d36b7.png)

![image](https://user-images.githubusercontent.com/37948906/142749110-8e1f8966-313f-4a08-8b57-7d935fd5c91b.png)


```java
public class ConfigurationSingletonTest {

    @Test
    void configurationTest() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

        MemberServiceImpl memberService = ac.getBean("memberService", MemberServiceImpl.class);
        OrderServiceImpl orderService = ac.getBean("orderService", OrderServiceImpl.class);
        MemberRepository memberRepository = ac.getBean("memberRepository", MemberRepository.class);

        MemberRepository memberRepository1 = memberService.getMemberRepository();
        MemberRepository memberRepository2 = orderService.getMemberRepository();

        System.out.println("memberService -> memberRepository1 = " + memberRepository1);
        System.out.println("orderService -> memberRepository2 = " + memberRepository2);
        System.out.println("memberRepository = " + memberRepository);

        Assertions.assertThat(memberRepository1).isEqualTo(memberRepository2);
    }
}
```

![image](https://user-images.githubusercontent.com/37948906/142749104-1b93fdc2-e654-4e76-acc8-1f71fb93419b.png)

> [꿀팁] soutm => 현제 메서드 print 찍기

```java
@Configuration
public class AppConfig {

    @Bean
    public MemberService memberService() {
        System.out.println("AppConfig.memberService");
        return new MemberServiceImpl(memberRepository());
    }

    @Bean
    public MemoryMemberRepository memberRepository() {
        System.out.println("AppConfig.memberRepository");
        return new MemoryMemberRepository();
    }

    @Bean
    public OrderService orderService() {
        System.out.println("AppConfig.orderService");
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }
}
```
![image](https://user-images.githubusercontent.com/37948906/142749173-3b4b6708-de0b-4ef3-8255-d2b536d54f51.png)
