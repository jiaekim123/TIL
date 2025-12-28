# OpenSearch

Amazon OpenSearch Service 관련 학습 내용을 정리합니다.

## 개요

Amazon OpenSearch Service는 Apache 2.0 라이선스 기반의 오픈소스 검색 및 분석 엔진인 OpenSearch를 기반으로 한 AWS의 완전 관리형 서비스입니다.

### OpenSearch란?

- Elasticsearch의 오픈소스 포크로, 2021년 AWS가 공개
- 실시간 검색, 로그 분석, 애플리케이션 모니터링 등에 사용
- RESTful API를 통해 데이터 색인 및 검색 가능
- Kibana의 포크인 OpenSearch Dashboards를 함께 제공

### 주요 특징

1. **완전 관리형 서비스**: 클러스터 프로비저닝, 소프트웨어 설치 및 패치, 백업 자동화
2. **높은 확장성**: 페타바이트 규모의 데이터 처리 가능
3. **고가용성**: 멀티 AZ 배포 지원
4. **보안**: VPC 지원, 데이터 암호화, IAM 통합, Fine-grained access control
5. **통합성**: AWS 서비스와의 원활한 통합 (CloudWatch, Kinesis, S3 등)

## 주요 개념

### 1. 클러스터 (Cluster)

- OpenSearch 인스턴스들의 모음
- 하나 이상의 노드로 구성
- 클러스터 이름으로 식별

### 2. 노드 (Node)

- 클러스터의 단일 서버
- 데이터를 저장하고 검색 작업에 참여
- 노드 타입:
  - **Data nodes**: 데이터 저장 및 검색 처리
  - **Master nodes**: 클러스터 관리 (인덱스 생성/삭제, 노드 추적)
  - **UltraWarm nodes**: 읽기 전용 데이터를 저비용으로 저장

### 3. 인덱스 (Index)

- 유사한 특성을 가진 문서들의 모음
- RDB의 테이블과 유사한 개념
- 인덱스 이름은 소문자로 작성

### 4. 문서 (Document)

- 인덱싱할 수 있는 기본 정보 단위
- JSON 형식으로 표현
- RDB의 행(row)과 유사

### 5. 샤드 (Shard)

- 인덱스를 여러 조각으로 나눈 것
- **Primary shard**: 원본 데이터
- **Replica shard**: 복제본, 고가용성 및 검색 성능 향상

### 6. 도메인 (Domain)

- AWS OpenSearch Service에서 클러스터를 의미하는 용어
- 설정, 인스턴스 타입, 스토리지 등을 포함

## 아키텍처

### 기본 구조

```
Client Application
    ↓
Amazon OpenSearch Domain
├── Master Nodes (클러스터 관리)
├── Data Nodes (데이터 저장 및 검색)
└── UltraWarm Nodes (비용 효율적 스토리지)
    ↓
Amazon S3 (스냅샷 저장)
```

### 데이터 흐름

1. **데이터 수집**: Kinesis, Lambda, Logstash 등을 통해 데이터 수집
2. **인덱싱**: 문서를 분석하고 인덱스에 저장
3. **검색**: 쿼리 실행 및 결과 반환
4. **시각화**: OpenSearch Dashboards를 통한 데이터 시각화

## 주요 기능

### 1. 검색 기능

- **Full-text search**: 전체 텍스트 검색
- **Autocomplete**: 자동 완성
- **Fuzzy search**: 유사 검색 (오타 허용)
- **Geospatial search**: 위치 기반 검색

### 2. 분석 기능

- **집계 (Aggregations)**: 데이터 그룹화 및 통계 계산
- **실시간 분석**: 실시간 로그 및 메트릭 분석
- **Time-series 분석**: 시계열 데이터 분석

### 3. 보안 기능

- **VPC 지원**: VPC 내에 도메인 배포
- **암호화**: 저장 데이터 및 전송 중 데이터 암호화
- **Fine-grained access control**: 인덱스, 문서, 필드 레벨 액세스 제어
- **SAML 인증**: SSO 지원

### 4. 모니터링 및 알림

- **CloudWatch 통합**: 메트릭 및 로그 모니터링
- **알림 기능**: 쿼리 결과 기반 알림 설정
- **Anomaly detection**: 이상 탐지

## 사용 사례

### 1. 로그 분석

```
Application → CloudWatch Logs → Lambda → OpenSearch → Dashboards
```

- 애플리케이션 로그 수집 및 분석
- 실시간 로그 모니터링
- 보안 이벤트 추적

### 2. 전체 텍스트 검색

- 웹사이트 검색 기능
- 문서 관리 시스템
- 제품 카탈로그 검색

### 3. 애플리케이션 모니터링

- APM (Application Performance Monitoring)
- 인프라 메트릭 수집 및 시각화
- 실시간 대시보드

### 4. 비즈니스 분석

- 사용자 행동 분석
- 매출 분석
- 트렌드 파악

