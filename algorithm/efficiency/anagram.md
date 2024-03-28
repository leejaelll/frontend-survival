# 아나그램

### 문제설명

Anagram이란 두 문자열이 알파벳의 나열 순서를 다르지만 그 구성이 일치하면 두 단어를 아나그램이라고 합니다.
예를 들면 AbaAeCe 와 baeeACA 는 알파벳을 나열 순서는 다르지만 그 구성을 살펴보면 A(2), a(1), b(1), C(1), e(2)로 알파벳과 그 개수가 모두 일치합니다. 즉 어느 한 단어를 재 배열하면 상대편 단어가 될 수 있는 것을 아나그램이라 합니다.
길이가 같은 두 개의 단어가 주어지면 두 단어가 아나그램인지 판별하는 프로그램을 작성하세요. 아나그램 판별시 대소문자가 구분됩니다.


### 입력 설명

첫 줄에 첫 번째 단어가 입력되고, 두 번째 줄에 두 번째 단어가 입력됩니다. 단어의 길이는 100을 넘지 않습니다.


### 출력 설명

두 단어가 아나그램이면 “YES"를 출력하고, 아니면 ”NO"를 출력합니다.

### 입출력 예

입력 - AbaAeCe, baeeACA
출력 - 'YES'

입력 - abaCC, Caaab
출력 - 'NO'

### 풀이
1. 첫 번째 인자로 받은 알파벳과 알파벳 개수를 확인한다. 
    ```js
    {
        'A': 2,
        'a': 1,
        'b': 1,
        'C': 1,
        'e': 2,
    }
    ```
2. 두 번째 인자로 받은 알파벳을 순회하면서
    - 해당하는 알파벳이 없다면 "NO"를 리턴한다. 
    - 알파벳에 해당하는 값이 0이라면 "NO"를 리턴한다. 
    - 두 조건에 해당하지 않는다면 알파벳의 개수를 -1한다. 
3. 반복문에서 "NO"를 리턴하지 않는다면 아나그램이므로 "YES"를 리턴한다. 

```js
function solution(word1, word2) {
  const wordMap = new Map()

  for(let word of word1) {
    wordMap.has(word) ? wordMap.set(word, wordMap.get(word) + 1): wordMap.set(word, 1)
  }

  for(let word of word2) {
    if(!wordMap.has(word) || wordMap.get(word) === 0) return "NO"
    wordMap.set(word, wordMap.get(word) - 1)
  }

  return "YES"
}
```