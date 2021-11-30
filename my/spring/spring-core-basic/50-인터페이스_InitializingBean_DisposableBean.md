# [스프링 핵심원리] 인터페이스 InitializingBean, DisposableBean

![image](https://user-images.githubusercontent.com/37948906/144041482-10ce416b-1f6d-4324-9a3e-383e23fafca1.png)

![image](https://user-images.githubusercontent.com/37948906/144041542-004b4dc1-d7e7-477e-8b4e-0c820642703e.png)

![image](https://user-images.githubusercontent.com/37948906/144041602-ec6f43ca-1d59-4393-aaf8-fd86071fcb77.png)

![image](https://user-images.githubusercontent.com/37948906/144041667-adfbc91a-08ab-4bcc-a007-6364735e6f9f.png)

![image](https://user-images.githubusercontent.com/37948906/144041707-c5f9fe52-ecd3-4efc-870c-1845540b128c.png)

![image](https://user-images.githubusercontent.com/37948906/144041776-49ccf918-dd0c-48a8-8dcc-d721e67f681f.png)

## 초기화, 소멸 인터페이스 단점
- 이 인터페이스는 스프링 전용 인터페이스다. 해당 코드가 스프링 전용 인터페이스에 의존한다.
- 초기화, 소멸 메서드의 이름을 변경할 수 없다
- 내가 코드를 고칠 수 없는 외부 라이브러리에 적용할 수 없다.

> 참고: 인터페이스를 사용하는 초기화, 종료 방법은 스프링 초창기에 나온 방법들이고, 지금은 다음의 더 나은 방법들이 있어서 거의 사용되지 않는다.