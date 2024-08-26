
# 간단한 Smart Contract
변수에 값을 저장하고, 다른 계약자가 접근해서 값을 확인할 수 있게 하는 간단한 코드로 시작하겠습니다. 전체적인것을 이해하지 못하더라도 읽어보세요.
## Storage 예시
```sol
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.16 <0.9.0;

contract SimpleStorage {
    uint storedData;

    function set(uint x) public {
        storedData = x;
    }

    function get() public view returns (uint) {
        return storedData;
    }
}
```
첫번째 줄은 소스코드가. GPL version 3.0 하에서 라이센스 된 것을 의미한다. 기계 판독 가능한 라이선스 지정자는 소스코드를 게시하는 것이 디폴트인 설정에서 중요합니다.

다음 라인은 Solidity 가 0.4.16 버전또는 그보다 최신의 버전에서 작성되었음을 명시합니다. 이것은 계약이 다으게 작동할 수 있는 (깨진) 컴파일러 버전으로 컴파일 할 수 없도록 하기 위함입니다. Pragma 는 컴파일러 지침입니다. (예시: Pragma once)

Solidity에서 Contract는 이더리움 블록체인의 특정 주소에 있는 코드와 데이터의 모음을 의미합니다. `uint storedData;` 는 `uint` 타입의 `storedData` 라는 변수를 선언합니다 (256바이트 unsigned int). 이것은 데이터베이스를 관리하는 코드의 함수를 호출하여 쿼리하고 변경할 수 있는 데이터베이스의 단일 슬롯으로 생각할 수 있습니다. 여기서 contract 는 데이터의 set 과 get 을 구현해서 값을 수정하거나 받아오는 일을 한다.
(첨부: 클래스의 private 변수와 유사하지만, 데이터를 읽고 쓸 때 [[gas fee]] 가 나가는 등 블록체인의 특성을 가지고 있다 생각하면 될 것 같다.)

현재 contract 에 접근하기 위해서 통상적인 this 접두어를 쓰지 않고, 그냥 이름으로 직접 접근합니다. 다른 언어와는 달리 이것을 생략하는 것은 단순히 style 의 차이가 아니라 멤버에 접근하는 다른 방식입니다만 나중에 더 설명하겠습니다.
(첨부: this 를 사용하면 연산량이 증가하기 때문에 이런 형식의 접근을 없앤 것으로 파악.)

이 Contract 는(이더리움이 구축한 인프라로 인해) 전 세계 누구나 접근할 수 있는 단일 번호를 저장할 수 있도록 하는 것 외에는 아직 많은 일을 하지 않습니다. 그리고 이 번호를 게시하는 것을 막을 수 있는 (실용 가능한) 방법 은 없습니다. 누구나 다른 값으로 설정을 다시 호출하고 번호를 덮어쓸 수 있지만, 그 번호는 여전히 블록체인의 역사에 저장됩니다. 나중에, 당신만이 번호를 변경할 수 있도록 접근 제한을 부과하는 방법을 알게 될 것입니다.
```
🚨 유니코드 문자를 입력할 땐 주의하세요. 다른 바이트 형식으로 바뀔 수 있습니다.
```

```
모든 identifiers (contract names, funcition names, var names)들은 ASCII charset 으로 제한됩니다. UTF-8 로 인코딩된 데이터를 문자열 변수에 저장할 수 있습니다.
```
## 하위 통화(subcurrency) 예시
다음 contract 는 암호화폐의 가장 간단한 형식을 구현합니다. 이 Contract 는 creator 만이 새로운 코인을 발행하고(다른 발행 방식이 가능합니다 (중앙화, 분산화, 알고리즘 기반,작업 증명 등 발행 조건 등을 선택할 수 있다는 것으로 해석됨)). 누구나 "Ethereum keypair"만 있으면 각자에게 유저이름, 비밀번호를 입력하지 않고 보낼 수 있습니다. 

>minting: 이란 코인을 새로 만들고, 발행을 관리하는 등의 행동을 하기 위한 함수입니다. 그래서 이 contract 배포자인지 확인해서 그 사람만 접근할 수 있는 require 가 있는겁니다. require 는 false 이면 동작을 즉시 종료하는 함수입니다.(에러메세지를 보낼 수도 있음)

```sol
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.26;

// This will only compile via IR
contract Coin {
    // The keyword "public" makes variables
    // accessible from other contracts
    address public minter;
    mapping(address => uint) public balances;

    // Events allow clients to react to specific
    // contract changes you declare
    event Sent(address from, address to, uint amount);

    // Constructor code is only run when the contract
    // is created
    constructor() {
        minter = msg.sender;
    }

    // Sends an amount of newly created coins to an address
    // Can only be called by the contract creator
    function mint(address receiver, uint amount) public {
        require(msg.sender == minter);
        balances[receiver] += amount;
    }

    // Errors allow you to provide information about
    // why an operation failed. They are returned
    // to the caller of the function.
    error InsufficientBalance(uint requested, uint available);

    // Sends an amount of existing coins
    // from any caller to an address
    function send(address receiver, uint amount) public {
        require(amount <= balances[msg.sender], InsufficientBalance(amount, balances[msg.sender]));
        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        emit Sent(msg.sender, receiver, amount);
    }
}
```
이 contract 는 새로운 개념을 소개합니다. 하나씩 따라가봅시다.
`address public minter;` 는 _address_ 타입의 [[상태 변수]]를 저장합니다. _address_ 타입은 연산을 금지한 160비트의 타입입니다. [[External accounts vs Contract accounts|계약상의 주소 또는 외부 주소]] 에 속하는 키페어 해시의 절반을 저장하기 좋은 자료형입니다. 

