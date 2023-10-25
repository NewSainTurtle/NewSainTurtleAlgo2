### PG121684 체육대회

[📁문제보기](https://school.programmers.co.kr/learn/courses/15008/lessons/121684)

---

#### 문제 풀이

* DFS로 풀음
* 각 종목별 참여 학생을 선발하고 그 때의 합이 가장 최대인 것을 출력
---

#### 전체 코드

```java
import java.io.*;
import java.util.*;

class Solution {
    static int sportsNum, studentNum, answer;
    static boolean[] student;
    static int[][] arr;
    
    public int solution(int[][] ability) {
        studentNum = ability.length; // 학생수
        sportsNum = ability[0].length; // 종목수
        student = new boolean[studentNum]; // 학생 체크
        arr = ability;
        answer = 0;
        
        DFS(0,0);
        
        return answer;
    }
    
    static void DFS(int cnt, int sum){
        if(cnt==sportsNum){
            answer = Math.max(sum,answer);
            return;
        }
        
       for(int i=0;i<studentNum;i++){
           if(student[i]) continue;
           student[i] = true;
           DFS(cnt+1, sum + arr[i][cnt]);
           student[i] = false;
       }
        
    }
}
```
