## ELK 스택이란 무엇인가요?

ELK 스택은 Elasticsearch, Logstash, Kibana의 세 가지 인기 있는 프로젝트로 구성된 스택을 의미하는 약어입니다. Elasticsearch라고도 불리는 ELK 스택은 사용자에게 모든 시스템과 애플리케이션에서 #log 를 집계하고 이를 분석하며 애플리케이션과 인프라 모니터링 시각화를 생성하고, 빠르게 문제를 해결하며 보안 분석할 수 있는 능력을 제공합니다.

ELK를 구성하는 엔진을 하나씩 사용해도 좋지만, 함께 사용했을 때 로그 관리적인 측면에서 매우 강력한 성능을 가집니다.

#### 개발기에는 중요하게 느껴지지 않을 수 있으나, 운영기에는 이와같은 로그프로그램 사용이 매우 중요하므로 프로젝트에 사용하지 않더라도 소개글과 예시, 설치법 등을 봐 두는 것은 도움이 된다고 생각합니다.

간단하게 이해한 대로 소개글만 정리해놓았으므로, 각각 출처에 들어가서 자세히 읽어보는 것을 추천드립니다.

![[ELK_stack.png]]
엔진 간의 워크플로우는 다음과 같으며, 각자의 엔진은 다음과 같은 역할을 합니다.

1. **Data Processing (Logstash)**
    - 서버 내의 로그, 웹, 메트릭 등 다양한 소스에서 데이터를 수집하여 입력
    - 데이터 변환 및 구조 구축
    - 데이터 출력 및 송신

2. **Storage (Elasticsearch)**
    - 데이터 저장
    - 데이터 분석
    - 데이터 관리

3. **Visualize (Kibana)**
    - Dashboard를 통한 데이터 탐색
    - 팀원들과 공유 및 협업하는데 사용 가능
    - 엑세스 제어 (Access Control) 사용 가능

출처: https://aws.amazon.com/ko/what-is/elk-stack/
출처: https://velog.io/@holidenty/ELK-ELK-Stack-이란-무엇일까


[[Elasticsearch]]
[[Logstash]]
[[Kibana]]