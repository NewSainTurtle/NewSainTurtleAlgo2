## [🗺️](https://school.programmers.co.kr/learn/courses/15009/lessons/121690) [4번] 보물 지도

> **소요 시간: 17분<br>
> 메모리: —KB<br>
> 시간: —ms**

### 문제 접근

- 최단시간을 구하는 문제이므로 bfs로 문제를 풀었다.
- `visited[i][j][v]`: `v`는 마법 사용여부 
	- v가 0이면, 아직 마법을 사용하지 않고 이동하는 상태
	- v가 1이면, 마법을 사용하고 이동하는 상태
- `hole`이 보드판 배열에 체크되는 형식으로 주어진게 아니라, 장애물의 위치를 제공함.
	- `visited`에 표시해서 플레이어가 이동할 수 없도록 함.
- `bfs`로 플레이어를 사방탐색하며 움직인다. 마법을 사용하지 않은 상태라면 2번 점프하는 것을 고려해야한다.
	- `bfs`특징으로 인해 보물 지점에 도착하는 순간이 최단시간이 되므로 바로 시간을 리턴한다.
	- `queeu`가 빌때 까지 보물을 찾지 못하면 -1을 리턴한다.
### 전체 코드

```java
import java.util.Queue;
import java.util.LinkedList;

class Solution {
    int[] di = new int[]{-1,0,1,0};
    int[] dj = new int[]{0,1,0,-1};
    boolean[][][] visited;
    int N, M;
    
    public int solution(int n, int m, int[][] hole) {
        N = n;
        M = m;
        visited = new boolean[N+1][M+1][2];
        for(int[] h: hole){
            visited[h[0]][h[1]][0] = true;
            visited[h[0]][h[1]][1] = true;
        }
        return bfs();
    }
    
    int bfs(){
        Queue<int[]> queue = new LinkedList();
        
        queue.offer(new int[]{1,1,0,0}); // i,j,time,magic
        visited[1][1][0] = true;
        visited[1][1][1] = true;
        
        while(!queue.isEmpty()){
            int[] node = queue.poll();
            for(int z=0; z<4; z++){
                int ni = node[0]+di[z];
                int nj = node[1]+dj[z];
                
                if(check(ni,nj)&&!visited[ni][nj][node[3]]){
                    if(ni==N && nj==M){
                        return node[2]+1;
                    }else{
                        visited[ni][nj][node[3]]=true;
                        queue.offer(new int[]{ni,nj,node[2]+1,node[3]});
                    }
                }
                
                if(node[3]==0){
                    ni += di[z];
                    nj += dj[z];
                    if(check(ni,nj)&&!visited[ni][nj][1]){
                        if(ni==N && nj==M){
                            return node[2]+1;
                        }else{
                            visited[ni][nj][1]=true;
                            queue.offer(new int[]{ni,nj,node[2]+1,1});
                        }
                    }
                }
            }
        }
        return -1;
    }
    
    boolean check(int i, int j){
        return i > 0 && i <= N && j > 0 && j <= M; 
    }
}
```
