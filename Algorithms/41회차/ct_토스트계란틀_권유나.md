## [🍳](https://www.codetree.ai/training-field/frequent-problems/problems/toast-eggmold/description) [ct] 토스트 계란틀

> **소요 시간: 18분<br>
> 메모리: 25MB<br>
> 시간: 435ms**

## 리뷰
- 최근 삼성 기출 문제보다 문제 조건이 쉽게 주어져서 빠르게 풀 수 있었다.
- BFS를 활용해서 문제를 풀었다!

## 문제 풀이
1. `N`, `L`, `P`와 `map`을 입력받는다.
2. 계란의 이동 횟수를 저장하는 `round`를 0으로 초기화한다.
3. `splitEggBoard`함수를 호출한다.
4. `change`: 계란의 이동이 한번이라도 발생했는지 체크하는 플래그이다.
5. 완전 탐색을 하면서 방문하지 않은 공간이 있는지 확인한다.
6. 방문하지 않은 공간을 발견했다면, 방문처리를 하고, `queue`에 위치값을 넣는다.
7. 계란의 합을 나타내는 `sum`을 `map[i][j]`로 초기화한다.
8. `queue`가 빌 때까지 반복한다.
 1. `queue`에서 원소를 빼 `node`에 저장한다.
 2. `queue_save`에 `node`를 저장한다.(이후 계란의 값을 변경하기 위해)
 3. 사방탐색을 하면서 계란틀의 선을 분리한다.
  1. 사방탐색을 한 위치가 유효하고, 방문하지 않았으며, `node`계란의 차이가 `L`이상 `R`이하인 경우
  2. 방문처리를 하고, `queue`에 위치값을 넣고, `sum`에 계란 값을 더한다.
 3. `queue_save`가 1이상인 경우 계란의 이동이 발생했다는 의미이므로, `change`를 참으로 설정한다.
 4. `sum`을 `queue_save`의 크기로 나눠 `value`에 저장한다.
 5. `queue_save`에 저장된 계란들의 위치에 `value`값을 저장한다.
 6. `change`를 반환한다.
9. `splitEggBoard`의 반환값이 참이라면, `round`에 1 더한 후 3번 부터 다시 반복한다.
10. `splitEggBoard`의 반환값이 거짓이라면, 반복문을 종료하고 `round`를 출력한다.

## 전체 코드
```java
import java.io.*;
import java.util.*;

public class Main_토스트계란틀 {
	static int[] di = new int[] { -1, 0, 1, 0 };
	static int[] dj = new int[] { 0, -1, 0, 1 };
	static int N, L, R;
	static int[][] map;

	public static void main(String[] args) throws Exception {
		init();
		int round = 0;
		while (splitEggBoard()) {
			round++;
		}
		System.out.println(round);
	}

	static void init() throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		N = Integer.parseInt(st.nextToken());
		L = Integer.parseInt(st.nextToken());
		R = Integer.parseInt(st.nextToken());
		map = new int[N][N];
		for (int i = 0; i < N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for (int j = 0; j < N; j++) {
				map[i][j] = Integer.parseInt(st.nextToken());
			}
		}
	}

	static boolean splitEggBoard() {
		boolean change = false;
		Queue<int[]> queue = new LinkedList<int[]>();
		Queue<int[]> queue_save = new LinkedList<int[]>();
		boolean[][] visited = new boolean[N][N];

		for (int i = 0; i < N; i++) {
			for (int j = 0; j < N; j++) {
				if (!visited[i][j]) {
					visited[i][j] = true;
					queue.offer(new int[] { i, j });
					int sum = map[i][j];
					while (!queue.isEmpty()) {
						int[] node = queue.poll();
						queue_save.offer(node);

						for (int z = 0; z < 4; z++) {
							int ni = node[0] + di[z];
							int nj = node[1] + dj[z];
							if (inRange(ni, nj) && !visited[ni][nj] && check(node[0], node[1], ni, nj)) {
								visited[ni][nj] = true;
								queue.offer(new int[] { ni, nj });
								sum += map[ni][nj];
							}
						}
					}
					if (1 < queue_save.size())
						change = true;
					int value = sum / queue_save.size();
					while (!queue_save.isEmpty()) {
						int[] node = queue_save.poll();
						map[node[0]][node[1]] = value;
					}
				}
			}
		}

		return change;
	}

	static boolean check(int i, int j, int ni, int nj) {
		int diff = Math.abs(map[i][j] - map[ni][nj]);
		return L <= diff && diff <= R;
	}

	static boolean inRange(int i, int j) {
		return 0 <= i && i < N && 0 <= j && j < N;
	}
}
```
