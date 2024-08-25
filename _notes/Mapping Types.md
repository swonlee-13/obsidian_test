
Mapping Types 는 `mapping(keyType KeyName? => ValueType ValueName?` 와 같은 형식을 가지며, mapping type 의 변수는 다음과 같이 선언할 수 있습니다.

>mapping(keyType KeyName? => ValueType ValueName? VariableName

KeyType 은 어떤 built-in 타입이면 다 될 수 있습니다.(_bytes_, _string_ 뿐만아니라 contract, _enum_ 까지.)
ValueType 은 어떤 타입이든 될 수 있습니다. _mapping_ 타입, 배열, 구조체까지요.

KeyName 과 ValueName 은 선택사항이며, (그러므로 `mapping(KeyType => ValueType)` 도 가능합니다.)

_mapping_ 을 해시 테이블로 생각할 수 있으며, 가능한 모든 키가 존재하고 바이트 표현이 모든 0인 값인 값에 mapping되도록 사실상 초기화되는 해시 테이블로 생각할 수 있습니다. 유사성은 거기서 끝나고, 주요 데이터는 매핑에 저장되지 않으며, keccak256 해시만 값을 찾는 데 사용됩니다.

이것때문에 mapping 은 설정되는 키 또는 값의 길이나 개념이 없으므로, 할당된 키에 대한 추가 정보 없이는 지울 수 없습니다. [[clearing Mappings]]

_mapping_ 은 storage 라는 데이터 위치만 가질 수 있으므로, 상태 변수, 함수의 storage 참조 유형 또는 라이브러리 함수의 매개 변수로 허용됩니다. 이들은 매개변수나 public하게 볼 수 있는 함수의 반환값으로는 쓰일 수 없습니다. 이 제약은 _array_ 와 _mapping_ 을 포함하고 있는 구조체에도 해당됩니다.

mapping type 인 상태 변수를 public 으로 표시할 수 있으며, Solidity 는 당신을 위한 getter 를 만듭니다. KeyType 은 KeyName 의 자료형이고, ValueType 은 ValueName의 자료형으로 모든 요소에 재귀적으로 적용됩니다.

예시처럼 MappingExample Contract 는 balance 를 매핑하는데, address 는 Key Type 이 되고 uint 는 Value Type 으로써 Ethereum 주소를 uint 로 받습니다. getter 는 타입과 일치하는 값을 반환하며, MappintUser Contract 는 특정 주소의 value 를 반환합니다.
```
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.0 <0.9.0;

contract MappingExample {
    mapping(address => uint) public balances;

    function update(uint newBalance) public {
        balances[msg.sender] = newBalance;
    }
}

contract MappingUser {
    function f() public returns (uint) {
        MappingExample m = new MappingExample();
        m.update(100);
        return m.balances(address(this));
    }
}
```


# 요약
mapping type 은 key - value 를 관리하는 std::map 등을 생각하고 시작하면 좋습니다. 사용하는 법도 \[]로 접근하기 때문에 초반에는 이렇게 이해해도 문제는 없으나, 세부 동작에서 차이가 있으므로 문서를 더 읽어보는 것이 좋습니다.
