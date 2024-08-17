# 개요
ModSecurity 는 WAF를 구현한 소프트웨어입니다.

ModSecurity는 Apache, Nginx 및 IIS와 같은 다양한 웹 서버에서 지원하는 오픈 소스 웹 애플리케이션 방화벽입니다. ModSecurity는 서버 구성을 위해 사용자 정의 가능한 규칙 세트를 제공합니다.

`mod_security-crs` 패키지에는 웹 사이트 간 스크립팅, 잘못된 사용자 에이전트, SQL 인젝션, 트로이 목마, 세션 hijacking 및 기타 악용에 대한 규칙이 있는 핵심 규칙 세트(CRS)가 포함되어 있습니다.


출처: https://docs.redhat.com/ko/documentation/red_hat_enterprise_linux/9/html/deploying_web_servers_and_reverse_proxies/assembly_securing-web-applications-on-a-web-server-using-modsecurity_setting-apache-http-server

## nginx 에 ModSecurity 를 적용하는 방법

https://www.linode.com/docs/guides/securing-nginx-with-modsecurity/

nginx 공식 사이트 링크
https://docs.nginx.com/nginx-waf/admin-guide/nginx-plus-modsecurity-waf-installation-logging/