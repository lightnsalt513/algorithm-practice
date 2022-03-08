# 프로그래머스 연습

<br />

## [소수 찾기](https://programmers.co.kr/learn/courses/30/lessons/42839)

<br />

자세한 문제 설명은 위의 Title 클릭해서 내용 확인
- 한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.
각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.
- 제한사항 : 
    - numbers는 길이 1 이상 7 이하인 문자열입니다.
    - numbers는 0~9까지 숫자만으로 이루어져 있습니다.
    - "013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

<br />
 
작업코드:

``` js
function solution(numbers) {
    const numArray = numbers.split('');
    let answer = [];
    let count = 0;
    
    function isPrimeNumber(num) {
        if (num <= 1) return false;
        for (let i = 2, max = num; i <= max; i++) {
            if (num % i === 0) {
                if (i === max) return true;
                return false;
            };
        }
    }
    
    function getVariations(arr, selectNum) {
        let result = [];
        if (selectNum === 1) return arr.map(el => [el]);
        
        arr.forEach((fixed, index) => {
            let rest = [...arr.slice(0, index), ...arr.slice(index + 1)];
            let variations = getVariations(rest, selectNum - 1);
            let attached = variations.map(el => [fixed, ...el]);
            result.push(...attached);
        });
        
        return result;
    }
    
    for (let i = numArray.length; i >= 1; i--) {
        answer = answer.concat(getVariations(numArray, i));    
    }
    
    answer = answer.map(arr => Number(arr.join('')));
    answer = [...new Set(answer)].filter(num => isPrimeNumber(num));
    
    return answer.length;
}
```

<br />

## [카펫](https://programmers.co.kr/learn/courses/30/lessons/42842)

<br />

자세한 문제 설명은 위의 Title 클릭해서 내용 확인
- Leo는 카펫을 사러 갔다가 아래 그림과 같이 중앙에는 노란색으로 칠해져 있고 테두리 1줄은 갈색으로 칠해져 있는 격자 모양 카펫을 봤습니다. Leo는 집으로 돌아와서 아까 본 카펫의 노란색과 갈색으로 색칠된 격자의 개수는 기억했지만, 전체 카펫의 크기는 기억하지 못했습니다. Leo가 본 카펫에서 갈색 격자의 수 brown, 노란색 격자의 수 yellow가 매개변수로 주어질 때 카펫의 가로, 세로 크기를 순서대로 배열에 담아 return 하도록 solution 함수를 작성해주세요.
- 제한사항 :
    - 갈색 격자의 수 brown은 8 이상 5,000 이하인 자연수입니다.
    - 노란색 격자의 수 yellow는 1 이상 2,000,000 이하인 자연수입니다.
    - 카펫의 가로 길이는 세로 길이와 같거나, 세로 길이보다 깁니다.

<br />
 
작업코드:

``` js
function solution(brown, yellow) {
    const answer = [];
    let brownLeft = (brown - 4) / 2;
    
    for (let i = 1; i <= brownLeft; i++) {
        const other = brownLeft - i;
        if (i * other === yellow) {
            answer.push(other + 2);
            answer.push(i + 2);
            break;
        }
    }
    
    return answer;
}
```