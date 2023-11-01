## [☕️](https://school.programmers.co.kr/learn/courses/15009/lessons/121689) [3번] 카페 확장

> **소요 시간: 25분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

* k초마다 손님 입장: 반복문 한번 돌때마다 k초가 걸린다고 생각하고 진행
	1. k초 안에 만들 수 있는 음료수 다 만들고, 음료가 완성된 손님 퇴장시키기
	2. 손님 입장시키기
	3. 남아있는 손님 카운트하고 필요한 경우 max 갱신하기
- 변수 설명
	- `max`: 남아 있는 손님 최댓값 
	- `start_idx`: 음료를 만들어야 하는 손님 인덱스
	- `end_idx`: 마지막에 줄서있는 손님 인덱스
	- `cur`: `start_idx` 손님 음료를 만드는데 남은 시간
### 전체 코드

```java
class Solution {
    public int solution(int[] menu, int[] order, int k) {
        int max = 1;
        int start_idx=0, end_idx=0, cur=menu[order[0]];
        int N = order.length;
        
        while(end_idx < N-1){
            // k초 안에 음료 만들기 및 퇴장
            int time = k;
            while(time > 0 && start_idx <= end_idx){
                if(cur <= time){
                    time -= cur;
                    cur = menu[order[++start_idx]];
                }else{
                    cur -= time;
                    time = 0;
                }
                
            }

            // 입장
            end_idx++;
            
            // 카운트
            if(max < end_idx-start_idx+1){
                max = end_idx-start_idx+1;
            }
            
        }
        
        return max;
    }
}
```
