## [💪](https://school.programmers.co.kr/learn/courses/15008/lessons/121684) [#2번] 체육대회 

> **소요 시간: 20분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

* 순열을 이용해서 종목 대표를 선정한 후 최대 능력치 합을 갱신함
### 전체 코드

```java
class Solution {
    int max,N,M;
    int[][] ability_arr;
    boolean[] checked;
    
    public int solution(int[][] ability) {
        max = 0;
        N = ability.length;
        M = ability[0].length;
        ability_arr = new int[N][M];
        checked = new boolean[N];
        
        for(int i=0; i<N; i++){
            for(int j=0; j<M; j++){
                ability_arr[i][j] = ability[i][j];
            }
        }
        makeTeam(0,0);
        return max;
    }
    
    void makeTeam(int idx, int value){
        if(idx==M){
            if(max<value) max = value;
            return;
        }
        
        for(int i=0; i<N; i++){
            if(!checked[i]){
                checked[i]=true;
                makeTeam(idx+1, value+ability_arr[i][idx]);
                checked[i]=false;
            }
        }
    }
}
```
