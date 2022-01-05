# [Chapter 0] 디자인 패턴

## 패턴

- 패턴이란? 많은 사람들이 겪은 문제나 해결 방법을 정리해둔 것
- 개발 분야에서 특정 문제를 유사한 유형으로 해결 → 해결 과정에서 발견된 방법을 패턴화

## 객체지향 설계 원칙 SOLID

- 단일 책임의 원칙 (Single responsibility principle, SRP)
- 개방 폐쇄의 원칙 (Open/closed principle, OCP)
- 리스코프 치환 원칙 (Liskov substitution principle, LSP)
- 인터페이스 분리 원칙 (Interface segregaion principle, ISP)
- 의존 관계 역전의 원칙 (Dependency inversion principle, DIP)

## GoF 디자인패턴

- 24개의 패턴
- 객체지향 설계 시 발생했던 문제를 카탈로그화하여 패턴으로 정리

## 패턴의 요소

- 이름(pattern name)
- 문제(problem)
- 해법(solution)
- 결과(consequence)

## 소프트웨어 유지보수와 방어적 설계

- 디자인 패턴을 적용하여 설계하는 목적 중 다른 하나는 유지보수성
- 오랫동안 문제없이 유지보수 하기 위해서 변경 가능한 디자인으로 설계
- 어떤 부분이 향후 수정될 것으로 예측된다면 해당 기능을 방어적으로 처리할 수 있도록 코드를 설계
- 방어적 설계를 위해 지속적으로 코드를 개선하는 리팩토링 작업 실시

## 정리

- 코드를 개발할 때는 유지보수성과 성능 고려