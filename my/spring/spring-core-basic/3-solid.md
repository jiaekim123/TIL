# 좋은 객체 지향 설계의 5가지 원칙

클린코드로 유명한 로버트 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리

- SRP: 단일 책임 원칙(Single Responsibility Principle)
- OCP: 개방 폐쇄 원칙(Open/Closed Principle)
- LSP: 리스코프 치환 원칙(Liskov Substitution Principle)
- ISP: 인터페이스 분리 원칙(Interface Segregation Principle)
- DIP: 의존관계 역전 원칙(Dependency Inversion Principle)

## SRP 단일 책임 원칙
Single responsibility principle

- 한 클래스는 하나의 책임만 가져야 한다.
- 하나의 책임이라는 것은 모호함.
  - 클 수도 있고, 작을 수 있다.
  - 문맥과 상황에 따라 다르다.
- 중요한 기준은 변경이다. 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것

## OCP 개방 폐쇄 원칙
Open Closed Principle
가장 중요!!
- 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
- 확장을 하려면 당연히 기존 코드를 변경해야 하는 것 아닌가?! => 다형성을 활용해보자
- 인터페이스를 구현한 클래스를 하나 만들어서 새로운 기능을 구현한다.
- 역할과 구현을 분리시켜서 마치 아반떼 -> 테슬라로 바꾼다고 해도 운전자가 운전하는데 아무 문제 없는 것과 같음.

### 문제점

![image](https://user-images.githubusercontent.com/37948906/140946728-089d96a4-d736-4fcb-b490-977cc46fabe0.png)

조립/설정자 => 스프링 컨테이너에서 해주는 DI의 역할

## LSP 리스코프 치환 원칙

![image](https://user-images.githubusercontent.com/37948906/140947060-ee6dae1e-0093-4a71-bd0e-7a07a770ceff.png)

## ISP 인터페이스 분리 원칙
![image](https://user-images.githubusercontent.com/37948906/140947246-2a3bdbbe-7305-4b0f-8b2f-70ce26501f70.png)

## DIP 의존관계 역전 원칙
![image](https://user-images.githubusercontent.com/37948906/140947470-f1ff97d9-5c1f-468f-ae13-10d876969d13.png)

![image](https://user-images.githubusercontent.com/37948906/140948194-e1685fd0-2942-42a2-a97f-8528640807f9.png)


## 정리
- 객체 지향의 핵심은 다형성
- 다형성만으로는 쉽게 부품을 갈아 끼우듯이 개발할 수 없다.
- 다형성 만으로는 구현 객체를 변경할 때 클라이언트 코드도 함께 변경된다.
- 다형성 만으로는 OCP, DIP를 지킬 수 없다.
- 뭔가 더 필요하다.

## 다음 시간
객체 지향 설계와 스프링