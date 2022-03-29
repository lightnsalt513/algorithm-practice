
## 이진수 출력

<br />

- 10진수 N이 입력되면 2진수로 변환하여 출력하는 프로그램을 작성 (재귀함수 사용)
- e.g. :
  - input: 11
  - output: 1011

<br />

``` js
function solution(num) {
  let answer = '';
  function dfs(num) {
    if (num === 0) return;
    dfs(num / 2);
    answer += String(n % 2);
  }
  dfs(num);
  return answer;
}
```

<br />

## 이진트리순회

<br />

- 7까지 출력
- 전위순회 출력 : 1 2 4 5 3 6 7 (부모 -> 왼쪽 -> 오른쪽)
- 중위순회 출력 : 4 2 5 1 6 3 7 (왼쪽 -> 부모 -> 오른쪽)
- 후위순회 출력 : 4 5 2 6 7 3 1 (왼쪽 -> 오른쪽 -> 부모)

<br />

``` js
function solution() {
  let answer = '';
  function dfs(num) {
    if (num > 7) return;
    answer += num; // 전위순회
    dfs(num * 2);
    // answer += num; // 중위순회
    dfs(num * 2 + 1);
    // answer += num; // 후위순회
  }
  dfs(1);
  return answer;
}
```

<br />

## 부분집합 구하기

<br />

- 자연수 N이 주어지면 1부터 N까지의 원소를 갖는 집합의 부분집합을 모두 출력
- 단 공집합은 미출력
- e.g. : 원소 3개가 있는 경우 부분집합은 `2^3` 여기서 공집합을 빼면 `2^3 - 1`
- 원소 1,2,3이 있다면 각 원소마다 참여 / 미참여로 이진트리 형식으로 구분이 가능하다

<br />

``` js
function solution(n) {
  let answer = [];
  let check = Array.from({ length: n + 1 }, () => 0);
  function dfs(num) {
    if (num === n + 1) {
      let temp = '';
      for (let i = 1; i <= n; i++) {
        if (check[i] === 1) temp += i + ' ';
      }
      // 공집합 처리
      if (temp.length > 0) answer.push(temp.trim());
    } else {
      check[num] = 1;
      dfs(num + 1);
      check[num] = 0;
      dfs(num + 1);
    }

  }
  dfs(1);
  return answer;
}
```

``` js
// another solution
function solution(n) {
  let answer = [];
  function dfs(num, str) {
    if (num === n + 1) {
      if (str.length) answer.push(str.trim())
    } else {
      dfs(num + 1, str + num + ' ');
      dfs(num + 1, str);
    }
  }
  dfs(1, '');
  return answer;
}
solution(3);
```

<br />

## 합이 같은 부분집합

<br />

- N개의 원소로 구성된 자연수 집합이 주어지면, 이 집합을 두 개의 분집합으로 나누었을 때 두 부분집합의 원소의 합이 서로 같은 경우가 존재하면 “YES"를 출력하고, 그렇지 않으면 ”NO"를 출력
- e.g. : {1, 3, 5, 6, 7, 10}이 입력되면 {1, 3, 5, 7} = {6, 10} 으로 두 부분집합의 합이 16으로 같은 경우가 존재

<br />

``` js
function solution(arr) {
  let answer = 'NO';
  let sum = arr.reduce((sum, num) => sum += num, 0);
  function dfs(depth, acc) {
    if (answer === 'YES') return;
    if (depth >= arr.length) {
      if (sum - acc === acc) answer = 'YES';
    } else {
      dfs(depth + 1, acc + arr[depth]);
      dfs(depth + 1, acc);
    }
  }
  dfs(0, 0);
  return answer;
}
console.log(solution([1,3,5,6,7,10]));
```

<br />

## 바둑이 승차

<br />

- 철수는 그의 바둑이들을 데리고 시장에 가려고 한다. 그런데 그의 트럭은 C킬로그램 넘게 태울수가 없다. 철수는 C를 넘지 않으면서 그의 바둑이들을 가장 무겁게 태우고 싶다. N마리의 바둑이와 각 바둑이의 무게 W가 주어지면, 철수가 트럭에 태울 수 있는 가장 무거운 무게를 구하는 프로그램을 작성
- 풀이 : 전체 무게의 부분집합을 구해서 가장 무거운 무게를 찾는 방식

<br />

``` js
function solution(c, weights) {
  let answer = Number.MIN_SAFE_INTEGER;
  let nCount = 0;
  function dfs(depth, total, count) {
    if (total > c) return;
    if (depth >= weights.length) {
      if (total > answer) {
        answer = total;
        nCount = count;
      }
    } else {
      dfs(depth + 1, total + weights[depth], count + 1)
      dfs(depth + 1, total, count)
    }
  }
  dfs(0, 0);
  return answer;
}
console.log(solution(259, [81, 58, 42, 33, 61]));
```
