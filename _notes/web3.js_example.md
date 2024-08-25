
```js
// Web3.js 1.x const
web3 = new Web3('웹소켓_프로바이더_URL');
const Coin = new web3.eth.Contract(ABI, 컨트랙트주소);
// 여기까지 코드는 제가 직접 추가한 것입니다. 이런 식으로 선행되는 동작은 몇가지 더 있어야 할 것입니다.

Coin.Sent().watch({}, '', function(error, result) {
    if (!error) {
        console.log("Coin transfer: " + result.args.amount +
            " coins were sent from " + result.args.from +
            " to " + result.args.to + ".");
        console.log("Balances now:\n" +
            "Sender: " + Coin.balances.call(result.args.from) +
            "Receiver: " + Coin.balances.call(result.args.to));
    }
})
```
### 코드 부연설명
`Coin.Sent().watch()` 는 Sent 이벤트를 감시합니다. 이 이벤트는 코인 전송 시 발생하며, 이 때 callback 함수가 실행됩니다.
- 인자
1. {}: 필터에 해당하며, 현재는 전부 받는 것을 의미합니다.
2. ' ': 추가적인 필터 옵션입니다.
3. callback 함수로써, 이벤트가 발생되었을 때 실행할 함수입니다. 두개의 매개변수는 다음과 같습니다.
	1. erorr: 오류가 발생했을 경우 오류 정보를 담습니다.
	2. result: 정상적일 시 이벤트 데이터를 담습니다.
- 키워드
1. result: 이 객체는 callback 함수에 전달된 이벤트 데이터를 포함하는 객체이며, 이 객체는 다음과 같은 멤버를 가지고 있습니다.
	-  `args`: 이벤트에서 정의된 매개변수들의 값
	- `event`: 이벤트의 이름
	- `logIndex`: 로그의 인덱스
	- `transactionIndex`: 트랜잭션의 인덱스
	- `transactionHash`: 트랜잭션의 해시
	- `address`: 이벤트를 발생시킨 컨트랙트의 주소
	- `blockHash`: 이벤트가 포함된 블록의 해시
	- `blockNumber`: 이벤트가 포함된 블록 번호
