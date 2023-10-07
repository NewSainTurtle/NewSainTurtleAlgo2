## [🏃‍♂️💨](https://www.codetree.ai/training-field/frequent-problems/problems/maze-runner/description) [ct] 메이즈 러너

> **소요 시간: 176분<br>
> 메모리: 9MB<br>
> 시간: 85ms**

## 문제 접근

* BFS, Map, List를 이용함

## 전체 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.List;
import java.util.StringTokenizer;

public class Main {
    static final int[] di = { -1, 1, 0, 0 };
    static final int[] dj = { 0, 0, -1, 1 };
    static int N, M, K, MapSize;
    static int[][] map;
    static int[][] infos;
    static List<Integer> runners;
    static int exitIdx;

    public static void main(String[] args) throws Exception {
        init();
        int total = 0;
        while (K-- > 0) {
            // 참가자 이동
            for (int r = runners.size() - 1; r >= 0; r--) {
                int i = infos[runners.get(r)][1];
                int j = infos[runners.get(r)][2];
                int curDist = Math.abs(i - infos[exitIdx][1]) + Math.abs(j - infos[exitIdx][2]);
                for (int z = 0; z < 4; z++) {
                    int ni = i + di[z];
                    int nj = j + dj[z];
                    
                    if (check(ni, nj)
                            && Math.abs(ni - infos[exitIdx][1]) + Math.abs(nj - infos[exitIdx][2]) < curDist) {
                        // 이동
                        runners.set(r,map[ni][nj]);
                        total++;
                        if (runners.get(r) == exitIdx) {
                            runners.remove(r); // 이동거리 더하기
                        }
                        break;
                    }
                }
            }

            if(runners.isEmpty()) break;

            // 미로 시계 회전
            int min = N;
            int i = N - 1, j = N - 1;
            for (int runner : runners) {
                
                int temp = Math.max(Math.abs(infos[runner][1] - infos[exitIdx][1]),
                        Math.abs(infos[runner][2] - infos[exitIdx][2]));
                int tempI = Math.max(infos[runner][1], infos[exitIdx][1]);
                int tempJ = Math.max(infos[runner][2], infos[exitIdx][2]);
                int MinI = Math.max(0, tempI - temp);
                int MinJ = Math.max(0, tempJ - temp);

                if (temp < min || (temp == min && (MinI < i || (MinI == i && MinJ < j)))) {
                    min = temp;
                    i = MinI;
                    j = MinJ;
                }
            }
            rotate(min+1, i, j);

        }
        StringBuilder sb = new StringBuilder();
        sb.append(total).append("\n").append(infos[exitIdx][1]+1).append(" ").append(infos[exitIdx][2]+1);
        System.out.print(sb);
    }

    static void init() throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        K = Integer.parseInt(st.nextToken());
        MapSize = N * N;

        map = new int[N][N];
        infos = new int[MapSize][3];
        runners = new LinkedList<Integer>();

        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            for (int j = 0; j < N; j++) {
                map[i][j] = i * N + j;
                infos[i * N + j] = new int[] { Integer.parseInt(st.nextToken()), i, j };
            }
        }

        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            runners.add(map[Integer.parseInt(st.nextToken()) - 1][Integer.parseInt(st.nextToken()) - 1]);
        }

        st = new StringTokenizer(br.readLine(), " ");
        exitIdx = map[Integer.parseInt(st.nextToken()) - 1][Integer.parseInt(st.nextToken()) - 1];

        // 초기 참가자들 출구에 있는 지 확인
        for (int i = runners.size() - 1; i >= 0; i--) {
            if (runners.get(i) == exitIdx) {
                runners.remove(i);
            }
        }

    }

    static boolean check(int i, int j) {
        if (i < 0 || i >= N || j < 0 || j >= N || infos[map[i][j]][0] > 0)
            return false;
        return true;
    }

    static void rotate(int size, int startI, int startJ) {
        int[][] temp = new int[size][size];
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                temp[i][j] = map[i + startI][j + startJ];
            }
        }

        
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) { 
                map[i + startI][j + startJ] = temp[size - 1 - j][i];
            }
        }
        
        for (int i = 0; i < size; i++) {
            for (int j = 0; j < size; j++) {
                infos[map[i + startI][j + startJ]][1] = i+startI;
                infos[map[i + startI][j + startJ]][2] = j+startJ;
                if(infos[map[i + startI][j + startJ]][0]>0) infos[map[i + startI][j + startJ]][0]--;
            }
        }
        
    }
}
```
