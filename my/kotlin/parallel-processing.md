# Kotlin/Java 병렬처리

여러 작업을 동시에 실행하는 병렬처리 방법을 정리합니다.

## 개요

병렬처리는 여러 작업을 동시에 수행하여 프로그램의 성능을 향상시키는 기법입니다. Kotlin/Java에서는 여러 방법으로 병렬처리를 구현할 수 있습니다.

## 1. Thread를 사용한 병렬처리

### 기본 Thread 사용

```kotlin
fun main() {
    val threads = mutableListOf<Thread>()

    // 10개의 스레드 생성
    for (i in 1..10) {
        val thread = Thread {
            println("Thread $i: ${Thread.currentThread().name} 시작")
            Thread.sleep(1000) // 작업 시뮬레이션
            println("Thread $i: ${Thread.currentThread().name} 완료")
        }
        threads.add(thread)
        thread.start()
    }

    // 모든 스레드가 완료될 때까지 대기
    threads.forEach { it.join() }
    println("모든 작업 완료!")
}
```

### Java 스타일

```java
public class ThreadExample {
    public static void main(String[] args) throws InterruptedException {
        List<Thread> threads = new ArrayList<>();

        for (int i = 1; i <= 10; i++) {
            final int threadNum = i;
            Thread thread = new Thread(() -> {
                System.out.println("Thread " + threadNum + ": 시작");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("Thread " + threadNum + ": 완료");
            });

            threads.add(thread);
            thread.start();
        }

        // 모든 스레드 완료 대기
        for (Thread thread : threads) {
            thread.join();
        }

        System.out.println("모든 작업 완료!");
    }
}
```

## 2. ExecutorService를 사용한 병렬처리 (권장)

### 고정 크기 스레드 풀

```kotlin
import java.util.concurrent.Executors
import java.util.concurrent.TimeUnit

fun main() {
    // 10개의 스레드를 가진 스레드 풀 생성
    val executor = Executors.newFixedThreadPool(10)

    // 20개의 작업 제출 (10개씩 2번 실행됨)
    for (i in 1..20) {
        executor.submit {
            println("작업 $i: ${Thread.currentThread().name} 시작")
            Thread.sleep(1000)
            println("작업 $i: ${Thread.currentThread().name} 완료")
        }
    }

    // 새로운 작업 받지 않음
    executor.shutdown()

    // 모든 작업이 완료될 때까지 대기 (최대 1분)
    executor.awaitTermination(1, TimeUnit.MINUTES)

    println("모든 작업 완료!")
}
```

### 결과값을 반환하는 병렬처리 (Callable)

```kotlin
import java.util.concurrent.Callable
import java.util.concurrent.Executors

fun main() {
    val executor = Executors.newFixedThreadPool(10)
    val futures = mutableListOf<java.util.concurrent.Future<Int>>()

    // 10개의 계산 작업 제출
    for (i in 1..10) {
        val future = executor.submit(Callable {
            println("계산 $i 시작")
            Thread.sleep(500)
            val result = i * i
            println("계산 $i 완료: $result")
            result
        })
        futures.add(future)
    }

    // 모든 결과 수집
    val results = futures.map { it.get() }
    println("결과: $results")
    println("합계: ${results.sum()}")

    executor.shutdown()
}
```

### Java 8+ CompletableFuture 사용

```kotlin
import java.util.concurrent.CompletableFuture

fun main() {
    val futures = (1..10).map { i ->
        CompletableFuture.supplyAsync {
            println("작업 $i 시작: ${Thread.currentThread().name}")
            Thread.sleep(1000)
            i * 2
        }
    }

    // 모든 작업 완료 대기
    val results = futures.map { it.join() }
    println("결과: $results")
}
```

## 3. Kotlin Coroutines를 사용한 병렬처리 (Kotlin 권장)

### 기본 Coroutines

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    // 10개의 코루틴을 동시에 실행
    val jobs = List(10) { i ->
        launch {
            println("코루틴 $i 시작: ${Thread.currentThread().name}")
            delay(1000)
            println("코루틴 $i 완료")
        }
    }

    // 모든 코루틴 완료 대기
    jobs.forEach { it.join() }
    println("모든 작업 완료!")
}
```

### async/await로 결과 반환

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    val deferreds = List(10) { i ->
        async {
            println("계산 $i 시작")
            delay(500)
            i * i
        }
    }

    val results = deferreds.awaitAll()
    println("결과: $results")
    println("합계: ${results.sum()}")
}
```

