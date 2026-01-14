# Spring Batch

## 개요

Spring Batch는 대용량 데이터 처리를 위한 Java 기반 프레임워크입니다. 대량의 데이터를 일괄 처리하는 배치 작업에 특화되어 있으며, 트랜잭션 관리, 재시도 기능, 모니터링 등의 기능을 제공합니다. 대표적인 사용 사례로는 데이터 마이그레이션, 정기 보고서 생성, 로그 파일 분석 등이 있습니다.

## 주요 개념

1. **Job**: 배치 작업의 단위로, 여러 개의 Step으로 구성됩니다.
2. **Step**: Job 내에서 수행되는 개별 작업 단위입니다. Reader, Processor, Writer로 구성됩니다.
3. **Reader**: 입력 데이터 소스(파일, DB 등)에서 데이터를 읽어들입니다.
4. **Processor**: 읽어들인 데이터를 가공하거나 필터링합니다.
5. **Writer**: 처리된 데이터를 출력 데이터 소스(파일, DB 등)에 쓰기 수행합니다.

## 사용 예시

다음은 Spring Batch를 활용한 간단한 파일 처리 예시입니다:

```java
@Configuration
public class BatchConfiguration {

    @Bean
    public Job importUserJob(JobRepository jobRepository,
                            Step step1) {
        return jobBuilderFactory.get("importUserJob")
                .flow(step1)
                .end()
                .build();
    }

    @Bean
    public Step step1(StepBuilderFactory stepBuilderFactory,
                     ItemReader<User> reader,
                     ItemProcessor<User, User> processor,
                     ItemWriter<User> writer) {
        return stepBuilderFactory.get("step1")
                .<User, User>chunk(10)
                .reader(reader)
                .processor(processor)
                .writer(writer)
                .build();
    }

    @Bean
    public FlatFileItemReader<User> reader() {
        return new FlatFileItemReaderBuilder<User>()
                .resource(new ClassPathResource("users.csv"))
                .delimited()
                .names(new String[]{"id", "name", "email"})
                .targetType(User.class)
                .build();
    }

    @Bean
    public ItemProcessor<User, User> processor() {
        return user -> {
            user.setName(user.getName().toUpperCase());
            return user;
        };
    }

    @Bean
    public JdbcBatchItemWriter<User> writer(DataSource dataSource) {
        return new JdbcBatchItemWriterBuilder<User>()
                .itemSqlParameterSourceProvider(new BeanPropertyItemSqlParameterSourceProvider<>())
                .sql("INSERT INTO users (id, name, email) VALUES (:id, :name, :email)")
                .dataSource(dataSource)
                .build();
    }
}
```

이 예제에서는 CSV 파일에서 사용자 데이터를 읽어들여, 이름을 대문자로 변환한 후 데이터베이스에 저장합니다.

## 주의사항

1. 대량 데이터 처리 시 메모리 사용량에 주의해야 합니다. Chunk 크기 조절, 데이터 분할 처리 등의 방법을 사용해야 합니다.
2. 장애 발생 시 재시작 기능을 적절히 활용해야 합니다. 중간에 실패한 경우 이어서 작업을 진행할 수 있도록 체크포인트를 설정해야 합니다.
3. 모니터링 및 관리 기능을 활용하여 배치 작업의 상태를 파악하고 문제를 신속하게 해결할 수 있어야 합니다.

## 참고자료

- [Spring Batch 공식 문서](https://spring.io/projects/spring-batch)
- [Spring Batch 레퍼런스](https://docs.spring.io/spring-batch/docs/current/reference/html/index.html)
- [Spring Batch 튜토리얼](https://www.baeldung.com/spring-batch-tutorial)