_public_ 키워드는 자동적으로 contract 외부에서 당신이 현재 상태 변수에 접근할 수 있는 getter함수를 자동적으로 생성합니다. 이 키워드가 없으면, 다른 contract 는 이 변수에 접근할 수 없습니다. 컴파일러에 의해서 생성된 함수의 코드는 다음과 동일합니다. (external 과 view 는 이 단계에서 무시하세요.)

```sol
function minter() external view returns (address) {return minter}
```

함수를 위에있는 것 처럼 추가해도 됩니다. 그러나 함수와 상태 변수는 같은 이름을 가지게 해야 합니다. 이걸 꼭 할 필요는 없지만, 컴파일러가 당신을 위해서 알아낼겁니다.

다음줄 `mapping(address -> uint) public balances;` 도 마찬가지로 public 상태변수를 만듭니다. 그러나 이것은 더 복잡한 데이터타입입니다. [[Mapping Types|(참고)]] 타입은 _unsigned int_ 로 주소를 매핑합니다.

Mapping 은 가능한 모든 키가 처음부터 존재하고 바이트 표현이 모두 0인 값에 매핑되도록 사실상 초기화된 해시 테이블로 볼 수 있습니다. 그러나, 매핑의 모든 키 목록이나 모든 값의 목록을 얻을 수 없습니다. 매핑에 추가한 것을 기록하거나, get 행위가 필요하지 않은 맥락에서 사용하세요. 또는 더 좋은 것은, List 를 쓰거나, 더 적합한 데이터 유형을 사용하세요.

_public_ 키워드에 의해 생긴 getter function 은 매핑의 경우에 더 복잡합니다. 형태는 다음과 같습니다.
```sol
function balances(address account) external view returns (uint) {
    return balances[account];
}
```
이 함수는 하나의 계좌의 balance(잔액)을 쿼리하는 데 사용할 수 있습니다.

`event Sent(address from, address to, uint amount);`는 "[[event]]"를 정의하며, 이 event 는 send 함수의 마지막 줄에서 emit(발생, 방출) 됩니다. Application 과 같은 Ethereum clients는 이 발생된 이벤트를 cost가 거의 없이 listen 할 수 있습니다. 수신자는 from, to, amount 를 받아서 트랜잭션을 추척할 수 있습니다.

>event 를 listen 하는 행위는 블록체인을 변경하지 않으므로, gas fee 가 사실상 발생하지 않는다고 볼 수 있습니다.

이 이벤트를 수신하기 위해서 당신은 javascript 코드를 사용할 수 있으며, 이 코드는 web3.js 를 이용해 만들어졌습니다. [[web3.js_example|보러가기]]

Constructor 는 contract 가 생성될 때에만 발생하며, 나중에 호출할 수 없습니다. 생성자가 호출되면 이 contract는 영구적으로 블록체인에 저장됩니다. _msg_ 변수는 **특수 전역 변수[(링크)](https://docs.soliditylang.org/en/v0.8.26/units-and-global-variables.html#special-variables-functions)** 입니다.

>여기서 msg.sender 는 이 함수 호출을 시작한 주소를 나타냅니다. 
>1. `constructor()`에서:
    - `owner = msg.sender;`는 컨트랙트를 배포한 주소를 `owner`로 설정합니다.
>2. `mint()` 함수에서:
    - `require(msg.sender == owner, ...)`는 함수 호출자가 소유자인지 확인합니다.
>3. `transfer()` 함수에서:
    - `balances[msg.sender]`는 함수를 호출한 주소의 잔액을 참조합니다.

mint 함수를 새로 발행된 코인을 다른 주소에 보냅니다. 여기서 require 함수는 정의된 상태를 만족하는지 확인해서 false 이면 모든 동작을 되돌립니다. 여기 예시에서 `require(msg.sender ==minter);` 이 contract 의 생성자 만이 접근할 수 있는 것입니다. 일반적으로, 제작자는 원하는 만큼 많은 토큰을 주조할 수 있지만, 어느 시점에서 이것은 "오버플로"라는 현상으로 이어지며, 작업은 revert 될 것입니다.

_send_ 함수는 다른 사람에게 코인을 보내기 위해 (이미 이 동전들 중 일부를 가지고 있는 사람) 누구나 사용할 수 있습니다. 발신자가 보낼 코인이 충분하지 않다면, if 조건은 true로 평가됩니다. 결과적으로, 되돌리면 `InsufficientBalance` 오류를 사용하여 발신자에게 오류 세부 정보를 제공하는 동안 작업이 실패합니다.

---
# [[Blockchain Basics]]

---
# [[EVM]]
문서 참고.

---
