## [💻](https://school.programmers.co.kr/learn/courses/15008/lessons/121686) [#4번] 운영체제

> **소요 시간: 40분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

- 호출된 시간으로 `program`을 정렬하고, 우선순위에 맞춰 `Program`클래스를 만들었다.
- 우선순위 큐를 이용해서 문제를 풀었다.
### 전체 코드

```java
import java.util.*;

class Solution {
    class Program implements Comparable<Program>{
        int score, callTime, runTime;
        public Program(int score, int callTime, int runTime){
            this.score = score;
            this.callTime = callTime;
            this.runTime = runTime;
        }
        
        @Override
        public int compareTo(Program o){
            return score == o.score? callTime - o.callTime : score-o.score;
        }
    }
    
    public long[] solution(int[][] program) {
        int N = program.length;
        Arrays.sort(program,  new Comparator<int[]>() { 
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[1]-o2[1];
            }
        });
       
        long[] answer = new long[11];
        PriorityQueue<Program> pq = new PriorityQueue();
        int time=program[0][1], idx=0, start = 0;
        Program cur = null;
        while(true){
            while(idx < N && time==program[idx][1]){
                pq.offer(new Program(program[idx][0],program[idx][1],program[idx++][2]));
            }
            
            if(cur!=null && time - start == cur.runTime){
                cur = null;
            }
            
            if(cur==null){
                if(pq.isEmpty()){
                    if(idx < N){
                        time = program[idx][1]-1;
                    }else{
                         answer[0]  = time;
                        break;
                    }
                }else{
                cur = pq.poll();
                start = time;
                answer[cur.score] += time - cur.callTime;
                }
            }
            time++;
        }
        return answer;
    }
}
```
