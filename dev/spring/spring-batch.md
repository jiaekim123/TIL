# Spring Batch

## 개요

Spring Batch는 대용량 데이터 처리를 위한 경량화된 프레임워크입니다. 대량의 데이터를 효율적으로 처리하고 관리할 수 있는 기능을 제공합니다. 배치 작업의 시작, 종료, 재시작, 로깅, 모니터링 등의 기능을 쉽게 구현할 수 있습니다.

## 주요 개념

1. **Job**: 배치 작업의 단위로, 하나 이상의 Step으로 구성됩니다.
2. **Step**: Job의 일부로, 실제 데이터 처리 로직이 구현됩니다.
3. **ItemReader**: 입력 데이터를 읽어오는 역할을 합니다.
4. **ItemProcessor**: 입력 데이터를 가공하는 역할을 합니다.
5. **ItemWriter**: 가공된 데이터를 출력하는 역할을 합니다.
6. **JobRepository**: Job, Step 실행 정보를 저장하고 관리합니다.
7. **JobLauncher**: Job을 실행하는 역할을 합니다.

## 사용 예시

다음은 간단한 Spring Batch 애플리케이션의 예제입니다. 이 예제에서는 CSV 파일에서 데이터를 읽어와 데이터베이스에 저장하는 작업을 수행합니다.

```java
@Configuration
@EnableBatchProcessing
public class BatchConfig {

    @Bean
    public Job csvToDbJob(JobBuilderFactory jobBuilderFactory,
                          StepBuilderFactory stepBuilderFactory,
                          ItemReader<User> itemReader,
                          ItemProcessor<User, User> itemProcessor,
                          ItemWriter<User> itemWriter) {
        return jobBuilderFactory.get("csvToDbJob")
                .start(step1(stepBuilderFactory, itemReader, itemProcessor, itemWriter))
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
        itemReader.setLineMapper(new DefaultLineMapper<>((tokenizer) -> {
            User user = new User();
            user.setId(Long.parseLong(tokenizer.nextToken()));
            user.setName(tokenizer.nextToken());
            return user;
        }));
        return itemReader;
    }

    @Bean
    public ItemProcessor<User, User> itemProcessor() {
        return user -> {
            user.setName(user.getName().toUpperCase());
            return user;
        };
    }

    @Bean
    public ItemWriter<User> itemWriter(DataSource dataSource) {
        JdbcBatchItemWriter<User> itemWriter = new JdbcBatchItemWriter<>();
        itemWriter.setDataSource(dataSource);
        itemWriter.setSql("INSERT INTO users (id, name) VALUES (:id, :name)");
        itemWriter.setItemSqlParameterSourceProvider(new BeanPropertyItemSqlParameterSourceProvider<>());
        return itemWriter;
    }
}
```

## 주의사항

1. Spring Batch는 대량의 데이터 처리를 위해 설계되었기 때문에, 작은 규모의 데이터 처리에는 적합하지 않을 수 있습니다.
2. 배치 작업의 실패 처리와 재시작 기능을 잘 설계해야 합니다.
3. 배치 작업의 성능을 높이기 위해 청크 사이즈, 스레드 수, 메모리 사용량 등을 적절하게 조정해야 합니다.

## 참고자료

- [Spring Batch 공식 문서](https://spring.io/projects/spring-batch)
- [Spring Batch 튜토리얼](https://www.baeldung.com/spring-batch-tutorial)
- [Spring Batch 예제](https://github.com/spring-projects/spring-batch-examples)