# Gradle



## Gradle

### 1. Gradle 이란?

* groovy를 기반으로 한 오픈소스 빌드 도구
* ant의 자유도와 maven과 같은 구조화된 build 프레임워크를 바탕으로 이전 빌드툴의 단점을 보완하여 개선된 서비스를 제공한다.
* Gradle 종속성 관리

![gradle-architecture](https://docs.gradle.org/current/userguide/img/dependency-management-resolution.png)

### 2. Gradle vs Maven

왜 사람들은 Maven에서 Gradle로 옮겨갔을까? Gradle이 Maven에 비해 좋은 점은 어떤 것이 있을까?

#### 2.1 Maven

pom.xml에 프로젝트에 필요한 모든 dependency를 설정해주면 직접 다운로드 받을 필요 없이 maven이 repository에서 필요한 모든 파일들을 해당 프로젝트로 불러와준다.

* 특징
  * Dependency를 관리하고, 표준화된 프로젝트를 제공한다.
  * XML, remote repository를 가져올 수 있다. jar나 class path를 다운로드 할 필요 없이 선언만으로 사용이 가능하다.
  * 라이브러리가 서로 종속될 경우 xml이 복잡해진다.
  * 계층적 데이터를 표현하기 좋지만, 조건부 상황을 표현하기 어렵다.
  * 편리하지만 맞춤화된 로직 실행이 어렵다.

#### 2.2 Gradle

빌드 자동화에 대한 요구가 증가하면서 maven과 ant의 단점을 보완해 나온 jvm 기반의 빌드 도구. java나 groovy를 통해 로직을 개발자의 의도에 따라 설계할 수 있다.

* 특징
  * 오픈소스 기반의 빌드 자동화 시스템으로 groovy 기반 DSL\(Domain-Specific Language\)로 작성한다.
  * maven 등의 기본 저장소 인프라나 pom.xml 파일에 대한 migration 편이성을 제공한다.
  * 멀티 프로젝트 빌드를 지원한다.
  * 의존성 관리의 다양한 방법을 지원한다.

#### 2.3 Gradle의 장점

* 유연성
  * Gradle은 빌드 스크립트가 코드이기도 하지만 확장 가능한 방식으로 모델링 되었다. \(이것이 Android용 빌드 도구로 지정된 이유이기도 함.\)
  * Gradle과 Maven은 모두 구성에 대한 규칙을 제공한다. 
    * 그러나 Maven은 매우 엄격한 모델을 제공하여 빌드를 더 쉽게 이해할 수 있지만 많은 자동화 문제에 적합하지 않다.
    * 반면에 Gradle은 권한이 있고 책임이 있는 사용자를 염두에 두고 구축되어 자동화 문제에 보다 적합하다.
* 성능
  * Gradle과 Maven은 모두 일종의 병렬 프로젝트 구축 및 병렬 종속성 해결을 사용한다.
  * 단, Gradle은 아래 3가지 특징에 의해 대부분의 시나리오에서 Maven보다 빌드 성능이 2배 이상 뛰어나다.
    * Incrementality\(증분성\) - Gradle은 작업의 입출력을 추적하여 필요한 것만 실행하고 가능한 경우 변경된 파일만 처리하여 작업을 최소화한다.
    * Build Cache - 같은 기기에 동일한 입력이 있을 경우 Gradle 빌드 캐시의 결과를 재사용한다.
    * Gradle Daemon - 빌드 정보를 메모리에 올려 유지하는 수명이 긴 Daemon process를 사용한다.
* 종속성 관리
  * Maven과 Gradle은 모두 구성 가능한 레포지토리에서 dependency를 해결하는 기본 기능을 제공하며 둘다 종속성을 로컬로 캐시하고 병렬로 다운로드 할 수 있다.
  * Maven은 버전에 의해서 종속성을 재정의할 수 있다. Gradle은 원치 않는 종속성을 처리할 수 있는 사용자 지정 가능한 종속성 선택 및 대체 규칙을 제공한다. 이를 통해 Gradle은 여러 소스 프로젝트를 함께 빌드하여 [복합 빌드](https://docs.gradle.org/current/userguide/composite_builds.html)를 만들 수 있다.
  * Maven에는 내장된 종속성 범위가 거의 없다. \(예를 들어 단위 테스트와 통합 테스트 사이의 분리가 없다.\) Gradle에서는 custom dependency scopes\(사용자 지정 종속성 범위\)를 허용한다.

### 3. Build 설정 파일

| 디렉터리 및 파일 | 설명 |
| :--- | :--- |
| `/settings.gradle` | 프로젝트 구성 설정 \(싱글 프로젝트라면 생략 가능\) |
| `/build.gradle` | 프로젝트의 빌드 정보를 구성 |
| `/gradlew`\(xnix\), `/gradlew.bat`\(windows\) | gradle 명령 파일 |
| `/.gradle`, `/gradle` | gradle 버전별 엔진 및 설정 파일 |
| `/build` | 빌드될 output이 위치하는 디렉토리 |

### 4. Gradle 사용법

build.gradle 파일에 빌드 정보를 구성한다.

#### 4.1 repository

**레포지토리 설정**

```groovy
repositories {
    mavenCentral()
    mavenLocal()
    maven { 
      url "http://repo.mycompany.com/maven"
      credentials {
        username "user"
        password "password"
      }
    }
}
```

* mavenCentral\(\): maven 중앙 저장소
* mavenLocal\(\): `{USER_HOME}/.m2/settings.xml` 파일에 저장된 로컬 Maven 캐시 위치 저장소
  * 일반적으로는 gradle에서 mavenLocal\(\)로 로컬 캐시 저장소를 통해 레포지토리를 불러오는 것을 추천하고 있지 않다. \(setting.xml 파일을 추적하기 어렵고 파일이 바뀔 수 있다는 점은 감안하여 gradle에서 로컬 레포에 대한 캐싱을 수행하지 않아 빌드가 느려질 수 있다.\)
  * 단, 다음과 같은 경우에는 mavenLocal\(\)을 사용해야할 수 있다. 예를 들어 프로젝트 A는 maven으로 빌드되고, 프로젝트 B는 gradle로 빌드되지만 개발 중 아티팩트를 공유해야 하는 경우 등

    > 레포지토리 계정정보가 build.gradle 파일에 포함되면 보안상 좋지 않으니 gradle.properties에서 관리하고, mavenLocal\(\)을 지우는게 좋을 듯.
* maven { url "[http://repo.mycompany.com/maven](http://repo.mycompany.com/maven)" }: maven 원격 저장소 \(사내 nexus maven repository 등\)

**레포지토리 중앙 집중화**

```groovy
dependencyResolutionManagement {
    repositories {
        mavenCentral()
    }
}
```

allProjects 블록을 선언하는 대신 위와 같이 모든 하위 프로젝트에서 사용하는 저장소를 선언할 수 있다.

**레포지토리 Credential**

```groovy
repositories {
    maven {
        url = "${url}"
        credentials(PasswordCredentials)
        // url = uri(<<some repository url>>)
    }
}
```

PasswordCredentials를 사용하면 `gradle.properties` 파일에 `mySecureRepositoryUsername`, `mySecureRepositoryPassword` 값을 읽어온다.

#### 종속성 선언

**Dependency configuration**

**Plugin**

미리 구성해 놓은 task들의 그룹으로, 특정 빌드 과정에서 필요한 기본정보를 포함하고, 필요에 따라 정보를 수정하여 목적에 맞게 사용할 수 있다.

```groovy
plugins {
    id 'java'
    id 'war'
    id 'org.springframework.boot' version '2.1.6.RELEASE'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    ...
}
```

* java plugin
  * 프로젝트에 아래와 같은 작업을 추가한다.
    * compileJava, processResource, classes, jar, javadoc, test, clean, build 등
  * 프로젝트에 아래와 같은 종속성 구성을 추가한다.
    * implementation, compileOnly, annotationProcessor, runtimeOnly, testImplementation 등

**Dependency**

* `implementation`: 구현 전용 종속성
* `compileOnly`: 런타임에 사용되지 않는 컴파일 시간 전용 종속성
* `annotationProcessor`: 컴파일 중에 사용되는 annotation 프로세서
* `testImplementation`: 테스트에 대한 구현 전용 종속성
* `runtimeOnly`: 런타임 전용 종속성
* `testCompileOnly`: 테스트 컴파일을 위한 추가 종속성이며 런타임에 사용되지 않음

**dependencyManagement**

```groovy
dependencyManagement {
    imports {
        mavenBom "org.springframework.cloud:spring-cloud-dependencies:${ext['spring-cloud.version']}"
        mavenBom "de.codecentric:spring-boot-admin-dependencies:${ext['spring-boot-admin.version']}"
    }
}
```

* mavenBom: 프로젝트에 추가한 dependency의 버전을 알아서 관리해주기 때문에 개발자는 버전을 신경쓰지 않고 프로젝트에 dependency를 추가할 수 있음.

**ext 변수**

```groovy
ext {
    set('spring-cloud.version', 'Greenwich.SR2')
    set('swagger2.version', '2.9.2')
    set('lombok.version', '1.18.20')
    ...
}
```

버전 등을 변수로 빼서 관리할 수 있음. \(그 외에 공통적으로 사용되는 것들을 변수로 빼서 관리할 수 있다.\)

**Dependency Configuration**

Gradle 프로젝트에 대해 선언된 모든 종속성은 특정 범위에 적용된다. 예를 들어 일부 종속성은 소스 코드를 컴파일하는 데 사용해야 하지만 다른 종속성은 런타임에만 사용할 수 있어야 한다. Gradle은 Configuration의 도움으로 종속성의 범위를 나타내고 모든 구성은 고유한 이름으로 식별할 수 있다.

![&#xD2B9;&#xC815; &#xBAA9;&#xC801;&#xC744; &#xC704;&#xD574; &#xC120;&#xC5B8;&#xB41C; &#xC885;&#xC18D;&#xC131;&#xC744; &#xC0AC;&#xC6A9;&#xD558;&#xB294; &#xAD6C;&#xC131;](https://docs.gradle.org/current/userguide/img/dependency-management-configurations.png)

**configurations**

`Configuration.extentForm(org.gradle.api.artifacts.Configuration[])` 메서드를 호출하여 상속 계층을 형성할 수 있습니다. configuration은 빌드 스크립트나 plugin의 정의와 관계 없이 다른 구성을 확장할 수 있다.

**예시**

```groovy
configurations {
    smokeTest.extendsFrom testImplementation
}

dependencies {
    testImplementation 'junit:junit:4.13'
    smokeTest 'org.apache.httpcomponents:httpclient:4.5.5'
}
```

위와 같이 testImplementation을 상속받은 smokeTest라는 이름의 configuration을 정의하여 테스트 프레임워크 의존성을 재사용할 수 있다.

**사용자 정의 구성**

목적에 따라 필요한 종속성의 범위를 분리하기 위해 사용자 지정 구성을 할 수 있다.

```groovy
configurations {
    jasper
}

repositories {
    mavenCentral()
}

dependencies {
    jasper 'org.apache.tomcat.embed:tomcat-embed-jasper:9.0.2'
}

tasks.register('preCompileJsps') {
    doLast {
        ant.taskdef(classname: 'org.apache.jasper.JspC',
                    name: 'jasper',
                    classpath: configurations.jasper.asPath)
        ant.jasper(validateXml: false,
                   uriroot: file('src/main/webapp'),
                   outputDir: file("$buildDir/compiled-jsps"))
    }
}
```

예를 들어, 위와 같이 `jasper`라는 Configuration을 구성해, Jsp파일을 컴파일하기 전에 해야할 task를 정의하고 싶을 경우 위와 같이 사용자 지정 구성을 사용할 수 있다. 이런 구성은 이름을 가질 수 있고, 서로 확장할 수 있다.

### 참고자료

* [https://docs.gradle.org/current/userguide/tutorial\_using\_tasks.html](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html)
* [https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.6:dep:build\_tool:gradle](https://www.egovframe.go.kr/wiki/doku.php?id=egovframework:dev3.6:dep:build_tool:gradle)
* [https://docs.gradle.org/current/userguide/multi\_project\_builds.html](https://docs.gradle.org/current/userguide/multi_project_builds.html)
* [https://codecrafting.tistory.com/1](https://codecrafting.tistory.com/1)
* [https://gradle.org/maven-vs-gradle/](https://gradle.org/maven-vs-gradle/)
* [https://docs.gradle.org/current/userguide/dependency\_management.html\#sub:configurations](https://docs.gradle.org/current/userguide/dependency_management.html#sub:configurations)
* [https://docs.gradle.org/current/userguide/java\_plugin.html\#sec:java\_plugin\_and\_dependency\_management](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_plugin_and_dependency_management)

