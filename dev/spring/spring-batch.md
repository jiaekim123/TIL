# Spring Batch

## 개요

Spring Batch는 대용량 데이터 처리를 위한 자바 기반의 오픈소스 프레임워크입니다. 복잡한 배치 처리 로직을 쉽게 구현할 수 있도록 도와주며, 작업 흐름 관리, 오류 처리, 모니터링 등의 기능을 제공합니다. 대규모 데이터 마이그레이션, 정기 보고서 생성 등 다양한 배치 처리 작업에 활용할 수 있습니다.

## 주요 개념

- **Job**: 하나의 배치 작업 단위를 의미합니다. 여러 개의 Step으로 구성됩니다.
- **Step**: 배치 작업의 각 단계를 나타냅니다. 주로 데이터 읽기, 데이터 처리, 데이터 쓰기 등의 작업으로 구성됩니다.
- **ItemReader**: 데이터 소스에서 데이터를 읽어오는 역할을 합니다.
- **ItemProcessor**: 읽어온 데이터를 가공하는 역할을 합니다.
- **ItemWriter**: 가공된 데이터를 목적지에 쓰는 역할을 합니다.

## 사용 예시

다음은 간단한 Spring Batch 사용 예시입니다. 고객 데이터를 CSV 파일에서 읽어와 데이터베이스에 저장하는 예제입니다.

```java
@Configuration
@EnableBatchProcessing
public class BatchConfiguration {

    @Bean
    public Job importUserJob(JobBuilderFactory jobBuilderFactory,
                             StepBuilderFactory stepBuilderFactory,
                             ItemReader<User> itemReader,
                             ItemProcessor<User, User> itemProcessor,
                             ItemWriter<User> itemWriter) {
        return jobBuilderFactory.get("importUserJob")
                .flow(step1(stepBuilderFactory, itemReader, itemProcessor, itemWriter))
                .end()
                .build();
    }

    @Bean
    public Step step1(StepBuilderFactory stepBuilderFactory,
                      ItemReader<User> itemReader,
                      ItemProcessor<User, User> itemProcessor,
                      ItemWriter<User> itemWriter) {
        return stepBuilderFactory.get("step1")
                .<User, User>chunk(10)
                .reader(itemReader)
                .processor(itemProcessor)
                .writer(itemWriter)
                .build();
    }

    @Bean
    public ItemReader<User> itemReader() {
        FlatFileItemReader<User> itemReader = new FlatFileItemReader<>();
        itemReader.setResource(new ClassPathResource("users.csv"));
        itemReader.setLineMapper(new DefaultLineMapper<>());
        return itemReader;
    }

    @Bean
    public ItemProcessor<User, User> itemProcessor() {
        return user -> {
            // 데이터 가공 로직 구현
            return user;
        };
    }

    @Bean
    public ItemWriter<User> itemWriter(DataSource dataSource) {
        JdbcBatchItemWriter<User> itemWriter = new JdbcBatchItemWriter<>();
        itemWriter.setDataSource(dataSource);
        itemWriter.setSql("INSERT INTO users (id, name, email) VALUES (:id, :name, :email)");
        itemWriter.setItemSqlParameterSourceProvider(new BeanPropertyItemSqlParameterSourceProvider<>());
        return itemWriter;
    }
}
```

이 예제에서는 `importUserJob`이라는 Job이 정의되어 있으며, 하나의 Step으로 구성되어 있습니다. 이 Step은 CSV 파일에서 사용자 데이터를 읽어와 가공한 후 데이터베이스에 저장하는 작업을 수행합니다.

## 주의사항

- 대용량 데이터 처리 시 메모리 관리에 주의해야 합니다. 적절한 청크 크기 설정과 데이터 분할 전략이 필요합니다.
- 작업 실패 시 재시작 및 오류 처리 전략을 잘 수립해야 합니다.
- 배치 작업의 성능 및 안정성을 모니터링하고 최적화하는 것이 중요합니다.

## 참고자료

- [Spring Batch 공식 문서](https://spring.io/projects/spring-batch)
- [Spring Batch 예제 프로젝트](https://github.com/spring-projects/spring-batch-examples)
- [Spring Batch 튜토리얼](https://www.baeldung.com/spring-batch-tutorial)