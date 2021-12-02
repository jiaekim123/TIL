# [스프링 핵심 원리] 빈 생명주기 콜백
## 애너테이션 @PostConstruct, @PreDestroy

![image](https://user-images.githubusercontent.com/37948906/144232287-1f61e67d-5570-4844-bfac-3132ede53b31.png)

![image](https://user-images.githubusercontent.com/37948906/144232354-0ddf58aa-50b3-4af2-ae7b-ad87226b6bb5.png)


```java
public class NetworkClient {
    private String url;


    public NetworkClient() {
        System.out.println("생성자 호출, url = " + url);
    }

    public void setUrl(String url) {
        this.url = url;
    }

    // 서비스 시작 시 호출
    public void connect() {
        System.out.println("connect: " + url);
    }

    public void call(String message) {
        System.out.println("call: " + url + " message: " + message);
    }

    // 서비스 종료 시 호출
    public void disconnect() {
        System.out.println("close: " + url);
    }

    @PostConstruct
    public void init() throws Exception {
        System.out.println("NetworkClient.init");
        connect();
        call("초기화 연결 메세지");
    }

    @PreDestroy
    public void close() throws Exception {
        System.out.println("NetworkClient.close");
        disconnect();
    }
}
```

```java
public class BeanLifeCycleTest {

    @Test
    public void likeCycleTest() {
        ConfigurableApplicationContext ac = new AnnotationConfigApplicationContext(LifeCycleConfig.class);
        NetworkClient client = ac.getBean(NetworkClient.class);
        ac.close();
    }

    @Configuration
    static class LifeCycleConfig {

        @Bean
        public NetworkClient networkClient() {
            NetworkClient networkClient = new NetworkClient();
            // 객체를 만든 이후에 값이 셋팅되는 경우
            networkClient.setUrl("http://hello-spring.com");
            return networkClient;
        }
    }
}

```

![image](https://user-images.githubusercontent.com/37948906/144232413-e94b1272-3e66-4264-a139-68d948dd952e.png)


### @PostConstructor, @PreDestory 애너테이션 확장
- 최신 스프링에서 가장 권장하는 방법
- 애노테이션 하나만 붙이면 되므로 매우 편리하다.
- 패키지를 잘 보면 `javax.annotation.PostConstructor`이다. 스프링에 종속적인 기술이 아니라 JSR 250이라는 자바 표준이다. 따라서 스프링이 아닌 다른 컨테이너에서도 동작한다.
- 컴포넌트 스캔과 잘 어울린다.
- 유일한 단점은 외부 라이브러리에는 적용하지 못한다는 것이다. 외부 라이브러리를 초기화, 종료해야 하면 @Bean의 기능을 사용하자.

## 정리
- `@PostConstruct, @PreDestroy` 애너테이션을 사용하자.
- 코드를 고칠 수 없는 외부 라이브러리를 초기화, 종료해야 하면 `@Bean`의 initMethod, destroyMethod를 사용하자.