### Dispatcher로 스레드 풀 제어

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking {
    // 10개의 스레드로 제한된 디스패처
    val dispatcher = Executors.newFixedThreadPool(10).asCoroutineDispatcher()

    val jobs = List(20) { i ->
        launch(dispatcher) {
            println("작업 $i: ${Thread.currentThread().name}")
            delay(1000)
            println("작업 $i 완료")
        }
    }

    jobs.forEach { it.join() }
    dispatcher.close()
}
```

## 4. Parallel Stream (Java 8+)

```kotlin
fun main() {
    val results = (1..10)
        .toList()
        .parallelStream()
        .map { i ->
            println("작업 $i: ${Thread.currentThread().name}")
            Thread.sleep(1000)
            i * i
        }
        .toList()

    println("결과: $results")
}
```

## 실전 예제: 웹 크롤링

### ExecutorService 사용

```kotlin
import java.net.URL
import java.util.concurrent.Executors
import java.util.concurrent.TimeUnit

fun fetchUrl(url: String): String {
    println("$url 다운로드 시작: ${Thread.currentThread().name}")
    Thread.sleep(1000) // 네트워크 요청 시뮬레이션
    return "$url 의 내용"
}

fun main() {
    val urls = listOf(
        "https://example.com/page1",
        "https://example.com/page2",
        "https://example.com/page3",
        "https://example.com/page4",
        "https://example.com/page5",
        "https://example.com/page6",
        "https://example.com/page7",
        "https://example.com/page8",
        "https://example.com/page9",
        "https://example.com/page10"
    )

    val executor = Executors.newFixedThreadPool(10)
    val startTime = System.currentTimeMillis()

    val futures = urls.map { url ->
        executor.submit<String> {
            fetchUrl(url)
        }
    }

    val results = futures.map { it.get() }

    executor.shutdown()
    executor.awaitTermination(1, TimeUnit.MINUTES)

    val endTime = System.currentTimeMillis()

    println("\n결과:")
    results.forEach { println(it) }
    println("\n소요 시간: ${endTime - startTime}ms")
}
```

### Coroutines 사용

```kotlin
import kotlinx.coroutines.*

suspend fun fetchUrlAsync(url: String): String {
    println("$url 다운로드 시작: ${Thread.currentThread().name}")
    delay(1000)
    return "$url 의 내용"
}

fun main() = runBlocking {
    val urls = listOf(
        "https://example.com/page1",
        "https://example.com/page2",
        "https://example.com/page3",
        "https://example.com/page4",
        "https://example.com/page5",
        "https://example.com/page6",
        "https://example.com/page7",
        "https://example.com/page8",
        "https://example.com/page9",
        "https://example.com/page10"
    )

    val startTime = System.currentTimeMillis()

    val results = urls.map { url ->
        async {
            fetchUrlAsync(url)
        }
    }.awaitAll()

    val endTime = System.currentTimeMillis()

    println("\n결과:")
    results.forEach { println(it) }
    println("\n소요 시간: ${endTime - startTime}ms")
}
```

## 비교: 각 방법의 장단점

| 방법 | 장점 | 단점 | 추천 상황 |
|------|------|------|----------|
| **Thread** | 간단함, 직관적 | 관리 어려움, 리소스 낭비 | 간단한 학습용 |
| **ExecutorService** | 스레드 풀 관리, 안정적 | 보일러플레이트 코드 | Java 프로젝트 |
| **CompletableFuture** | 함수형 스타일, 체이닝 가능 | 복잡한 에러 처리 | 비동기 파이프라인 |
| **Coroutines** | 가볍고 효율적, 읽기 쉬움 | Kotlin 전용 | Kotlin 프로젝트 (최고 권장) |
| **Parallel Stream** | 간결함 | 제어 어려움, CPU 집약적 작업만 | 간단한 데이터 처리 |

## 주의사항

### 1. 공유 자원 접근

```kotlin
import java.util.concurrent.Executors
import java.util.concurrent.atomic.AtomicInteger

