
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
# Blockchain Basics
블록체인은 프로그래머들이 이해하기 그렇게 어렵지 않습니다. 이유는 대부분의 어려운 것들(mining, [hashing](https://en.wikipedia.org/wiki/Cryptographic_hash_function), [elliptic-curve cryptography](https://en.wikipedia.org/wiki/Elliptic_curve_cryptography), [peer-to-peer networks](https://en.wikipedia.org/wiki/Peer-to-peer), etc) 은 특정 기능과 약속을 제공하기 위해 그저 있을 뿐이기 때문입니다. 한번 주어진 기능들을 받아들이고 나면 기반에 있는 기술에 대해서는 걱정할 필요가 없습니다. - AWS 가 어떻게 내부적으로 돌아가는지 알아야 우리가 쓸 수 있는건 아니잖아요?

## Transactions
블록체인은 전세계적으로 공유되는 거래(transaction) 데이터베이스입니다. 이것은 모든 사람이 네트워크에 참여하는것 만드로도 모든 데이터베이스 항목을 읽을 수 있다는 것을 뜻합니다. 만약 데이터베이스에 뭔가를 바꾸고 싶으면, 모든 다른 유저들이 accept 하는 transaction(한국사람들도 트랜잭션이라고 부름)이라고 불리는 동작을 생성하면 됩니다. Transaction 이라는 단어는 당신이 만들고 싶은 변경(두 값을 동시에 변경하고 싶다고 가정)이 전혀 이루어지지 않았거나 완전히 적용되었다는 것을 의미합니다. (모 아니면 도 라고 생각하면 편합니다.) 게다가 트랜잭션이 데이터베이스에 적용되면, 어떤 다른 트랜잭션도 이것을 수정할 수 없습니다.

예를들어, 모든 전자화폐의 잔고 정보를 다 담아 놓은 하나의 테이블을 생각해 봅시다. 만약 특정 계정에서 다른 계정으로의 트랙잭션이 요청되면, 데이터베이스의 거래 특성은 한 계좌에서 금액을 빼면 항상 다른 계좌에 추가되도록 합니다. 어떤 이유로든, 대상 계정에 금액을 추가할 수 없다면, 원천 계정도 수정되지 않습니다.

더욱이, 트랜잭션은 항상 발신자(생성자)에 의해 암호화 서명됩니다. 이는 데이터베이스의 특정 수정에 대한 접근을 보호하는 것을 간단하게 만듭니다. 전자 화폐의 예시에서, 간단한 확인 과정을 통해 오직 계정의 키를 보유한 사람만이 그 계정으로부터 일정량의 보상(예: 이더)을 전송할 수 있도록 보장합니다.

## Blocks
비트코인의 관점에서 극복해야 할 마지막 장애물은 "double-spend attack" 입니다: 만약 두 트랜잭션이 네트워크 상에 존재하는데, 둘 다 특정 계정을 비우고 싶어한다면? 하나의 트랜잭션만이 유효할 것입니다. 통상적으로 먼저 받은 쪽이 유효할 것인데, 문제는 peer-to-peer network 에서 first 라는 것은 객관적인 관점이 아니라는 것입니다.

이 추상적인 질문에 대한 것은 당신은 신경쓰지 않아도 됩니다. 전 세계적으로 수락되는 트랜잭션의 순서가 당신을 위해 선택되고, 갈등을 해결할 것입니다. 거래는 block 이라 불리는 번들에 제공되고, 모든 참여 노드 간에 실행되고 배포될 것입니다. 두 거래가 서로 충돌한다면, 결국 두번째 거래는 거부되고 블록의 일부가 되지 않을 것입니다.

이 블록들은 선형인 구조를 시간에 따라서 형성합니다. 이것이 `blockchain` 이라는 단어가 유래한 곳입니다. 블록체인은 일정한 시간 각격으로 추가되지만, 이 시간 간격은 차후에 변경될 수도 있습니다. 가장 최신의 정보를 위해서 [[Eherscan]] 을 활용해서 모니터링 하는 것이 좋습니다.

`attestation` 이라 불리는 order selection mechanism(순서 선택 메커니즘) 의 일부로 블록들은 시간에 따라서 되돌아가지만, 체인의 "끝" 에서만 발생할 수 있습니다. 더 많은 블록들이 특정 블록 위에 추가될 수록, 이 블록은 더 되돌아갈 가능성이 줄어듭니다. 그러므로 당신의 거래가 되돌아갈 수도, 심지어 제거될 수도 있지만 기다릴 수록 그럴 가능성이 줄어듭니다.

```
트랜잭션은 다음 블록이나 특정 미래 블록에 포함될 것을 보장하지 않습니다. 왜냐하면 이런 사항은 트랜잭션 제출자에게 달려 있는 것이 아니라, 거래가 어떤 블록에 포함되는지 결정하는 것은 광부에게 달려있기 때문입니다.

만약에 당신의 contract의 호출을 스케쥴링 하고싶으면, 오라클 서비스나 smart contract automation 을 이용할 수 있습니다.
```

---
# [[EVM]]
문서 참고.

---
