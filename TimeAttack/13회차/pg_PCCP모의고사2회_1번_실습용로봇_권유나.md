## [🤖](https://school.programmers.co.kr/learn/courses/15009/lessons/121687) [1번] 실습용 로봇

> **소요 시간: 7분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

* 방향 변경, 앞 뒤 이동하는 문제
* 사방탐색 전 기초단계 문제 느낌
### 전체 코드

```java
class Solution {
    int[] dx = new int[]{0,1,0,-1};
    int[] dy = new int[]{1,0,-1,0};
    
    public int[] solution(String command) {
        int robot_x=0, robot_y=0, robot_d=0;
        for(char order: command.toCharArray()){
            if(order=='G'){
                robot_x+=dx[robot_d];
                robot_y+=dy[robot_d];
            }else if(order=='B'){
                robot_x-=dx[robot_d];
                robot_y-=dy[robot_d];
            }else if(order=='R'){
                if(robot_d++==3) robot_d=0;
            }else if(order=='L'){
                if(robot_d--==0) robot_d=3;
            }
        }
        return new int[]{robot_x,robot_y};
    }
}
```
