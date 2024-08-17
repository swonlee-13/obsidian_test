무료 개방형 서버의 데이터 처리 파이프라인인 Logstash는 다양한 소스에서 데이터를 수집하여 변환한 후 자주 사용하는 저장소로 전달합니다.

Logstash는 실시간 파이프라이닝 기능을 갖춘 오픈 소스 데이터 수집 엔진이다. Logstash는 서로 다른 소스의 데이터를 동적으로 통합하고 원하는 목적지로 데이터를 정규화할 수 있습니다. 다양한 고급 다운스트림 분석 및 시각화 사용 사례를 위해 모든 데이터를 정리하고 민주화하십시오.

Logstash는 원래 로그 수집의 혁신을 주도했지만, 그 기능은 그 사용 사례를 훨씬 넘어선다. 모든 유형의 이벤트는 다양한 입력, 필터 및 출력 플러그인으로 풍부하고 변형될 수 있으며, 많은 네이티브 코덱이 수집 프로세스를 더욱 단순화합니다. Logstash는 더 많은 양과 다양한 데이터를 활용하여 통찰력을 가속화합니다.

1. 강력한 엘라스틱과 키바나와의 시너지와 함께 수평적으로 데이터 프로세스 파이프라인이 스케일업 할 수 있다.

2. 다른 인풋, 필터 그리고 아웃풋 플러그인들을 Mix, Match 할 수 있다.

3. 200개가 넘는 플러그인이 사용 가능하며, 게다가 새로 생성하고 기여할 수 있다

출처: https://www.elastic.co/kr/logstash
출처: https://www.elastic.co/guide/en/logstash/current/introduction.html

### Logstash configuration 예시
출처: https://soyoung-new-challenge.tistory.com/99

### claude 로 만들어본 logstash 예시

input 과 output 으로 구성되어있고, 이 중간에 데이터를 filtering 을 하는 형식이다.
``` nginx
input {
  file {
    path => "/var/log/nginx/access.log"
    start_position => "beginning"
    sincedb_path => "/var/lib/logstash/sincedb"
    codec => "json"
  }
}

filter {
  date {
    match => [ "@timestamp", "ISO8601" ]
    target => "@timestamp"
  }
  
  mutate {
    convert => {
      "status" => "integer"
      "body_bytes_sent" => "integer"
      "request_time" => "float"
      "upstream_response_time" => "float"
    }
  }
	#물리적 주소까지 관찰하고 싶을 때
  geoip {
    source => "remote_addr"
    target => "geoip"
    database => "/etc/logstash/GeoLite2-City.mmdb"
  }
  
  useragent {
    source => "http_user_agent"
    target => "user_agent"
  }
}

output {
  elasticsearch {
    hosts => ["localhost:9200"]
    index => "nginx-logs-%{+YYYY.MM.dd}"
  }
}
```