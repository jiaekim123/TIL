# kotlin let, with, run, apply, also

## 개요

Kotlin은 확장 함수라는 기능을 제공하여 코드의 가독성과 간결성을 높일 수 있는 다양한 함수형 프로그래밍 기법을 지원합니다. 이 문서에서는 Kotlin의 대표적인 확장 함수인 `let`, `with`, `run`, `apply`, `also`에 대해 알아보겠습니다.

## 주요 개념

1. **let**: 객체가 null이 아닌 경우에만 람다식을 실행하며, 람다식의 결과를 반환합니다.
2. **with**: 주어진 객체를 사용하여 여러 작업을 수행할 수 있는 람다식을 제공합니다. 람다식의 결과를 반환합니다.
3. **run**: 주어진 객체를 사용하여 여러 작업을 수행할 수 있는 람다식을 제공하며, 람다식의 결과를 반환합니다.
4. **apply**: 주어진 객체를 수정한 후 해당 객체를 반환합니다.
5. **also**: 주어진 객체를 사용하여 부수 효과를 발생시킨 후 해당 객체를 반환합니다.

## 사용 예시

```kotlin
// let
val str: String? = "Hello, Kotlin!"
str?.let {
    println("Length: ${it.length}")
    it.uppercase()
}

// with
val person = Person("John Doe", 30)
with(person) {
    println("Name: $name")
    println("Age: $age")
}

// run
val numbers = listOf(1, 2, 3, 4, 5)
val result = numbers.run {
    val sum = fold(0) { acc, i -> acc + i }
    sum * 2
}
println(result) // Output: 30

// apply
val mutableList = mutableListOf(1, 2, 3)
val newList = mutableList.apply {
    add(4)
    add(5)
}
println(mutableList) // Output: [1, 2, 3, 4, 5]
println(newList) // Output: [1, 2, 3, 4, 5]

// also
val person2 = Person("Jane Doe", 25)
val samePersonRef = person2.also {
    println("Name: ${it.name}, Age: ${it.age}")
}
println(person2 === samePersonRef) // Output: true
```

## 주의사항

1. `let`은 null 안전성을 제공하지만, 다른 확장 함수들은 그렇지 않으므로 null 처리에 주의해야 합니다.
2. `with`와 `run`은 유사한 기능을 제공하지만, 반환 값의 차이가 있습니다. `with`는 람다식의 마지막 표현식을 반환하고, `run`은 람다식 전체의 결과를 반환합니다.
3. `apply`와 `also`는 주어진 객체를 반환하지만, `apply`는 객체 자체를 반환하고 `also`는 동일한 객체의 참조를 반환합니다.

## 참고자료

- [Kotlin 공식 문서 - 확장 함수](https://kotlinlang.org/docs/extensions.html)
- [Kotlin 코틀린 강의 - 확장 함수](https://www.inflearn.com/course/the-kotlin-programming-language)
- [Medium - Kotlin의 let, with, run, apply, also 이해하기](https://medium.com/@limgyumin/%EC%BD%94%ED%8B%80%EB%A6%B0%EC%9D%98-let-with-run-apply-also-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-b9a6d56c9baf)