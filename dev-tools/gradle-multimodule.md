# Gradle Multimodule

## 멀티모듈 코드
`git clone https://github.com/spring-guides/gs-multi-module.git`
처음부터 생성을 따라해보고, 완성본 코드와 비교해보자.
위 코드에는 Gradle과 Maven으로 작업하는데 필요한 코드가 포함되어 있다.

## 1. Root Project 생성
이 가이드는 두 개의 프로젝트를 구축하는 방법을 안내한다. 그 중 하나는 다른 프로젝트에 종속되어 있다. 루트 프로젝트 아래에 두 개의 하위 프로젝트를 만든다. 
```
gs-multi-module (root)
  ㄴ library
  ㄴ application 
```
##### Maven
먼저 최상위 root에서 빌드 구성을 만든다. Maven의 경우 하위 디렉토리 pom.xml를 <modules>에 나열한다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.springframework</groupId>
    <artifactId>gs-multi-module</artifactId>
    <version>0.1.0</version>
    <packaging>pom</packaging>

    <modules>
        <module>library</module>
        <module>application</module>
    </modules>

</project>
```
##### Gradle
Gradle은 `setting.gradle`을 최상위 디렉토리에 만든다.
```
rootProject.name = 'gs-multi-module'

include 'library'
include 'application'
```
최상위 프로젝트에는 선택적으로 비어있는 `build.gradle`파일을 추가할 수 있다. (단지 IDE가 root 프로젝트를 식별하는데 도움을 주는 역할)

## 2. Directory 구조 생성
```
  ㄴ library
  ㄴ application 
```
루트 디렉토리 하위에 디렉터리 `library`와 `application` 폴더를 생성한다.

## 3. Library 프로젝트를 생성한다.
두 프로젝트 중 하나는 다른 프로젝트(application)에서 사용할 라이브러리를 제공한다.
### 3.1 Library 디렉토리 구조 생성하기
```
└── src
    └── main
        └── java
            └── com
                └── example
                    └── multimodule
                        └── service
