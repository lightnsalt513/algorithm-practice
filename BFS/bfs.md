
## 송아지 찾기

<br />

- 현수는 송아지를 잃어버렸다. 다행히 송아지에는 위치추적기가 달려 있다. 현수의 위치와 송아지의 위치가 수직선상의 좌표 점으로 주어지면 현수는 현재 위치에서 송아지의 위치까지 다음과 같은 방법으로 이동한다. 송아지는 움직이지 않고 제자리에 있다. 현수는 스카이 콩콩을 타고 가는데 한 번의 점프로 앞으로 1, 뒤로 1, 앞으로 5를 이동할 수 있다. 최소 몇 번의 점프로 현수가 송아지의 위치까지 갈 수 있는지 구하는 프로그램을 작성하세요.
- input : 첫 번째 줄에 현수의 위치 S와 송아지의 위치 E가 주어진다. 직선의 좌표 점은 1부터 10,000까지이다

<br />

``` js
function solution(start, end) {
  const moves = [1, -1, 5];
  const queue = [];
  const visited = Array.from({ length: 10001 }, () => 0);
  const distance = Array.from({ length: 10001 }, () => 0);
  queue.push(start)
  visited[start] = 1;
  
  while (queue.length) {
    const baseDistance = queue.shift();
    for (let move of moves) {
      const newDistance = baseDistance + move;
      if (newDistance === end) return distance[baseDistance] + 1;
      if (newDistance > 0 && newDistance <= 10000 && visited[newDistance] === 0) {
        visited[newDistance] = 1;
        distance[newDistance] = distance[baseDistance] + 1;
        queue.push(newDistance);
      }
    }
  }
}
console.log(solution(5, 14));
```

``` js
function solution2(start, end) {
  const moves = [1, -1, 5];
  const queue = [];
  const visited = Array.from({ length: 10001 }, () => 0);
  let level = 0;
  queue.push(start)
  visited[start] = 1;
  
  while (queue.length) {
    let len = queue.length;
    for (let i = 0; i < len; i++) {
      const baseDistance = queue.shift();
      for (let move of moves) {
        const newDistance = baseDistance + move;
        if (newDistance === end) return level + 1;
        if (newDistance > 0 && newDistance <= 10000 && visited[newDistance] === 0) {
          visited[newDistance] = 1;
          queue.push(newDistance);
        }
      }
    }
    level += 1;
  }
  return level;
}
console.log(solution2(5, 14));
```
<br />

## 섬나라 아일랜드

<br />

- N*N의 섬나라 아일랜드의 지도가 격자판의 정보로 주어집니다. 각 섬은 1로 표시되어 상하좌우와 대각선으로 연결되어 있으며, 0은 바다입니다. 섬나라 아일랜드에 몇 개의 섬이 있는지 구하는 프로그램을 작성하세요.

<br />

``` js
function solution(board) {
  let answer = 0;
  const dx = [-1, -1, 0, 1, 1, 1, 0, -1];
  const dy = [0, 1, 1, 1, 0, -1, -1, -1];
  const n = board.length;
  const queue = [];

  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      if (board[i][j] === 1) {
        board[i][j] = 0;
        queue.push([i, j]);
        answer++;
        while (queue.length) {
          const [x, y] = queue.shift();
          for (let k = 0; k < dx.length; k++) {
            let newX = x + dx[k];
            let newY = y + dy[k];
            if (newX > 0 && newY > 0 && newX < n && newY < n && board[newX][newY] === 1) {
              board[newX][newY] = 0;
              queue.push([newX, newY]);
            }
          }
        }
      }
    }
  }

  return answer;
}
const arr = [[1, 1, 0, 0, 0, 1, 0], [0, 1, 1, 0, 1, 1, 0], [0, 1, 0, 0, 0, 0, 0], [0, 0, 0, 1, 0, 1, 1], [1, 1, 0, 1, 1, 0, 0], [1, 0, 0, 0, 1, 0, 0], [1, 0, 1, 0, 1, 0, 0]]
console.log(solution(arr));
```