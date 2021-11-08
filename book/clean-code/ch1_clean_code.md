## 1장. 깨끗한 코드

### 클린코드 책의 목표

-   좋은 코드와 나쁜 코드를 구분하는 능력을 갖춘다.
-   좋은 코드를 작성하는 방법을 익힌다.
-   나쁜 코드를 좋은 코드로 바꾸는 실력도 쌓는다.

### 코드가 존재하리라

-   `코드는 요구사항을 상세히 표현하는 수단`이다.
    -   어느 수준에 이르면 코드의 도움 없이 요구사항을 상세히 표현하기란 불가능하다. 추상화도 불가능하다.
    -   고도로 추상화된 언어나 특정 응용 분야 언어로 기술하는 명세 역시 코드이므로 결코 코드는 사라지지 않는다.

### 나쁜 코드

-   `회사가 망한 원인은 바로 나쁜 코드 탓이었다.`
    
    -   80년대 후반 킬러 앱 하나를 구현한 회사는 나쁜 코드로 인한 버그로 인해 망했다.
    -   버그는 다음 버전에도 그대로 남아있고, 프로그램 시동 시간이 길어지고 죽는 횟수도 늘었다.
    -   원인은 출시에 바빠 코드를 마구 짜서, 기능을 추가할수록 코드가 엉망이 되었던 탓었다.
-   `르블랑의 법칙 - 나중은 결코 오지 않는다.`
    
    -   우리 모두는 자신이 짠 쓰레기 코드를 보며 나중에 손보겠다고 생각한 경험이 있다. 하지만 나중은 결코 오지 않는다.

### 나쁜 코드로 치르는 대가

-   `나쁜 코드가 쌓일 수록 팀 생산성은 떨어진다.`
    -   나쁜 코드는 코드를 고칠 때마다 엉뚱한 곳에서 문제가 생긴다.
    -   생산성을 증가시키기 위해 투입한 신규 인력은 시스템 설계 의도를 분명하게 알지 못해 결국 더 많은 나쁜 코드를 양산한다.

#### 원대한 재설계의 꿈

-   나쁜 코드에 의해 생산성이 바닥이 되면 반기를 들고 재설계를 하는 경우가 생긴다. 그러나 이 팀에서는 기존 시스템 기능을 모두 제공하는 새 시스템을 내놓으며 기존 시스템에 가해지는 변경까지 모두 반영해야 한다.
-   새 시스템이 기존 시스템을 따라 잡을 즈음이면, 초창기 재설계팀은 모두 팀을 떠났고 새로운 팀원들이 새 시스템을 설계하자고 한다. 왜? 현재 시스템이 너무 엉망이기 때문이다.
-   `결론적으로, 처음부터 깨끗한 코드를 만들어야 한다.`

#### 태도

-   `나쁜 코드의 위험을 이해하지 못하는 관리자의 말을 그대로 따르는 행동은 전문가답지 못하다`
    -   나쁜 코드가 되는 이유는 요구사항과 일정, 관리자와 마케팅 등의 잘못이라고 할 수 없다. 프로그래머가 전문가답지 못했기 때문이다.
    -   사용자는 요구사항을 내놓으며 개발자에게 현실성을 자문한다. 프로젝트 관리자는 일정을 잡으며 우리에게 도움을 청한다. 그러므로 프로젝트 실패는 개발자에게도 커다란 책임이 있다. 특히 나쁜 코드가 초래하는 실패에는 더더욱 책임이 크다.

#### 원초적 난제

-   프로그래머가 기한을 맞추려면 나쁜 코드를 양산할 수 밖에 없는가?
    -   틀렸다. 나쁜 코드를 양산하면 기한을 맞추지 못한다. 기한을 맞추는 유일한 방법은 언제나 코드를 최대한 깨끗하게 유지하는 습관이다.

#### 깨끗한 코드라는 예술?

-   `깨끗한 코드는 어떻게 작성할까?`
    -   깨끗한 코드가 무엇인지 모르면 깨끗한 코드를 만들려고 애써봤자 소용이 없다.
    -   깨끗한 코드를 작성하려면 '청결'이라는 힘겹게 습득한 감각을 활용해 자잘한 기법들을 적용하는 절제와 규율이 필요하다. 열쇠는 '코드 감각'이다.
