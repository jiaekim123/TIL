# [스프링 핵심원리] 프로토타입 스코프
## 싱글톤 빈과 함께 사용 시 Provider로 문제 해결

- 싱글톤 빈과 프로토타입 빈을 함께 사용할 때, 어떻게 하면 사용할 때마다 항상 새로운 프로토타입 빈을 생성할 수 있을까?

## 1. 스프링 컨테이너에 요청
- 가장 간단한 방법: 싱글톤 빈이 프로토타입을 사용할 때마다 스프링 컨테이너에 새로 요청

![image](https://user-images.githubusercontent.com/37948906/144436114-c0bc52fd-1349-44f2-8b96-4777d4117b74.png)

![image](https://user-images.githubusercontent.com/37948906/144436205-e51e8930-1d2b-4278-9869-388d3856136b.png)

- DL(Dependency Lookup) 의존관계 조회 => 스프링 컨테이너에 종속적인 코드가 되고 단위 테스트도 어려워짐.
- 지금 필요한 기능은 지정한 프로토타입 빈을 컨테이너에서 대신 찾아주는 딱! "DL"정도의 기능만 제공하는 기능이 있다!

## ObjectFactory, ObjectProvider

![image](https://user-images.githubusercontent.com/37948906/144436536-53f32101-5c4c-4382-9f5f-3eb28904838e.png)

```java
public class SingletonWithPrototypeTest1 {

    @Test
    void prototypeFind() {
        AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(PrototypeBean.class);
        PrototypeBean bean1 = ac.getBean(PrototypeBean.class);
        bean1.addCount();
        assertThat(bean1.getCount()).isEqualTo(1);

        PrototypeBean bean2 = ac.getBean(PrototypeBean.class);
        bean2.addCount();
        assertThat(bean2.getCount()).isEqualTo(1);
    }

    @Test
    void singletonClientUsePrototype() {
        AnnotationConfigApplicationContext ac =
                new AnnotationConfigApplicationContext(ClientBean.class, PrototypeBean.class);
        ClientBean clientBean1 = ac.getBean(ClientBean.class);
        int count1 = clientBean1.logic();
        assertThat(count1).isEqualTo(1);

        ClientBean clientBean2 = ac.getBean(ClientBean.class);
        int count2 = clientBean2.logic();
        assertThat(count2).isEqualTo(1);
    }

    @RequiredArgsConstructor
    static class ClientBean {
        private final ObjectProvider<PrototypeBean> prototypeBeanProvider;

        public int logic() {
            PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
            prototypeBean.addCount();
            return prototypeBean.getCount();
        }

    }

    @Scope("prototype")
    static class PrototypeBean {
        private int count;

        public void addCount() {
            this.count++;
        }

        public int getCount() {
            return this.count;
        }

        @PostConstruct
        public void init() {
            System.out.println("PrototypeBean.init" + this);
            count = 0;
        }

        @PreDestroy // 호출 안됨.
        public void destroy() {
            System.out.println("PrototypeBean.destroy");
        }
    }
}

```

![image](https://user-images.githubusercontent.com/37948906/144437053-f5f3318f-8606-478a-ba6a-8ec86d3a9757.png)

![image](https://user-images.githubusercontent.com/37948906/144436993-724ac28c-45a9-40cf-9b4e-7065384fa7a4.png)

ObjectProvider -> ObjectFactory 상속
부수적인 기능이 있음.(옵션, 스트림 처리 등의 편의 기능이 있고 별도 라이브러리는 필요 없음. 스프링에 의존적)

![image](https://user-images.githubusercontent.com/37948906/144437377-845f91be-1224-44b6-a686-4f93fe8760a6.png)

SpringContainer 대신 조회하는 대리자정도의 역할

![image](https://user-images.githubusercontent.com/37948906/144437588-05c5809d-3941-434e-a8e3-ce3a92ba5ecc.png)

## JSR 330 Provider

단점: 라이브러리를 추가해줘야 함.

![image](https://user-images.githubusercontent.com/37948906/144437942-340be6ad-8c0b-4bac-b7e8-93362923c958.png)

provider 추가
![image](https://user-images.githubusercontent.com/37948906/144438145-19799cd6-f27c-4658-843c-f2754b3abded.png)

![image](https://user-images.githubusercontent.com/37948906/144438249-6af3d2b4-b8d4-48f5-9ae7-3fd6dfd5048b.png)

![image](https://user-images.githubusercontent.com/37948906/144438303-6bd91737-a3fd-4c11-b13e-a36fba65f9ee.png)

![image](https://user-images.githubusercontent.com/37948906/144438364-2017329d-09f0-486c-8816-fbc5e58a1522.png)

![image](https://user-images.githubusercontent.com/37948906/144438414-ad55c2fb-16e5-44eb-8642-502437111139.png)

## 특징

![image](https://user-images.githubusercontent.com/37948906/144438475-65c65b74-c71d-434c-bb8e-a620e1d4023c.png)

## 정리

![image](https://user-images.githubusercontent.com/37948906/144438671-e09e6c30-9abc-49ea-8068-e7a6a61262db.png)

언제 Provider를 사용하는가?
- 순환참조 의존 시, lazy나 optional 하게 사용할 때
![image](https://user-images.githubusercontent.com/37948906/144438818-4483460f-6ca6-41e3-b9f3-267517256820.png)

![image](https://user-images.githubusercontent.com/37948906/144438969-30dc4005-47a9-4fe7-b9d6-24da4c88d3d4.png)
