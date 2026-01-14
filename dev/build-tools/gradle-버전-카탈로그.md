# Gradle 버전 카탈로그

## 개요

Gradle은 프로젝트 빌드 자동화 도구로, 프로젝트 구조와 의존성 관리를 용이하게 해줍니다. Gradle 버전 카탈로그는 Gradle의 버전 정보를 관리하는 기능으로, 프로젝트에 필요한 Gradle 버전을 손쉽게 지정할 수 있습니다.

## 주요 개념

1. **버전 카탈로그**: Gradle 버전 정보를 관리하는 기능으로, 프로젝트에서 사용할 Gradle 버전을 지정할 수 있습니다.
2. **버전 잠금**: 버전 카탈로그를 사용하면 프로젝트에서 사용하는 Gradle 버전을 고정할 수 있어, 빌드 환경의 일관성을 유지할 수 있습니다.
3. **의존성 관리**: Gradle 버전 카탈로그를 사용하면 프로젝트의 의존성 관리도 용이해집니다. 버전 충돌 등의 문제를 사전에 방지할 수 있습니다.

## 사용 예시

Gradle 버전 카탈로그를 사용하는 방법은 다음과 같습니다:

```kotlin
// settings.gradle.kts
dependencyResolutionManagement {
    versionCatalogs {
        create("libs") {
            version("gradle", "7.5.1")
            version("junit", "5.9.0")
            version("mockito", "4.8.0")
        }
    }
}

// build.gradle.kts
dependencies {
    implementation("org.junit.jupiter:junit-jupiter:${libs.versions.junit.get()}")
    testImplementation("org.mockito:mockito-core:${libs.versions.mockito.get()}")
}
```

위 예제에서는 `settings.gradle.kts` 파일에서 Gradle 버전 카탈로그를 정의하고, `build.gradle.kts` 파일에서 해당 버전 정보를 사용하고 있습니다.

## 주의사항

1. Gradle 버전 카탈로그는 Gradle 6.0 이상 버전에서만 지원됩니다.
2. 버전 카탈로그 정의 시 버전 정보는 정확히 기재해야 합니다. 잘못된 버전 정보는 빌드 오류를 발생시킬 수 있습니다.
3. 버전 카탈로그를 사용하더라도 의존성 관리는 여전히 중요합니다. 버전 호환성을 꼭 확인해야 합니다.

## 참고자료

- [Gradle 공식 문서 - 버전 카탈로그](https://docs.gradle.org/current/userguide/platforms.html#sub:version-catalog)
- [Gradle 버전 관리 전략](https://engineering.linecorp.com/ko/blog/gradle-version-management/)
- [Gradle 의존성 관리 모범 사례](https://www.gradle.org/blog/better-dependency-management/)