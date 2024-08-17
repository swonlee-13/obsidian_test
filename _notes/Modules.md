---
tags: module
---
자 이제 과제의 25퍼센트는 따라왔습니다!ㅋ 축하해여~ㅋ
### 기능적인 기본 웹사이트가 있는 상태에서, 다음 단계는 더 많은 발전을 위한 모듈을 선택할 차례입니다.
100퍼센트 과제 달성을 위해서 최소 7개의 모듈이 필요합니다. 몇몇 모듈을 선택하면 기본 웹사이트를 수정해야 할 수 있으므로, 각각의 모듈을 자세하게 살펴야합니다. 그러므로 우리는 전체 과제를 심도있게 살펴보는 것을 추천합니다.

```
🚨 라이브러리나 프레임워크를 사용하는 것은 절대 금지입니다.
각 모듈마다 사용할 수 있는 서드파티 소프트웨어는 정확하게 명시되어 있습니다.
특정 동작을 심플하게 작성하기 위해서 어떤 것이든 사용할 수 있으며, 오히려 추천됩니다.
그러나 이것은 반드시 정당화 할 수 있어야 합니다.
특정 기능을 간단하게 만드는 것은 일 전체를 그것으로 처리하는 것은 반드시 다르다는 것을 유념하세요.
```

```
💡 두 개의 마이너 #module 은 한개의 메이저 모듈과 동급입니다.
```

# 목차

## Web
	해당 모듈들은 퐁 게임에 발전된 웹 구성을 넣어줍니다.

- Major module: [[Backend module]]
- Minor module: [[FrontEnd module]]
- Minor module: [[Database module]]
- Major module: [[Blockchain module]]

## User Management
	해당 모듈들은 User Management 의 세계를 깊게 탐구하며,
	 퐁 플랫폼 내에서 사용자 상호 작용과 액세스 제어의 중요한 측면을 다룹니다.
	  이것은 두가지 주요 구성을 포함하며, 유저 관리 및 인증의 필수 요소에 중점을 둡니다.
	   1. 여러 토너먼트에 걸친 사용자 참여.
	   2. 원격 인증 구현.

- Major module: [[Standard User Management module]]
- Major module: [[Remote authentication]]

## Gameplay and User experience
	이 모듈들은 기본적인 게임플레이를 발전시키게끔 디자인 되어있습니다. 

- Major module: [[Remote players module]]
- Major module: [[Multiplayers module]]
- Major module: [[One more game module]]
- Minor module: [[Game Customization Options module]]
- Major module: [[Live chat module]]

## AI-Algo
	이 모듈은 프로젝트에 데이터 기반 요소를 도입하는 역할을 하며,
	 주요 모듈은 향상된 게임 플레이를 위한 AI 상대를 소개하고,
	 
	  사용자 및 게임 통계 대시보드를 위한 마이너 모듈은
	사용자에게 게임 경험을 미니멀하면서도 통찰력 있게 엿볼 수 있게 제공합니다. 

- Major module: [[AI Opponent module]]
- Minor module: [[User and Game Stats Dashboards module]]

## Cybersecurity
	이 모듈은 프로젝트의 보안 상태를 강화하도록 설계되어있습니다.
	메이저 모듈은 secure secrets management 를 위해서
	Web Application Firewall (WAF), ModSecurity Configurations 와 HashiCorp Vault 를 통해 보안을 강화합니다.
	
	마이너 모듈은 GDPR 준수 옵션, 사용자 데이터 익명화, 계정 삭제, two-factor authentication(2FA), Json Web Tokens (JWT)를 추가해서
	종합적으로 프로젝트의 데이터 보호, 보안, 인증 보안을 보장합니다.

- Major module: [[WAF  ModSecurity module]]
- Minor module: [[GDPR module]]
- Major module: [[Two-Factor Authentication and JWT module]]

## Devops
	이 모듈은 종합적으로 과제의 인프라 스트럭쳐와 아키텍쳐를 강화하는데 초점을 맞추고 있으며
	 그 수단으로 두가지 메이저 모듈과 한 가지 마이너 모듈을 제시합니다.

- Major module: [[Infrastructure module]]
- Minor module: [[Monitoring system module]]
- Major module: [[Microservice module]]

## Gaming
	💡 Gameplay and User experience 의 new game, 사용자 설정 모드와 같은 모듈입니다.
	
	목차에는 없지만, 나중에는 적혀있어서 추가해서 적어놨습니다.
	
- Major module: [[One more game module]]
- Minor module: [[Game Customization Options module]]

## Graphics
- Major module: [[3D game module]]

## Accessibility
	이 모듈은 호환성을 보장하면서 웹 어플리케이션의 접근성을 향상시키는 데 집중합니다.
	성능과 사용자 경험을 향상하기 위해서 브라우저를 지원하고,
	다국어 지원을 하고,
	시각장애인들을 이용해서 기능을 확장하고,
	서버사이트 렌더링(SSR) 을 제공합니다. 
	
- Minor module: [[support all device module]]
- Minor module: [[Browser Compatibility module]]
- Minor module: [[Multiple language module]]
- Minor module: [[Visually impaired Users module]]
- Minor module: [[Server-Side Rendering module]]

## Server-Side Pong

- Major module: [[Server-Side Pong module]]
- Major module: [[CLI Pong]]