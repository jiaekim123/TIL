# [스프링 핵심 원리] IoC, DI, 그리고 컨테이너

# IoC, DI, 그리고 컨테이너

## 제어의 역전 IoC(Inversion of Control)
![image](https://user-images.githubusercontent.com/37948906/141606529-302d43ac-416e-48da-93b8-f017fdb7f770.png)

## 프레임워크 vs 라이브러리
- 프레임워크가 내가 작성하 코드를 제어하고, 대신 실행하면 그것은 프레임워크가 맞다. (Junit)
- 반면에 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 그것은 프레임워크가 아니라 라이브러리다. (의존성 추가해서 유틸을 사용하는 경우 등)

## 의존관계 주입 (DI)

<img width="659" alt="스크린샷 2021-11-13 오후 2 13 37" src="https://user-images.githubusercontent.com/37948906/141606620-1bb03030-f4eb-44d8-acb4-9ff2b91f996b.png">

정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스) 의존 관계를 분리해서 생각해야 한다.

앱이 돌때 실제로 구현체가 들어올지는 동적으로 객체 다이어그램을 통해 알 수 있음.
![image](https://user-images.githubusercontent.com/37948906/141606691-4ff3d2d4-9b22-4b06-a5cb-fe3591119996.png)

`의존 관계 주입을 사용하면 정적인 클래스 의존 관계를 수정하지 않고도 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있다.`

![image](https://user-images.githubusercontent.com/37948906/141606718-db5a06e6-59ff-4537-86db-46843f83d2b0.png)
