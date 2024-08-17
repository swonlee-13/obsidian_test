Elasticsearch는 Apache Lucene에 구축되어 배포된 검색 및 분석 엔진입니다. 다양한 언어를 지원하고 고성능에 스키마가 없는 JSON 문서로 Elasticsearch는 다양한 로그 분석과 검색 사용 사례에 최고의 선택이 되었습니다.
(*nginx 에도 적용 가능함*)

이 엔진은 로그를 저장하고, 검색하는데 탁월한 성능을 가지고 있습니다.

nginx 에 적용하는 법: https://www.elastic.co/kr/blog/how-to-monitor-nginx-web-servers-with-the-elastic-stack

### nginx configuration 예시

```nginx
http {
    log_format json_combined escape=json
    '{'
        # 기본 시간 정보 (ISO 8601 형식 추가)
        '"time_local":"$time_local",'
        '"@timestamp":"$time_iso8601",'
        # 클라이언트 정보
        '"remote_addr":"$remote_addr",'
        '"remote_user":"$remote_user",'
        # 요청 정보
        '"request":"$request",'
        # HTTP 메소드 (GET, POST 등)
        '"request_method":"$request_method",'
        # 요청 URI
        '"uri":"$uri",'

        # 응답 정보
        '"status": "$status",'
        '"body_bytes_sent":"$body_bytes_sent",'

        # 성능 메트릭
        '"request_time":"$request_time",'
        # 업스트림 서버 응답 시간
        '"upstream_response_time":"$upstream_response_time",'
        # 서버 정보
        '"server_name":"$server_name",'
        '"http_host":"$http_host",'

        # HTTP 헤더 정보
        '"http_referrer":"$http_referer",'
        '"http_user_agent":"$http_user_agent",'
        
	# 추가 정보 (필요에 따라 주석 해제 또는 추가)
	    # 프록시 뒤의 실제 클라이언트 IP
        # '"http_x_forwarded_for":"$http_x_forwarded_for",'
		# 사용된 SSL/TLS 프로토콜
        # '"ssl_protocol":"$ssl_protocol",'
		# 사용된 SSL/TLS 암호화 방식
        # '"ssl_cipher":"$ssl_cipher",'

    # 요청 처리에 대한 추가 정보
		# Gzip 압축률 (활성화된 경우)
        '"gzip_ratio":"$gzip_ratio",'
		# 연결 일련번호
        '"connection":"$connection",'
		# 현재 연결을 통한 요청 수
        '"connection_requests":"$connection_requests"'
    '}';

    access_log /var/log/nginx/access.log json_combined;
}
```


이와 같은 형식으로 로그 포맷을 만들어놓고, 이것을 elasticsesarch 에 보내 로그를 분석할 수 있게 하면 된다. 이후에 Logstash, Kibana 등을 거치면서 로그를 더 효과적으로 확인할 수 있.