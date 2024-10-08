---
tags:
  - "#Game"
---
### 이 웹사이트의 주요 목적은 다른 플레이어와 #Pong 게임을 하는 것 입니다.

- 그러므로 유저는 웹사이트에서 다른 유저와 #Pong 게임을 직접적으로 겨룰 수 있어야 합니다. 양쪽 플레이어는 같은 키보드를 사용합니다. [[Remote players module]]은 원격플레이로 이 기능을 향상시킬 수 있습니다.
- 플레이어는 다른 플레이어와 게임을 할 수 있어야 하는데, 또한 #토너먼트 를 제안할 수도 있어야 합니다. 이 토너먼트는 번갈아가면서 할 수 있는 여러 선수로 구성될 것입니다. 어떻게 토너먼트를 구현할 것인지는 유연하게 대응해도 됩니다. 하지만 반드시 누가 누구와 플레이 하고 있는지와 플레이어의 순위는 표시되어야 합니다.
- #등록시스템 이 필요합니다. #토너먼트 의 시작에, 각자의 플레이어는 별명을 입력해야 합니다. 별명들은 새로운 토너먼트가 시작되면 리셋됩니다. 그러나, 이 요구사항은 [[Standard User Management module]]로 수정될 수 있습니다.
- 게임에는 반드시 #매치메이킹 시스템이 있어야 합니다: #토너먼트 시스템은 참가자의 매치메이킹을 조직하고, 다음 경기를 발표합니다.
- 모든 플레이어는 같은 패들 속도를 포함하여 같은 규칙을 충실하게 따라야 합니다. 이 사항은 #AI 를 적용할 때에도 마찬가지입니다. AI 는 일반 플레이어와 같은 속도를 보여야 합니다.
- #game 자체는 반드시 기본 #frontend  규칙에 맞게 개발되어야합니다. 이 규칙에 선택사항으로 [[FrontEnd module]]을 채용해 기능을 활용하거나,  [[Graphics module]]을 선택해서 제약을 덮어쓸 수 있습니다. 시각적인 미학은 다양할 수 있지만, 반드시 Original Pong (1972) 의 본질을 가지고 있어야 합니다.
```
🚨 구현(또는 메인 역할)을 라이브러리, 프레임워크 또는 도구를 이용하여 대체하는 것은 절대 금지되어있습니다.
과제의 각 파트는 명시적으로 허용된 서드파티 소프트웨어를 명시하고 있습니다. 
이것 이외에 특정 액션을 쉽게 해줄 수 있는 어떤 프로그램이든 사용할 수 있으며 오히려 추천하는 바입니다. 
어떤 편의성 도구든 사용했다면 그것을 정당화 할 수 있어야 한다는 사실을 기억하세요. 
단순화 할 수 있게 도와준다는 표현과 전체 일을 완료한다는 표현은 같지 않다는 것을 명심하세요.
```


## [[Security concerns]] 로 넘어가기