# 프로그래머스 연습

<br />

## [크레인 인형뽑기 게임](https://programmers.co.kr/learn/courses/30/lessons/64061)

<br />

자세한 문제 설명은 위의 Title 클릭해서 내용 확인
- N X N 구조의 인형들이 쌓인 2차원 배열을 board 매개변수로 전달하고, moves에는 각 board의 가로 크기 이하인 자연수들을 담은 배열로 구성
- moves로 하나씩 인형을 제거하여 stack 구조의 바구니에 쌓아두고 이때 동일한 인형이 쌓이면 이 개수를 count
- 총 제거된 인형 개수를 반환할 것

<br />

작업타임: 90분    
작업코드:


``` js
function solution(board, moves) {
    let newBoard = board.map(arr => arr.slice());
    let basket = [];
    let count = 0;
    
    for (let move = 0, imax = moves.length; move < imax; move++) {
        for (let j = 0, jmax = newBoard.length; j < jmax; j++) {
            const currentItem = newBoard[j][moves[move] - 1];
            if (currentItem === 0) continue;
            basket.push(currentItem);  
            if (basket[basket.length - 2] === currentItem) {
                basket.splice(-2);
                count += 2;
            }
            newBoard[j][moves[move] - 1] = 0;
            break;
        }
    }
    
    return count;
}
```

<br />

---

<br />

## [실패율](https://programmers.co.kr/learn/courses/30/lessons/42889)

<br />

자세한 문제 설명은 위의 Title 클릭해서 내용 확인
- 매개변수 N은 총 게임의 스테이지 개수
- 매개변수 stages는 각 사용자가 현재 도달한 stage를 담은 배열
- 실패율 = 스테이지에 도달했으나 아직 클리어하지 못한 플레이어의 수 / 스테이지에 도달한 플레이어 수
- 만약 실패율이 같은 스테이지가 있다면 작은 번호의 스테이지가 먼저 오도록 함
- 스테이지에 도달한 유저가 없는 경우 해당 스테이지의 실패율은 0 으로 정의
- 실패율이 높은 스테이지부터 내림차순으로 스테이지의 번호가 담겨있는 배열을 return 하도록 함수 구현

<br />

작업타임: 35분    
작업코드:


``` js
function solution(N, stages) { 
    // N만큼 loop를 돌아서
    // 1) 각 스테이지의 클리어하지 못한 플레이어 개수 구하고
    // 2) 스테이지에 도달한 플레이어 수 (num >= stage) 를 구해서 실패율을 구함
    // 3) 그리고 배열 내 object에 저장 [{stage: 1, failrate: 실패율}, ... ]
    // 생성된 배열 구조를 가지고 sort를 사용해서 순서 바꿔보기 

    const dataArr = [];
    
    for (let i = 1, max = N; i <= max; i++) {
        let numberofFail = stages.filter(stage => stage === i).length;
        let numberofSuccess = stages.filter(stage => stage >= i).length;
        dataArr.push({
            stage: i,
            failrate: numberofFail / numberofSuccess
        });
    }
    
    dataArr.sort((a, b) => {
        if (b.failrate === a.failrate) {
            return a.stage - b.stage;
        }
        return b.failrate - a.failrate;
    });
    
    return dataArr.map(arr => arr.stage);
}
```



    