## 실습: OpenSearch 도메인 생성

### 1. AWS 콘솔에서 도메인 생성

1. AWS Management Console → OpenSearch Service
2. "Create domain" 클릭
3. 도메인 구성:
   - **Deployment type**: Development (테스트용) 또는 Production
   - **Version**: OpenSearch 버전 선택
   - **Domain name**: 고유한 도메인 이름 입력

### 2. 데이터 노드 구성

```
Instance type: t3.small.search (개발용)
Number of nodes: 3 (프로덕션 권장)
Storage type: EBS
EBS volume size: 10GB
```

### 3. 네트워크 설정

- **Public access** (테스트용) 또는 **VPC access** (프로덕션)
- VPC, 서브넷, 보안 그룹 설정

### 4. 액세스 정책 설정

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": "*"
      },
      "Action": "es:*",
      "Resource": "arn:aws:es:region:account-id:domain/domain-name/*",
      "Condition": {
        "IpAddress": {
          "aws:SourceIp": ["your-ip-address/32"]
        }
      }
    }
  ]
}
```

### 5. 데이터 인덱싱 예제

```bash
# 문서 추가
curl -XPOST "https://your-domain-endpoint/movies/_doc/1" \
  -H 'Content-Type: application/json' \
  -d '{
    "title": "The Matrix",
    "director": "Wachowski Sisters",
    "year": 1999
  }'

# 검색
curl -XGET "https://your-domain-endpoint/movies/_search?q=matrix"
```

### 6. Python을 사용한 인덱싱

```python
from opensearchpy import OpenSearch

# 연결 설정
host = 'your-domain-endpoint'
client = OpenSearch(
    hosts=[{'host': host, 'port': 443}],
    http_auth=('username', 'password'),
    use_ssl=True,
    verify_certs=True
)

# 문서 인덱싱
document = {
    'title': 'Inception',
    'director': 'Christopher Nolan',
    'year': 2010
}

response = client.index(
    index='movies',
    body=document,
    id='2'
)

# 검색
response = client.search(
    index='movies',
    body={'query': {'match': {'title': 'inception'}}}
)

print(response['hits']['hits'])
```

## 비용 최적화

### 1. UltraWarm 사용

- 자주 조회되지 않는 데이터를 저비용으로 저장
- Hot 스토리지 대비 90% 비용 절감

### 2. 적절한 인스턴스 타입 선택

- 개발/테스트: t3.small.search
- 프로덕션: r6g.large.search 이상

### 3. 예약 인스턴스

- 장기 사용 시 최대 50% 비용 절감

### 4. 데이터 보관 정책

- Index State Management (ISM)를 사용한 자동 삭제
- Cold 스토리지로 이동

## 모범 사례

### 1. 샤드 크기 관리

- Primary shard 크기: 10-50GB 권장
- 샤드가 너무 많으면 클러스터 성능 저하

### 2. 인덱스 설계

```json
{
  "mappings": {
    "properties": {
      "timestamp": {"type": "date"},
      "message": {"type": "text"},
      "level": {"type": "keyword"}
    }
  }
}
```

### 3. 모니터링 메트릭

- CPU 사용률: 80% 이하 유지
- JVM 메모리 압력: 75% 이하
- 디스크 공간: 25% 이상 여유 확보

### 4. 백업 설정

- 자동 스냅샷 활성화
- 수동 스냅샷을 S3에 저장

## 주의사항

### 1. 버전 업그레이드

- 인플레이스 업그레이드 지원
- Blue/Green 배포로 다운타임 최소화
- 메이저 버전은 한 번에 하나씩만 업그레이드

### 2. 보안

- 프로덕션 환경에서는 반드시 VPC 내 배포
- Fine-grained access control 활성화
- 정기적인 보안 패치 적용

### 3. 성능

- 적절한 샤드 수 설정
- 벌크 API 사용으로 인덱싱 성능 향상
- 검색 쿼리 최적화

### 4. 비용

- 불필요한 인덱스 삭제
- 인스턴스 타입 정기적 검토
- CloudWatch 알람 설정으로 비정상적인 사용량 감지

## 트러블슈팅

### 클러스터 상태가 Red인 경우

1. 노드 상태 확인
2. 샤드 할당 상태 확인
3. 디스크 공간 확인
4. 필요 시 노드 추가

### 검색 성능 저하

1. 인덱스 최적화 (force merge)
2. 캐시 설정 검토
3. 쿼리 튜닝
4. 샤드 재분배

## 참고 자료

- [AWS OpenSearch Service 공식 문서](https://docs.aws.amazon.com/opensearch-service/)
- [OpenSearch 공식 문서](https://opensearch.org/docs/latest/)
- [OpenSearch Dashboards 가이드](https://opensearch.org/docs/latest/dashboards/)
- [AWS OpenSearch Best Practices](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/bp.html)
- [OpenSearch Python Client](https://github.com/opensearch-project/opensearch-py)
