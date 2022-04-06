
## 경로탐색(인접행렬 : 노드개수가 적을때)

<br />

- 방향그래프가 주어지면 1번 정점에서 N번 정점으로 가는 모든 경로의 가지 수를 출력하는 프로그램을 작성하세요.
- 아래 그래프에서 1번 정점에서 5번 정점으로 가는 가지 수는 
- NOTE: 인접행렬은 각 node 확인할 때 전체 node를 for로 돌아서 visited 여부 확인후 아래로 탐색

<br />

``` js
function solution(n, arr) {
  let answer = 0;
  const graph = Array.from(Array(n + 1), () => Array(n + 1).fill(0));
  const visited = Array.from({ length: n + 1 }, () => 0);

  for (let [a, b] of arr) {
    graph[a][b] = 1;
  }

  function dfs(v) {
    if (v === n) {
      answer++
    } else {
      for (let i = 1; i <= n; i++) {
        if (graph[v][i] && visited[i] === 0) {
          visited[i] = 1;
          dfs(i);
          visited[i] = 0;
        }
      }
    }
  }

  visited[1] = 1;
  dfs(1);
  return answer;
}
const arr = [[1, 2], [1, 3], [1, 4], [2, 1], [2, 3], [2, 5], [3, 4], [4, 2], [4, 5]]
console.log(solution(5, arr));
```

<br />

## 경로탐색(인접리스트 : 노드개수가 많을때)

<br />

- 방향그래프가 주어지면 1번 정점에서 N번 정점으로 가는 모든 경로의 가지 수를 출력하는 프로그램을 작성하세요.
- 아래 그래프에서 1번 정점에서 5번 정점으로 가는 가지 수는 
- NOTE: 인접행렬로 검색하는 것은 메모리 차지도 많이하고 시간복잡도도 높다. 노드개수가 많은 경우는 인접리스트를 활용해야한다

<br />

``` js
function solution(n, arr) {
  let answer = 0;
  const graph = Array.from(Array(n + 1), () => Array());
  const visited = Array.from({ length: n + 1 }, () => 0);

  for (let [a, b] of arr) {
    graph[a].push(b)
  }

  function dfs(v) {
    if (v === n) {
      answer++
    } else {
      for (let i = 0; i <= graph[v].length; i++) {
        if (visited[graph[v][i]] === 0) {
          visited[graph[v][i]] = 1;
          dfs(graph[v][i]);
          visited[graph[v][i]] = 0;
        }
      }
    }
  }
  
  visited[1] = 1;
  dfs(1);
  return answer;
}
const arr = [[1, 2], [1, 3], [1, 4], [2, 1], [2, 3], [2, 5], [3, 4], [4, 2], [4, 5]]
console.log(solution(5, arr));
```

<br />

## 미로탐색

<br />

- 7 * 7 격자판 미로를 탈출하는 경로의 가지수를 출력하는 프로그램 작성

<br />

``` js
function solution(board) {
  let answer = 0;
  let dx = [-1, 0, 1, 0];
  let dy = [0, 1, 0, -1];

  function dfs(x, y) {
    if (x === 6 && y === 6) {
      answer++
    } else {
      for (let i = 0; i < dx.length; i++) {
        let newX = x + dx[i];
        let newY = y + dy[i];
        if (newX >= 0 && newX <= 6 && newY >= 0 && newY <= 6 && board[newX][newY] === 0) {
          board[newX][newY] = 1;
          dfs(newX, newY);
          board[newX][newY] = 0;
        }
      }
    }
  }
  board[0][0] = 1;
  dfs(0, 0)
  return answer;
}
const arr = [[0, 0, 0, 0, 0, 0, 0], [0, 1, 1, 1, 1, 1, 0], [0, 0, 0, 1, 0, 0, 0], [1, 1, 0, 1, 0, 1, 1], [1, 1, 0, 0, 0, 0, 1], [1, 1, 0, 1, 1, 0, 0], [1, 0, 0, 0, 0, 0, 0]]
console.log(solution(arr));
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


  function dfs(x, y) {
    board[x][y] = 0;
    for (let i = 0; i < dx.length; i++) {
      const newX = x + dx[i];
      const newY = y + dy[i];
      if (newX >= 0 && newY >= 0 && newX < n && newY < n && board[newX][newY] === 1 ) {
        dfs(newX, newY)
      }
    }
  }

  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      if (board[i][j] === 1) {
        answer++;
        dfs(i, j);
      }
    }
  }

  return answer;
}
const arr = [[1, 1, 0, 0, 0, 1, 0], [0, 1, 1, 0, 1, 1, 0], [0, 1, 0, 0, 0, 0, 0], [0, 0, 0, 1, 0, 1, 1], [1, 1, 0, 1, 1, 0, 0], [1, 0, 0, 0, 1, 0, 0], [1, 0, 1, 0, 1, 0, 0]]
console.log(solution(arr));
```
