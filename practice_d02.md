# 프로그래머스 연습

<br />

## [[1차] 뉴스 클러스터링](https://programmers.co.kr/learn/courses/30/lessons/17677)

<br />

자세한 문제 설명은 위의 Title 클릭해서 내용 확인
(자카드 유사도라는 방법을 구현)

<br />

작업타임: 60분    
작업코드:


``` js
function solution(str1, str2) {
    let answer = 0, numberToMultiply = 65536;
    let arr1 = [], arr2 = [], inter = [], union = [];
    let firstArr, secondArr;
    const turnToArrayFunc = function (str) {
        let newArr = str
            .split('')
            .map((char, i) => {
                if (i === str.length) return;
                return char + str[i+1];
            })
            .slice(0, -1)
            .join()
            .toLowerCase()
            .match(/[a-z]{2}/ig);
        newArr = newArr === null ? [] : newArr;
        return newArr;
    }
    
    arr1 = turnToArrayFunc(str1);
    arr2 = turnToArrayFunc(str2);

    if (!arr1.length || !arr2.length) {
        if (!arr1.length && !arr2.length) {
            answer = 1 * numberToMultiply;
            return answer;
        }
        answer = 0;
    } else {
        for (let i = 0, iMax = arr1.length; i < iMax; i++) {
            for (let j = 0, jMax = arr2.length; j < jMax; j++) {
                if (arr1[i] === arr2[j]) {
                    arr2.splice(j, 1);
                    inter.push(arr1[i]);
                    break;
                }
            }
            union.push(arr1[i]);
        }

        for (let i = 0, max = arr2.length; i < max; i++) {
          union.push(arr2[i]);
        }
        
        answer = inter.length / union.length;
    }
    
    answer = Math.floor(answer * numberToMultiply);

    return answer;
}
```