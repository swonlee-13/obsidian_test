---
aliases:
  - WAF | ModSecurity module
---
# 이 모듈은 메이저 모듈입니다.

강화된 비밀 관리를 위해서 다음을 구현합니다.

[[Hardened Configuration]]
[[HashiCorp Vault]]
[[WAF]] 및 [[ModSecurity]]

이 메이저 모듈의 목적은 몇몇 주요 컴포넌트를 이용해 프로젝트의 보안 하부구조(infrastructure) 를 강화하는 것입니다. 주요 기능과 목적은 다음을 포함합니다: 

- WAF 와 ModSecurity 설정을 엄격하고, 안전하게 구성 및 배포해서 웹 기반의 공격으로부터 방어합니다.

- HashiCorp Vault를 추가해서 API 키, 인증서, 환경 변수 등 민감한 정보를 안전하게 관리하고 저장하여 이러한 비밀들이 적절하게 암호화되고 격리되도록 합니다.

### 이 메이저 모듈은 프로젝트의 보안 인프라스트럭쳐를 강화하는 데 목적을 둡니다.