```
Spring Boot 플러그인은 라이브러리 프로젝트에서 전혀 사용되지 않는다. 단, Spring Boot 종속성 관리를 활용하고자 `spring-boot-starter-parent` 를 Spring Boot 상위 프로젝트로 사용하여 구성한다. 

혹은 대안으로 `pom.xml`파일의 `<dependencyManagement/>` 섹션에서 `BOM(Bill of Materials)`을 사용할 수 있다.

### 3.2 Library 프로젝트 셋팅하기
Libary 프로젝트의 경우에 종속성을 추가할 필요가 없다. `spring-boot-stater` 종속성은 필요한 모든걸 제공한다.

##### Maven
`library/pom.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>multi-module-library-initial</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>multi-module-library-initial</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```
##### Gradle
`/library/build.gradle`
```groovy
plugins {
	id 'org.springframework.boot' version '2.5.2'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

### 3.3 Libary Project 조정하기
라이브러리 프로젝트를 생성한 경우 `start.spring.io` 빌드 시스템에 대한 wrapper script가 포함된다. (mvnw 또는 gradlew) 해당 스크립트 및 관련 구성을 루트 디렉토리로 이동할 수 있다.

```
$ mv mvnw* .mvn ..
$ mv gradlew* gradle ..
```

라이브러리 프로젝트에는 기본 메서드가 있는 클래스가 없다. (Application이 아니기 때문에)
그러므로 라이브러리 프로젝트에 대해 실행 가능한 jar를 빌드하지 않도록 빌드하지 않도록 빌드 시스템에 알려야 한다.

##### Maven

Maven 라이브러리 프로젝트에 대한 실행 가능한 jar를 빌드하지 않도록 지시하려면 `pom.xml` Spring Initializr에 의해 생성된 아래 블록을 제거한다.

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-maven-plugin</artifactId>
    </plugin>
  </plugins>
</build>
```

다음은 Libary project의 최조 `pom.xml` 파일이다.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>library-complete</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>multi-module-library-complete</name>
	<description>Demo project for Spring Boot</description>

	<properties>
		<java.version>1.8</java.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

</project>
```

#### Gradle
gradle에서는 Library 프로젝트에서 실행가능한 jar를 빌드하지 않도록 하기 위해서는 반드시 `build.gradle`에 아래 블록을 추가해야 한다.
```groovy
bootJar {
  enabled = false
}

jar {
  enabled = true
}
```

이 `bootJar` 작업은 실행 가능한 jar를 생성하려고 시도하며 이를 위해서는 main() 메서드가 필요하다. 결과적으로 bootJar 작업을 비활성화하고 실행 가능한 jar가 아닌 일반 jar를 생성하게 하기 위해 `jar`를 enable로 설정해주어야 한다.

아래는 최종 완성된 libary 프로젝트의 `build.gradle` 파일이다.
```groovy
plugins {
	id 'org.springframework.boot' version '2.5.2'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

bootJar {
	enabled = false
}

jar {
	enabled = true
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

## 4. Service Component 생성하기
library는 application에서 사용되는 `MyService` 클래스를 제공한다. 
##### MyService.java
`library/src/main/java/com/example/multimodule/service/MyService.java`

```java
package com.example.multimodule.service;

import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.stereotype.Service;

@Service
@EnableConfigurationProperties(ServiceProperties.class)
public class MyService {

  private final ServiceProperties serviceProperties;

  public MyService(ServiceProperties serviceProperties) {
    this.serviceProperties = serviceProperties;
  }

  public String message() {
    return this.serviceProperties.getMessage();
  }
}
```

`application.properties`에서 구성할 수 있도록 `@ConfigurationProperties` 클래스를 추가할 수도 있다.

##### ServiceProperties
`library/src/main/java/com/example/multimodule/service/ServiceProperties.java`

```java
package com.example.multimodule.service;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties("service")
public class ServiceProperties {

  /**
   * A message for the service.
   */
  private String message;

  public String getMessage() {
    return message;
  }

  public void setMessage(String message) {
    this.message = message;
  }
}
```
라이브러리는 순수한 java apis만 제공하고 spring 기능을 제공하지 않을 수 있으니, 이럴 경우에는 라이브러리를 사용하는 어플리케이션은 configuration 자체를 제공해야 한다.

## 5. Service Component 테스트하기

재사용 가능한 Spring Configuration을 라이브러리의 일부로 제공하는 경우 구성이 작동하는지 확인하기 위한 통합 테스트를 작성할 수 있다. 이 경우 Junit과 `@SpringBootTest` 어노테이션을 사용할 수 있다. 

##### MyServiceTest.java
`library/src/test/java/com/example/multimodule/service/MyServiceTest.java`

```java
package com.example.multimodule.service;

import static org.assertj.core.api.Assertions.assertThat;

import org.junit.jupiter.api.Test;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest("service.message=Hello")
public class MyServiceTest {

  @Autowired
  private MyService myService;

  @Test
  public void contextLoads() {
    assertThat(myService.message()).isNotNull();
  }

  @SpringBootApplication
  static class TestConfiguration {
  }

}
```

## 6. Application Project 생성하기
application 프로젝트는 다른 프로젝트에서 사용할 수 있는 서비스를 제공하는 library 프로젝트를 사용한다.

### 6.1 Directory 구조 만들기

application 프로젝트에서 아래 디렉토리 구조를 생성한다.

```
└── src
    └── main
        └── java
            └── com
                └── example
                    └── multimodule
                        └── application
```
> application에서 `@ComponentScan`에 의해 library의 모든 Spring 구성 요소를 포함하려는 경우가 아니면 libary와 동일한 패키지를 이용하지 말자.

### 6.2 Application Project 셋팅하기
Application 프로젝트를 위해, Spring Boot Actuator와 Spring Web 디펜던시가 필요하다.

##### Maven
Spring Initializr에서 필요한 종속성을 가진 Maven 빌드 파일을 얻을 수 있다. 다음은 Maven을 선택하면 자동으로 생성되는 pom.xml 파일이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>multi-module-application-initial</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>multi-module-application-initial</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

##### Gradle
Spring initializr를 통해 직접 필요한 종속성이 있는 gradle 파일을 얻을 수 있다. 다음은 gradle을 선택했을 때 자동으로 생성되는 `build.gradle`파일이다.

```groovy
plugins {
	id 'org.springframework.boot' version '2.5.2'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-actuator'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

mvnw나 gradlew 래퍼 및 관련 구성 파일을 삭제할 수 있다.
```
$ rm -rf mvnw* .mvn
$ rm -rf gradlew* gradle
```

### 6.3 Application 프로젝트에 Library dependency 추가하기
application 프로젝트는 libary 프로젝트 디펜던시가 필요하므로 그에 따른 application 빌드 파일을 수정해야 한다.

##### Maven
pom.xml 파일에 아래 dependency를 추가한다.
```xml
<dependency>
  <groupId>com.example</groupId>
  <artifactId>library</artifactId>
  <version>${project.version}</version>
</dependency>
```

아래는 최종 pom.xml 파일이다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.5.2</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
	<groupId>com.example</groupId>
	<artifactId>multi-module-application-complete</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>multi-module-application-complete</name>
	<description>Demo project for Spring Boot</description>
	<properties>
		<java.version>1.8</java.version>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-actuator</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>com.example</groupId>
			<artifactId>library</artifactId>
			<version>${project.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>
```

##### Gradle
gradle에서는 아래 dependency를 추가한다.
```groovy
implementation project(':library')
```

최종 `build.gradle` 파일은 다음과 같다.

```groovy
plugins {
	id 'org.springframework.boot' version '2.5.2'
	id 'io.spring.dependency-management' version '1.0.11.RELEASE'
	id 'java'
}

group = 'com.example'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'

repositories {
	mavenCentral()
}

dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-actuator'
	implementation 'org.springframework.boot:spring-boot-starter-web'
	implementation project(':library')
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

## 7. Application 작성하기

application의 main class는 libary의 `Service`를 랜더링하는 `@RestController`가 될 수 있다. 

##### DemoApplication.java
`application/src/main/java/com/example/multimodule/application/DemoApplication.java)`
```java
package com.example.multimodule.application;

import com.example.multimodule.service.MyService;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication(scanBasePackages = "com.example.multimodule")
@RestController
public class DemoApplication {

  private final MyService myService;

  public DemoApplication(MyService myService) {
    this.myService = myService;
  }

  @GetMapping("/")
  public String home() {
    return myService.message();
  }

  public static void main(String[] args) {
    SpringApplication.run(DemoApplication.class, args);
  }
}
```

`@SpringBootApplication` 어노테이션은 다음을 모두 추가하는 편리한 어노테이션이다.
- `@Configuration`: application context에 대한 빈 정의 소스로 클래스에 태그로 지정한다.
- `@EnableAutoConfiguration`: Spring boot에 클래스 경로 지정, 기타 빈 및 다양한 속성을 기반으로 한 Bean 추가를 지시한다. 예를 들어, 만약 `spring-webmvc`가 클래스 패스에 있다면, 이 주석은 웹 어플리케이션으로 플래그를 지정하고 `DispatcherServlet` 같은 주요 동작을 활성화한다.
- `@ComponentScan`: 스프링이 `com.example` 패키지에 있는 다른 components, configuration, services를 찾도록 지시하여 컨트롤러를 찾는다.

`main()` 메서드는 Spring Boot의 `SpringApplication.run()` 메서드를 사용하여 application을 실행한다. 

`web.xml` 파일은 없다. 이 웹 어플리케이션은 100% 순수 Java로 구성되어 어떤 plumbing이나 인프라를 설정할 필요가 없다.

왜냐하면 `DemoApplication`은 `MyService(com.example.multimodule.service)`와 다른 패키지`(com.example.multimodule.application)` 안에 있기 때문에 `@SpringBootApplication`은 이를 자동으로 감지할 수 없다.

MyService가 선택되도록 하는 방법은 아래와 같이 여러 가지가 있다.

- `@Import(MyService.class)`로 직접적으로 Import 하는 방법
- `@SpringBootApplication(scanBasePackageClasses={...})`를 사용하여 해당 패키지로부터 모든 것을 가져오는 방법
- 이름으로 상위 패키지를 지정하는 방법: `com.example.multimodule` 이 가이드에서는 이런 방법을 사용하고 있다.

> 만약 JPA나 Spring Data를 사용하고 있는 application이라면 `@EntityScan`과 `@EnableJpaRepositories` 주석은 명시적으로 지정되지 않은 경우 `@SpringBootApplication`에서 기본 패키지만 상속한다.
> 즉, 일단 scanBasePackageClasses 또는 scanBasePackages를 지정하면 명시적으로 구성된 패키지 스캔과 함께 `@EntityScan` alc `@EnableJpaRepositories`를 명시적으로 사용해야 할 수도 있다.

## 8. application.properties 파일 생성하기
`application.properties`의 라이브러리에 있는 서비스에 대한 메시지를 제공한다.
`src/main/resources/application.properties`
```
service.message=Hello, World
```

## 9. Application 테스트하기
Application을 시작해서 End-to-End 결과를 테스트한다. IDE에서 어플리케이션을 실행하거나 명령어를 사용할 수 있다.

어플리케이션이 실행되면 브라우저에서 `http://localhost:8080/`을 방문하여 응답에 `Hello, World`가 반영된 것을 확인할 수 있다.

##### Gradle
Gradle을 사용하는 경우 다음 명령(실제로 두 개의 명령이 차례로 실행된다.)은 라이브러리를 먼저 빌드하고 어플리케이션을 실행한다.
```
$ ./gradlew build && ./gradlew :application:bootRun
```

##### Maven
Maven을 사용하는 경우 다음 명령(실제로 두 개의 명령이 차례로 실행된다.)은 라이브러리르 먼저 빌드하고 어프리케이션을 실행한다.
```
$ ./mvnw install && ./mvnw spring-boot:run -pl application
```

## 참고 자료
- https://spring.io/guides/gs/multi-module/