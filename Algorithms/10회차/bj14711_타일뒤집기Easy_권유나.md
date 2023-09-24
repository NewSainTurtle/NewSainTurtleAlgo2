## [⬛️⬜️](https://www.acmicpc.net/problem/14711) [bj14711] 타일 뒤집기 (Easy)

> **소요 시간: 72분<br>
> 메모리: 24092KB<br>
> 시간: 204ms**

## 문제 접근

* 위에서 아래로 확인하면서 문제를 풀었다.
* 자신 바로 위에 타일이 검은색(`#`)인 경우 자신이 뒤집혀야 위에 있는 타일이 흰색(`.`)이 될 수 있다.

## 문제 풀이

1. `map`에 타일의 정보를 저장한다. 흰색이면 true, 검은색이면 false
2. `changeTile` 메서드를 통해 타일을 뒤집으며 초기값을 찾아낸다.
3. `count`는 타일들이 뒤집힌 횟수를 카운트한다.
4. 1행부터 N-1행까지 타일을 뒤집으며 확인한다
   1. i행의 모든 열의 타일을 확인한다.
      1. 바로 위에 있는 타일이 검은색(`!map[i-1][j]`)인 경우 현재 위치의 타일과,  왼쪽, 오른쪽, 아래방향의 타일을 뒤집는다. (`count`값을 올린다)
   2. i행의 모든 열의 타일을 확인했으면, 타일의 색을 설정한다.
      1. `count[i][j]`값이 짝수라면, 타일을 뒤집은 결과가 뒤집기 전과 동일하므로 `map[i][j]`에 `map[i-1][j]`값을 저장한다.
      2. `count[i][j]`값이 홀수라면, 타일을 뒤집은 결과가 뒤집기 전과 다르므로 `map[i][j]`에 `!map[i-1][j]`값을 저장한다.
5. `changeTile` 메서드가 종료되면, `map` 전체를 확인하며 결과를 출력한다. 
   1. `map[i][j]`값이 true면 흰색(`.`), false면 검은색(`#`)

## 전체 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

public class Main14711 {
    static final char WHITE = '.', BLACK = '#';
    static final int[] di = new int[]{0, 1, 0};
    static final int[] dj = new int[]{1, 0, -1};
    static int N;
    static boolean[][] map;

    public static void main(String[] args) throws Exception {
        init();
        changeTile();

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                sb.append(map[i][j] ? WHITE : BLACK);
            }
            sb.append("\n");
        }

        System.out.print(sb);
    }

    static void init() throws Exception {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        map = new boolean[N][N];
        String input = br.readLine();
        for (int j = 0; j < N; j++) {
            map[0][j] = input.charAt(j) == WHITE;
        }
    }

    static void changeTile() {
        int[][] count = new int[N][N];
        for (int i = 1; i < N; i++) {
            for (int j = 0; j < N; j++) {
                if (!map[i - 1][j]) {
                    count[i][j]++;
                    for (int z = 0; z < 3; z++) {
                        int ni = i + di[z];
                        int nj = j + dj[z];
                        if (ni >= 0 && ni < N && nj >= 0 && nj < N) {
                            count[ni][nj]++;
                        }
                    }
                }
            }

            for (int j = 0; j < N; j++) {
                map[i][j] = count[i][j] % 2 == 1 ? !map[i - 1][j] : map[i - 1][j];
            }
        }
    }
}

```
