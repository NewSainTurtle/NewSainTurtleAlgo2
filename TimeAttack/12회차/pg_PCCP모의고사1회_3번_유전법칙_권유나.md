## [🧬](https://school.programmers.co.kr/learn/courses/15008/lessons/121685) [#3번] 유전법칙

> **소요 시간: 60분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

* `checkParent` 함수를 통해 조상 유전자의 위치를 저장한다.
* `checkDNA` 함수를 통해 2세대부터 n세대까지 탐색하며 `RR`과 `rr` 유전자를 찾아낸다. 아니라면, `Rr`유전자이다.
### 전체 코드

```java
class Solution {
    int[] DNA_parent;
    public String[] solution(int[][] queries) {
        int M = queries.length;
        String[] answer = new String[M];
        for(int m=0; m<M; m++){
            int n = queries[m][0];
            int p = queries[m][1];
            DNA_parent = new int[17];
            checkParent(n,p);
            answer[m] = checkDNA(n,p);
        }
        return answer;
    }
    
    void checkParent(int n, int p){
        while(n > 0){
            DNA_parent[n] = p;
            n--;
            p = (p-1)/4+1;
        }
    }
    
    String checkDNA(int n, int p){
        long t = 1;
        
        for(int i=2; i<=n; i++){
            if(DNA_parent[i]%4==1){
                return "RR";
            }else if(DNA_parent[i]%4==0){
                return "rr";
            }
            t*=4;
        }
        return "Rr";
    }

}
```
