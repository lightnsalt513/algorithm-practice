
# 프로그래머스 연습 > 스택/큐

<br />

## [기능개발](https://programmers.co.kr/learn/courses/30/lessons/42586)

<br />

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

제한 사항
- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.

<br />

입출력 예
| progresses | speeds | return |
|---|---|---|
| [93, 30, 55] | [1, 30, 5] | [2, 1] |
| [95, 90, 99, 99, 80, 99] | [1, 1, 1, 1, 1, 1] | [1, 3, 2] |

<br />

작업타임: 30분    
작업코드:

``` js
function solution(progresses, speeds) {
    if (progresses.length < 1 || progresses.length !== speeds.length) return;
    
    var answer = [], lastDaysLeft, counter = 0;
    var daysLeft = progresses.map((progress, i) => Math.ceil((100 - progress) / speeds[i]));
  
    daysLeft.forEach((days, i) => {
        if (i === 0) {
            lastDaysLeft = days;
            counter = 1;
        } else {
            if (days > lastDaysLeft) {
                answer.push(counter);
                lastDaysLeft = days;
                counter = 1;
            } else {
                counter++;
            }
            if (i === (daysLeft.length - 1)) {
                answer.push(counter);
            }
        }                
    });
    
    return answer;
}
```
<br />
<hr>
<br />

## [프린터](https://programmers.co.kr/learn/courses/30/lessons/42586)

<br />

일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.

1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.
3. 그렇지 않으면 J를 인쇄합니다.

예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.

내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.

현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

제한사항
- 현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.
- 인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.
- location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.

<br />

입출력 예
| priorities | location | return |
|---|---|---|
| [2, 1, 3, 2] | 2 | 1 |
| [1, 1, 9, 1, 1, 1] | 0 | 5 |

<br />

작업타임: 90분    
작업코드:

``` js
function solution(priorities, location) {
    var answer = 0,
        sortedArray = [...priorities],
        idxArray = [],
        newIdxArray = [],
        maxNum;

    for (var i = 0, max = sortedArray.length; i < max; i++) {
        idxArray.push(i);
    }

    var findMaxNum = function () {
      var maxValue = 0;
      for (var i = 0, max = sortedArray.length; i < max; i++) {
        if (sortedArray[i] > maxValue) maxValue = sortedArray[i];
      }
      return maxValue;
    };
    
    var checkPriorityFunc = function () {
        maxNum = findMaxNum();

        for (var i = 0, iMax = sortedArray.length; i < iMax; i++) {
            var removed = sortedArray.shift();
            var removedLocation = idxArray.shift();

            if (removed < maxNum) {
                sortedArray.push(removed);
                idxArray.push(removedLocation);
            } else {
                newIdxArray.push(removedLocation);
                if (sortedArray.length === 0) break;
                if (sortedArray[0] !== maxNum) {
                    checkPriorityFunc();
                    break;
                };
            }
        }
    }

    checkPriorityFunc();
    answer = newIdxArray.indexOf(location) + 1;
    
    return answer;
}
```