-   `코드 감각`
    -   코드감각이 있으면 좋은 코드와 나쁜 코드를 구분한다.
    -   코드감각이 있으면 절제와 규율을 적용해 나쁜 코드를 좋은 코드로 바꾸는 전략도 파악한다.

#### 깨끗한 코드란?

각 분야의 유명한 프로그래머들의 깨끗한 코드에 대한 의견

##### 비야네 스트롭스트룹

C++ 창시자이자 C++ Programing Language 저자

-   우아하고 효율적인 코드
-   논리가 간단해야 버그가 숨어들지 못한다.
-   의존성을 최대한 줄여야 유지보수가 쉬워진다.
-   성능을 최적으로 유지해야 사람들이 원칙없는 최적화로 코드를 망치려는 유혹에 빠지지 않는다.
-   깨끗한 코드는 한가지를 제대로 한다.

##### 그래디 부치

Object Oriendted Analysis and Design with Application 저자

-   깨끗한 코드는 단순하고 직접적이다.
-   깨끗한 코드는 잘 쓴 문장처럼 읽힌다.
-   깨끗한 코드는 설계자의 의도를 숨기지 않는다. 오히려 명쾌한 추상화와 단순한 제어문으로 가득하다.

##### 큰 데이브 토마스

OTI 창립자이자 이클립스 전략의 대부

-   깨끗한 코드는 작성자가 아닌 사람도 읽기 쉽고 고치기 쉽다.
-   단위 테스트 케이스와 인수 테스트 케이스가 존재한다.
-   깨끗한 코드에는 의미 있는 이름이 붙는다.
-   특정 목적을 달성하는 방법은 (여러가지가 아니라) 하나만 제공한다.
-   의존성은 최소이며 각 의존성을 명확히 정의한다.
-   API는 명확하며 최소로 줄였다.
-   사람이 읽기 좋은 코드를 작성한 것이 좋은 코드이다.

##### 마이클 페더스

Working Effectively with Legacy Code 저자

-   깨끗한 코드는 주의 깊게 작성한 코드다.

##### 존 제프리스

Extreme Programming Installed 와 Extreme Programming Adventure in C# 저자

-   모든 테스트를 통과한다.
-   중복이 없다.
-   시스템 내 모든 설계 아이디어를 표현한다.
-   클래스, 메서드, 함수 등을 최대한 줄인다.

##### 워드 커닝햄

위키 창시자

-   코드를 읽으면서 짐작했던 기능을 각 루틴이 그대로 수행한다면 깨끗한 코드다.

##### 우리들 생각(클린코드)

-   깨끗한 변수 이름, 깨끗한 함수, 깨끗한 클래스를 만드는 방법을 소개할 것.
-   오브젝트 멘토 진영이 생각하는 깨끗한 코드를 설명한다.
-   이 책에서 주장하는 기법 다수는 논쟁의 여지가 있다. 그러나 오랫동안 고민하고 숙고한 교훈과 기법을 권고한다.
-   우리는 코드를 읽는 시간 대 코드를 짜는 시간 비율이 10:1을 훌쩍 넘는다. 그러므로 쉽게 짜려면 읽기 쉽게 만들어야 한다.

##### 보이스카우트 규칙

-   잘 짠 코드가 전부가 아니다. 시간이 지나도 언제나 깨끗하게 유지해야 한다.
-   `캠프장은 처음 왔을 때보다 더 깨끗하게 해놓고 떠나라.`
-   한꺼번에 많은 시간과 노력을 투자해 코드를 정리할 필요가 없다. 변수 이름 하나 개선하고, 조금 긴 함수 하나 분할하고, 약간의 중복을 제거하고, 복잡한 if문 하나를 정리하면 충분하다.

##### 프리퀼과 원칙

-   이 책은 많은 부분이 PPP(Agile Software Development: Prin-ciples, Patterns, and Practices) 객체 지향 설계의 원칙을 설명하는 책에서 하는 이야기를 이어간다. (SOLID 원칙 관련)

##### 결론

-   이 책은 도구와 기법, 그리고 생각하는 방식을 소개할 뿐이다. 이 책을 읽는다고 뛰어난 프로그래머가 되리라는 보장은 없다. '코드 감각'을 얻는다는 보장도 없다.
-   방법을 알았으면, `연습하자!`