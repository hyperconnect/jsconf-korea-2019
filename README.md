# JSConf Korea 2019 Event - 쿠나의 핸드폰 비밀번호를 찾아라!

![kuna-icon](https://user-images.githubusercontent.com/40815423/63143786-e1be1100-c02a-11e9-8f0b-2c2ff08a6e37.png)

안녕? 나는 하쿠나 1등 호스트인 쿠나라고해!

이제 곧 방송을 시작해야하는데, 핸드폰이 바이러스에 걸리는 바람에 비밀번호가 1시간 마다 바뀌고 있지 뭐야..!

내 핸드폰의 비밀번호를 열심히 찾아준 친구에게 나의 마음을 담아서 1등에게는 아이패드에어, 2등에게는 에어팟을 줄께! 꼭 도와줘!

## 쿠나의 핸드폰 비밀번호를 찾는 방법

비밀번호에 대한 힌트는 두가지 형식으로 번갈아가면서 나오고 있어!

### 1. Up & Down

입력을 시도한 비밀번호보다 실제 비밀번호의 값이 크면 **Up**, 작으면 **Down**이라는 힌트가 나와!

예를 들어서, 실제 비밀번호가 1234이고 입력을 시도한 비밀번호가 `0123`이라면 `Up`이라는 힌트가 나오겠지?

### 2. Baseball Game

입력을 시도한 비밀번호와 실제 비밀번호가 비교되어서 힌트가 나오는 형식이야

실제 비밀번호와 자리수까지 일치하면 **Strike(S)**, 자리수는 일치하지 않지만 사용되는 숫자라면 **Ball(B)** 힌트가 나와!

예를 들어서, 실제 비밀번호가 1234이고 입력을 시도한 비밀번호가 `1023`이라면 `1S2B`이라는 힌트가 나오겠지? (1이 Strike, 2와 3이 Ball)

## 추가 사항

내 휴대전화의 비밀번호는 중복된 자리수가 없이 `0000~9999` 사이의 어떤 수로 1시간마다 계속해서 바뀌고 있어! 즉, `0001`처럼 같은 숫자(0)가 반복해서 사용되지 않아!

비밀번호 입력을 시도할 때마다 5점씩 점수가 차감되고, 답을 맞으면 100점이 추가되는데, 파이콘이 끝나고 가장 높은 점수를 가진 사람에게 엄청난 상품을 줄게!

## API

- 직접 API를 호출해서 이벤트에 참여할 수 있어! 여기에 필요한 API들을 알려줄게!
- 답은 1시간 마다 갱신되고, 높은 점수를 받은 사람에게 나의 마음을 담을 선물을 줄테니까! 긴장의 끈을 놓지마!

### 게임 시작하기

#### Request

> `[POST]` https://event.hpcnt.com/game/

```js
import axios from "axios";

axios.post("https://event.hpcnt.com/game/", {
  email: "test@test.com", // str
  phone_number: "01012345678", // str
  username: "test-username" // str
});
```

#### Response

```js
{
    id: '6wHm',    // 게임 진행에 꼭 필요한 정보니까 기억해둬! 이 ID를 통해서 너의 점수가 집계되거든!
    score: 0,
    username: 'test-username'
}
```

### 현재 내 정보 확인하기

> `[GET]` https://event.hpcnt.com/game/{id}/

#### Request

```js
import axios from "axios";

axios.get("https://event.hpcnt.com/game/6wHm/");
```

#### Response

```js
{
    id: '6wHm',
    score: 0,
    username: 'test-username'
}
```

### 답안 제출하기

> `[POST]` https://event.hpcnt.com/game/{id}/submit/

#### Request

```js
import axios from "axios";

axios
  .post("https://event.hpcnt.com/game/6wHm/submit/", {
    number: "0239" // str
  })
  .then(response => {
    console.log(response.data);
  });
```

#### Response

```js
// Example 1
{
    is_pass: false,    // boolean
    updown: null,    // null | UP | DOWN
    baseball: {strike: 0, ball: 3},
    score: -5   // int
}

// Example 2
{
    is_pass: false,    // boolean
    updown: 'UP',    // null | UP | DOWN
    baseball: null,    // null | {strike: int, ball: int}
    score: -10   // int
}

// Example 3
{
    is_pass: false,    // boolean
    updown: null,    // null | UP | DOWN
    baseball: {strike: 2, ball: 0},    // null | {strike: int, ball: int}
    score: -15   // int
}

// Example 4
{
    is_pass: false,    // bool
    updown: 'DOWN',    // null | UP | DOWN
    baseball: null,    // null | {'strike: int, 'ball': int}
    score: -20   // int
}
```

### 게임 진행을 위한 ID 찾기

만약에 ID를 까먹었다고 해서 당황하지 마! 이벤트에 참여하기 위해 입력한 정보를 통해서 다시 찾을 수 있어!

> `[POST]` https://event.hpcnt.com/game/find/

#### Request

```js
import axios from "axios";

axios
  .post("https://event.hpcnt.com/game/find/", {
    email: "test@test.com", // str
    phone_number: "01012345678" // str
  })
  .then(response => {
    console.log(response.data);
  });
```

#### Response

```js
{
  results: [
    { id: "Y0HX", score: 0, username: "test-username" },
    { id: "xWHW", score: 100, username: "test-username" },
    { id: "NKH6", score: -55, username: "test-username" }
  ];
}
```

## 개인정보처리방침

```
㈜하이퍼커넥트는 JSConf Korea 2019와 하이퍼커넥트 주관 이벤트에 참석하시분들을 대상으로 아래와 같이 개인정보를 수집, 이용하고 있습니다. 수집한 개인정보는 '정보통신망 이용촉진 및 정보보호 등에 관한 법률' 및 기타 관계 법령에 의거하여 보호됩니다.

• 수집, 이용목적: 입사 및 이벤트 지원에 따른 본인확인, 고지사항의 전달, 문의사항에 대한 응대, 원활한 의사소통의 경로 확보

• 수집하는 개인정보 항목: 이메일주소, 휴대폰번호

• 개인정보 보유 및 이용기간: 제출일로부터 1년

• 동의를 거부할 권리 및 동의 거부에 따른 불이익: 설문응답자는 개인정보의 수집, 이용 등과 관련한 위 사항에 대하여 원하지 않는 경우 동의를 거부할 수 있습니다. 다만, 수집하는 개인정보의 항목에서 필수정보에 대한 수집 및 이용에 대하여 동의하지 않는 경우는 향후 (주)하이퍼커넥트 주관행사 및 이벤트 당첨 안내 시 제한이 있을 수 있습니다. 그 밖의 사항은 개인정보처리관련법을 준수합니다.
```