fun main() {
    val executor = Executors.newFixedThreadPool(10)

    // ❌ 잘못된 방법 - Race Condition
    var counter = 0

    // ✅ 올바른 방법 - AtomicInteger 사용
    val atomicCounter = AtomicInteger(0)

    repeat(100) {
        executor.submit {
            counter++ // 스레드 안전하지 않음!
            atomicCounter.incrementAndGet() // 스레드 안전함
        }
    }

    executor.shutdown()
    executor.awaitTermination(1, java.util.concurrent.TimeUnit.SECONDS)

    println("일반 카운터: $counter") // 100이 아닐 수 있음
    println("Atomic 카운터: ${atomicCounter.get()}") // 항상 100
}
```

### 2. synchronized 사용

```kotlin
class SafeCounter {
    private var count = 0

    @Synchronized
    fun increment() {
        count++
    }

    @Synchronized
    fun get(): Int = count
}

fun main() {
    val executor = Executors.newFixedThreadPool(10)
    val counter = SafeCounter()

    repeat(100) {
        executor.submit {
            counter.increment()
        }
    }

    executor.shutdown()
    executor.awaitTermination(1, java.util.concurrent.TimeUnit.SECONDS)

    println("카운터: ${counter.get()}") // 항상 100
}
```

### 3. 데드락 방지

```kotlin
// ❌ 데드락 발생 가능
fun transferMoney(from: Account, to: Account, amount: Int) {
    synchronized(from) {
        synchronized(to) {
            from.balance -= amount
            to.balance += amount
        }
    }
}

// ✅ 데드락 방지 - 항상 같은 순서로 락 획득
fun transferMoneySafe(from: Account, to: Account, amount: Int) {
    val (first, second) = if (from.id < to.id) {
        from to to
    } else {
        to to from
    }

    synchronized(first) {
        synchronized(second) {
            from.balance -= amount
            to.balance += amount
        }
    }
}
```

## 성능 측정 예제

```kotlin
import kotlinx.coroutines.*
import java.util.concurrent.Executors
import kotlin.system.measureTimeMillis

fun cpuIntensiveTask(n: Int): Long {
    var sum = 0L
    for (i in 1..n) {
        sum += i
    }
    return sum
}

fun main() {
    val taskCount = 100
    val workSize = 10_000_000

    // 1. 순차 처리
    val sequentialTime = measureTimeMillis {
        repeat(taskCount) {
            cpuIntensiveTask(workSize)
        }
    }
    println("순차 처리: ${sequentialTime}ms")

    // 2. ExecutorService
    val executorTime = measureTimeMillis {
        val executor = Executors.newFixedThreadPool(10)
        val futures = List(taskCount) {
            executor.submit<Long> { cpuIntensiveTask(workSize) }
        }
        futures.forEach { it.get() }
        executor.shutdown()
    }
    println("ExecutorService (10 스레드): ${executorTime}ms")

    // 3. Coroutines
    val coroutineTime = measureTimeMillis {
        runBlocking {
            List(taskCount) {
                async(Dispatchers.Default) {
                    cpuIntensiveTask(workSize)
                }
            }.awaitAll()
        }
    }
    println("Coroutines: ${coroutineTime}ms")

    println("\n성능 개선:")
    println("ExecutorService: ${sequentialTime / executorTime.toDouble()}배 빠름")
    println("Coroutines: ${sequentialTime / coroutineTime.toDouble()}배 빠름")
}
```

## 추천 사항

1. **Kotlin 프로젝트**: Coroutines 사용 (가장 효율적이고 읽기 쉬움)
2. **Java 프로젝트**: ExecutorService 사용
3. **간단한 병렬 처리**: Parallel Stream 고려
4. **항상 Thread를 직접 생성하는 것보다 스레드 풀 사용**
5. **공유 자원 접근 시 동기화 필수**

## 참고 자료

- [Kotlin Coroutines 공식 문서](https://kotlinlang.org/docs/coroutines-overview.html)
- [Java Concurrency in Practice](https://jcip.net/)
- [ExecutorService 가이드](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html)
