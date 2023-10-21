## [🎲](https://www.codetree.ai/training-field/frequent-problems/problems/cube-rounding-again/description) [ct] **정육면체 한번 더 굴리기**

> **소요 시간: 47분<br>
> 메모리: 9MB<br>
> 시간: 104ms**

## 문제 접근

* 주사위의 앞쪽, 왼쪽, 아래쪽에 대한 숫자값을 저장함
* 입력 받은 후 보드판을 그룹화하여 보드판의 점수를 미리 계산함 (BFS)

## 전체 코드

```java
import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
    static final int[] di = new int[] { 0, 1, 0, -1 };
    static final int[] dj = new int[] { 1, 0, -1, 0 };
    static int N, M;
    static int[][] map, scoreBoard;

    public static void main(String[] args) throws Exception {
        init();
        int totalScore = 0;
        int diceFront = 2, diceLeft = 4, diceDown = 6;
        int diceI = 0, diceJ = 0, diceDirection = 0;
        while (M-- > 0) {
            // 주사위 움직임
            int ni = diceI + di[diceDirection];
            int nj = diceJ + dj[diceDirection];
            if (!check(ni, nj)) {
                diceDirection = (diceDirection + 2) % 4;
                ni = diceI + di[diceDirection];
                nj = diceJ + dj[diceDirection];
            }
            diceI = ni;
            diceJ = nj;

            // 주사위 면 변경
            int tempDiceDown = diceDown;
            if(diceDirection==0) { //우
                diceDown = 7 - diceLeft;
                diceLeft = tempDiceDown;
            }else if(diceDirection==1) { //하
                diceDown = diceFront;
                diceFront = 7 - tempDiceDown;
            }else if(diceDirection==2) { //좌
                diceDown = diceLeft;
                diceLeft = 7 - tempDiceDown;
            }else { //상
                diceDown = 7 - diceFront;
                diceFront = tempDiceDown;
            }

            // 점수 얻음
            totalScore += scoreBoard[diceI][diceJ];

            // 방향 결정
            if (diceDown > map[diceI][diceJ]) {
                if(++diceDirection == 4) diceDirection=0;
            } else if (diceDown < map[diceI][diceJ]) {
                if(--diceDirection == -1) diceDirection=3;
            }
        }
        System.out.println(totalScore);
    }

    static void init() throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine(), " ");

        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());

        map = new int[N][N];
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine(), " ");
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        fillScoreBoard();
     
    }

    static void fillScoreBoard() {
        scoreBoard = new int[N][N];
        Queue<int[]> queue = new LinkedList<>();
        Queue<int[]> memory = new LinkedList<>();
        boolean[][] visited = new boolean[N][N];

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                if (!visited[i][j]) {
                    int cnt = 1;
                    visited[i][j] = true;
                    queue.add(new int[] { i, j });
                    while (!queue.isEmpty()) {
                        int[] node = queue.poll();
                        for (int z = 0; z < 4; z++) {
                            int ni = node[0] + di[z];
                            int nj = node[1] + dj[z];
                            if (check(ni, nj) && !visited[ni][nj] && map[ni][nj] == map[i][j]) {
                                cnt++;
                                visited[ni][nj] = true;
                                queue.add(new int[] { ni, nj });
                            }
                        }
                        memory.add(node);
                    }

                    int score = map[i][j] * cnt;
                    while (!memory.isEmpty()) {
                        int[] node = memory.poll();
                        scoreBoard[node[0]][node[1]] = score;
                    }
                }
            }
        }
    }

    static boolean check(int i, int j) {
        if (i < 0 || i >= N || j < 0 || j >= N)
            return false;
        return true;
    }

}
```
