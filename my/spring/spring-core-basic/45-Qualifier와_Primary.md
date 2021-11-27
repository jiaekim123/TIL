# [스프링 핵심원리] @Autowired 필드 명, @Quilifier, @Primary

## 조회 대상 빈이 2개 이상일 때 해결 방법
- @Autowired 필드 명 매칭
- @Quilifier + @Quilifier 끼리 매칭 -> 빈 이름 매칭
- @Primary 사용

## 1. Autowired 필드 명 매칭
- @Autowired는 타입 매칭을 시도하고, 이때 여러 빈이 있으면 필드 이름, 파라미터 이름으로 빈 이름을 추가 매칭한다.

![image](https://user-images.githubusercontent.com/37948906/143669692-132a4549-4487-428d-bb40-dd974df6c2e8.png)

![image](https://user-images.githubusercontent.com/37948906/143669696-d9284c76-39a8-43f8-8577-ef4d93f6260c.png)

### @Autowired 매칭 정리
1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드 명, 파라미터 명으로 빈 이름 매칭

## 2. Quilifier 사용
@Quilifier는 추가 구분자를 붙여주는 방법이다. 주입 시 추가적인 방법을 제공하는 것이지 빈 이름을 변경하는 것은 아니다.

`빈 등록 시 @Qualifier를 붙여준다.`

![image](https://user-images.githubusercontent.com/37948906/143669812-591d78c0-266d-4415-891e-10c25c030359.png)

![image](https://user-images.githubusercontent.com/37948906/143669815-56b80905-24ac-41e1-9ba1-b5e632616da9.png)

![image](https://user-images.githubusercontent.com/37948906/143669822-a8127f25-1b09-45a9-ba98-e9f809d19a59.png)

주입 시에 @Qualifier를 붙여주고 등록한 이름을 적어준다.
![image](https://user-images.githubusercontent.com/37948906/143669872-289716a7-3d50-4aa3-9b09-1547dd68fb6b.png)

`@Qualifier`로 주입할 때 `Quailifier("mainDiscountPolicy")`를 못찾으면 어떻게 될까? 그러면 mainDiscountPolicy라는 이름의 스프링 빈을 추가로 찾는다. 하지만 경험상 @Qualifier는 @Qualifier를 찾는 용도로만 사용하는게 명확하고 좋다.

![image](https://user-images.githubusercontent.com/37948906/143669924-8c4f2c21-ada6-4712-89b4-997f4047de9c.png)

### @Qualifier 정리
1. @Qualifier끼리 매칭
2. 빈 이름 매칭
3. `NoSuchBeanDefinitionException` 예외 발생

## @Primary 사용
@Primary는 우선순위를 정하는 방법이다. @Autowired 시 여러 빈이 매칭이 되면 @Primary가 우선권을 가진다.

![image](https://user-images.githubusercontent.com/37948906/143669969-87b1f36c-0ddc-4d31-a9c9-5799c4bd26ec.png)

## @Primary vs @Qualifier
- @Qualifier의 단점: 주입 받을 때 모든 코드에 @Qualifier를 붙여주어야 한다.
- 반면에 @Primary를 사용하면 @Qualifier를 붙일 필요가 없다.

## @Primary, @Qualifier 활용
코드에서 자주 사용하는 메인 데이터베이스의 커넥션을 흭득하는 스프링 빈이 있고, 코드에서 특별한 기능으로 가끔 사용하는 서브 데이터베이스의 커넥션을 흭득하는 스프링 빈이 있다고 생각해보자.
메인 데이터베이스의 커넥션을 흭득하는 스프링 빈은 @Primary를 적용해서 조회하는 곳에서 @Qualifier 지정 없이 편리하게 조회하고, 서브 데이터베이스 커넥션 빈을 흭득할 때는 @Qualifier를 지정해서 명시적으로 획득하는 방식으로 사용하면 코드를 깔끔하게 유지할 수 있다. 물론 이때 메인 데이터베이스의 스프링 빈을 등록할 때 @Qualifier를 지정해주는 것은 상관 없다.

## 우선순위
@Primary는 기본값처럼 동작하는 것이고 @Qualifier는 매우 상세하게 동작한다. 이런 경우 어떤 것이 우선권을 가져갈까? 스프링은 자동보다는 수동이, 넓은 범위의 선택권보다는 좁은 범위의 선택권이 우선 순위가 높다. 따라서 여기서도 @Qualifier의 우선권이 높다.


