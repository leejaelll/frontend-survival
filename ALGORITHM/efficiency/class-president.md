# 학급 회장 - Hash Table

### 문제 설명

학급 회장을 뽑는데 후보로 기호 A, B, C, D, E 후보가 등록을 했습니다.
투표용지에는 반 학생들이 자기가 선택한 후보의 기호(알파벳)가 쓰여져 있으며 선생님은 그 기호를 발표하고 있습니다.
선생님의 발표가 끝난 후 어떤 기호의 후보가 학급 회장이 되었는지 출력하는 프로그램을 작성하세요. 반드시 한 명의 학급회장이 선출되도록 투표결과가 나왔다고 가정합니다.

### 입력 설명

첫 줄에는 반 학생수 N(5<=N<=50)이 주어집니다.
두 번째 줄에 N개의 투표용지에 쓰여져 있던 각 후보의 기호가 선생님이 발표한 순서대로 문자열로 입력됩니다.

### 출력 설명

학급 회장으로 선택된 기호를 출력합니다.

### 입출력 예

입력 - 15, BACBACCACCBDEDE
출력 - C

### 풀이

1. 알파벳과 알파벳의 갯수가 키-값으로 이뤄져있어야한다.
2. `Object.entreis()`를 사용해 배열로 만든 후 큰 값으로 정렬한다.
3. 가장 큰 값의 알파벳을 리턴한다.

```js
function solution(n, votes) {
  const votesMap = {};
  for (let vote of votes) {
    votesMap[vote] = votesMap[vote] + 1 || 1;
  }

  return Object.entries(votesMap).sort((a, b) => b[1] - a[1])[0][0];
}
```

Map을 이용한 풀이방법

1. Map 객체를 선언한다.
2. 문자열을 순회하면서 Map에 알파벳 카운트를 증가시킨다.
3. `entries` 메서드를 사용해 배열로 변환한 후 정렬 후 알파벳을 리턴한다.

```js
const votesMap = new Map();
for (let vote of votes) {
  votesMap.has(vote)
    ? votesMap.set(vote, votesMap.get(vote) + 1)
    : votesMap.set(vote, 1);
}

return [...votesMap.entries()].sort((a, b) => b[1] - a[1])[0][0];
```
