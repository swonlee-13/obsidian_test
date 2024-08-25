[[EVM]] 항목에서 발췌.
# Accounts
Ehereum에는 같은 주소 공간을 공유하는 account 는 두가지 종류가 있습니다. 공개-개인키 쌍에 의해 제어되는 External accounts와 저장된 코드에 의해 제어되는 contract account 입니다.

`external account` 의 주소는 public key 에서 판단되는 반면, `contact account` 는 계약이 생성될 때 결정됩니다(작성자 주소와 그 주소에서 보낸 거래 수, 소위 "nonce"에서 파생됩니다).

계정이 코드를 저장 하는지 여부에 관계없이, 두 유형은 EVM에 의해 동등하게 취급됩니다.

모든 account 는 256 비트 단어를 매핑하는 Storage 라고 불리는 key-value 저장소가 있습니다.

게다가 모든 계정은 이더에 대한 잔액(balance)을 가지고 있으며(정확히는 Wei 에서, 1 에테르는 10**18we) 이더를 포함하는 거래를 보내 수정할 수 있습니다.

# 이더리움 계정 주소 생성 방식

## 1. 외부 계정(External Account)

- **정의**: 사용자가 직접 제어하는 계정
- **주소 생성 방식**: 공개키(public key)에서 파생됨
- **과정**:
    1. 개인키 생성 (256비트 무작위 숫자)
    2. 개인키로부터 공개키 생성
    3. 공개키의 Keccak-256 해시 계산
    4. 해시의 마지막 20바이트를 주소로 사용

## 2. 컨트랙트 계정(Contract Account)

- **정의**: 스마트 컨트랙트를 위한 계정
- **주소 생성 방식**: 컨트랙트 배포 시 결정됨
- **계산 공식**: `address = keccak256(rlp.encode([sender, nonce]))[12:]`
    - `sender`: 컨트랙트 생성자의 주소
    - `nonce`: 생성자 계정의 트랜잭션 카운트
    - `keccak256`: 해시 함수
    - `rlp.encode`: RLP 인코딩 (Recursive Length Prefix)
    - `[12:]`: 결과의 마지막 20바이트 선택

## 예시

1. 외부 계정 주소: `0x71C7656EC7ab88b098defB751B7401B5f6d8976F`
2. 컨트랙트 주소 (가정): 생성자 주소: `0x123...` Nonce: 5 결과 주소: `0x7a2b2a51003a4c28913052b4b9305d9d4ae3dfb2`
3. 코드 예시
```sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract AddressExample {
address public owner;

	constructor(){
		owner = msg.sender; // 외부 계정 주소가 여기에 저장됩니다
	}

	function getBalance() public view returns (uint256){ 
		return owner.balance; // 외부 계정의 잔액을 조회합니다
	}

	function sendEther() public payable { 
		payable(owner).transfer(msg.value); // 외부 계정으로 이더를 전송합니다
	}
}
```

## 주요 차이점

- 외부 계정: 공개키에서 직접 파생
- 컨트랙트 계정: 생성자와 nonce의 조합으로 결정

# 한 줄 요약
Contract 내부에 있는 adress 변수의 주소값은 호출할 때마다 매번 달라진다. 여기에 담을 변수는 External Account 의 public 값이다.
** 일단 이렇게 포인터 개념으로 생각해놓고 넘어가도 좋음 나중에 필요할 때 자세히 배우면 된다.