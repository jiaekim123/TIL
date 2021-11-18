# [스프링 핵심 원리] 스프링 빈 설정 메타 정보 - BeanDefinition

![image](https://user-images.githubusercontent.com/37948906/142403397-3ba26afa-d07b-4259-9e60-fb4aa09ada95.png)

BeanDefinition 추상화 => 역할과 구현을 개념적으로 나눈 것
- xml을 읽어서 BeanDefinition을 만들면 된다.
- 자바 코드를 읽어서 BeanDefinition을 만들면 된다.
- 스프링 컨테이너는 자바 코드인지, XML인지 몰라도 된다. 오직 BeanDefinition만 알면 된다.

코드 레벨은 좀더 깊이 있게 들어가보자.
![image](https://user-images.githubusercontent.com/37948906/142403793-c3f13706-647c-4b41-8418-467bb93ef91f.png)

AnnotationConfigApplicationContext
![image](https://user-images.githubusercontent.com/37948906/142404098-e85d8101-04ad-4b86-9e8e-f664477dacba.png)

GenericXmlApplicationContext
![image](https://user-images.githubusercontent.com/37948906/142404149-6df5e8c9-a707-4b8b-9b6d-ef78b7a1737c.png)

BeanDefinition 

BeanDefinition 정보
- BeanClassName: 생성할 빈의 클래스 명(자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음)
- FactoryBeanName: 팩토리 역할의 빈을 사용할 경우 이름, 예) appConfig
- factoryMethodName: 빈을 생성할 팩토리 메서드 지정, 예) memberService
- Scope: 싱글톤(기본 값)
- lazyInit: 스프링 컨테이너를 생성할 때 빈을 생성하는 것이 아니라, 실제 빈을 사용할 때까지 최대한 생성을 지연처리하는지 여부
- InitMethodName: 빈을 생성하고, 의존관계를 적용한 뒤에 호출되는 초기화 메서드 명
- DestroyMethodName: 빈의 새명주기가 끝나서 제거하기 직전에 호출되는 메서드 명
- Constructor arguments, Properties: 의존 관계 주입에서 사용. (자바 설정 처럼 팩토리 역할의 빈을 사용하면 없음.)

![image](https://user-images.githubusercontent.com/37948906/142405097-4a12ca8e-c926-4f61-817d-983c50218151.png)

![image](https://user-images.githubusercontent.com/37948906/142405076-f410b899-dc4d-49ac-8ddf-5f9555e7b224.png)


정리
BeanDefinition을 직접 생성해서 스프링 컨테이너에 등록할수도 있다. 하지만 실무에서 직접 정의하거나 사용하는 일은 거의 없음.
BeanDefinition에 대해 너무 깊이있게 이해하기보다는 스프링이 다양한 형태의 설정 정보를 BeanDefinition으로 추상화해서 사용하는 정도만 이해하면 됨.
스프링 프레임워크 내에서 가끔 보이는 경우가 보이는데 이런 경우 이해하는 정도면 됨.


