## [💎](https://www.codetree.ai/training-field/frequent-problems/problems/ancient-ruin-exploration/description) [ct] 고대 문명 유적 탐사

> **소요 시간: 72분<br>
> 메모리: 9MB<br>
> 시간: 125ms**

## 문제 풀이
1. `map`: 현 라운드 유적지 형태 저장
2. `wall_numbers`: 유적 벽에 쓰인 숫자 조각 저장, `wall_index`: 아직 사용하지 않은 숫자 조각 중 가장 왼쪽에 위치한 인덱스 값 저장
3. `init`함수를 통해 입력을 모두 받고, `wall_index`를 0으로 초기화 한다.
4. `K`번 반복하며 획득한 유물의 가치를 출력한다. (5 ~ 15)
5. `goAdventure()`함수를 호출해 탐사를 진행한다.
   	1. 열을 1부터 3까지 반복하며 중심축의 열을 정한다.(오름차순)
   	2. 열을 정했다면, 행을 1부터 3까지 반복하며 중심축의 열을 정한다.(오름차순)
   	3. 행을 정했다면, `map`을 중심축을 중심으로 3*3격자를 90도 회전시킨다. `rotateMap()`
   	4. 회전시킨 `map`을 `countDiamond`를 통해 유물 1차 획득 개수를 구한다.
   	5. 유물 1차 획득 개수가 `max_diamond_count`보다 크거나, 유물 1차 획득 개수가 `max_diamond_count`가 같지만 회전한 각도가 작을 경우 `target`값과 `max_diamond_count`를 갱신한다.
   	6. 90, 180, 270도로 회전시키기 위해 (5-3 ~ 5-5)를 3번 반복한다.
   	7. 270도 회전시킨 `map`까지 확인이 끝났다면 마지막으로 `rotateMap()`를 한번더 호출해 원래 `map`으로 돌려놓는다.
6. `target`에 저장된 중심축과 각도대로 `map`을 회전시키고, `max_diamond_count`를 반환한다.
7. `goAdventure()`함수에서 반환된 유물의 가치가 0일 경우, 더이상 탐사를 진행할 수 없다는 의미로 반복문을 종료한다.
8. 아니라면, 유물 획득 과정을 진행한다.
9. 현재 라운드에서 획득하는 유물의 총 가치를 저장할 `total`를 0으로 초기화한다.
10. `getDiamond()`함수를 이용해 유물 1차 획득 후 `count`에 저장한다.
11. `getDiamond()`함수에서는 bfs를 활용해 유물을 획득한다.
12. `count`가 0 이상인 경우 `total`에 `count`를 더한다.
13. `fillMap()`함수를 호출해 빈 공간을 벽에 적힌 숫자 조각으로 채운다. (열-오름차순, 행-내림차순)
14. `map`을 채운 후 `getDiamond()`함수를 이용해 유물을 연쇄 획득하여 `count`에 저장하고 12번 과정으로 돌아간다.
15. 유물 연쇄 획득이 끝나면, `total`과 공백 1칸을 출력한다.

## 전체 코드
```java
import java.io.*;
import java.util.*;

//72m 고대 문명 유적 탐사
public class Main {
	static final int[] di = new int[] { -1, 0, 1, 0 };
	static final int[] dj = new int[] { 0, -1, 0, 1 };
	static int[][] map;
	static int[] wall_numbers;
	static int K, M, wall_index;

	public static void main(String[] args) throws Exception {
		init();
		StringBuilder sb = new StringBuilder();
		int total = 0;
		while (0 < K--) {
			// 탐사진행
			// 3*3 격자 선택
			if (goAdventure() == 0)
				break;
			// 유물획득
			total = 0;
			int count = getDiamond();
			while (0 < count) {
				total += count;
				fillMap();
				count = getDiamond();
			}

			sb.append(total).append(" ");
		}
		System.out.println(sb);

	}

	static void init() throws Exception {
		// System.setIn(new FileInputStream("src/input.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		K = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());
		map = new int[5][5];
		for (int i = 0; i < 5; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for (int j = 0; j < 5; j++) {
				map[i][j] = Integer.parseInt(st.nextToken());
			}
		}

		wall_numbers = new int[M];
		st = new StringTokenizer(br.readLine(), " ");
		for (int m = 0; m < M; m++) {
			wall_numbers[m] = Integer.parseInt(st.nextToken());
		}
		wall_index = 0;
	}

	static int goAdventure() {
		int target_i = 1, target_j = 1, target_d = 0;
		int max_diamond_count = 0;

		// 중심 축 (1), (2)가 같은 경우 열이 작고, 행이 작은 구간 우선
		for (int j = 1; j <= 3; j++) {
			for (int i = 1; i <= 3; i++) {
				for (int d = 0; d < 3; d++) {
					rotateMap(i, j);
					int diamond_count = countDiamond();
					if ((max_diamond_count < diamond_count) || (max_diamond_count == diamond_count && d < target_d)) {
						target_i = i;
						target_j = j;
						target_d = d;
						max_diamond_count = diamond_count;
					}
				}
				rotateMap(i, j);
			}
		}

		for (int d = 0; d <= target_d; d++) {
			rotateMap(target_i, target_j);
		}

		return max_diamond_count;
	}

	static void rotateMap(int target_i, int target_j) {
		int[][] temp = new int[3][3];
		for (int i = 0; i < 3; i++) {
			for (int j = 0; j < 3; j++) {
				temp[i][j] = map[i + target_i - 1][j + target_j - 1];
			}
		}

		for (int i = 0; i < 3; i++) {
			for (int j = 0; j < 3; j++) {
				map[i + target_i - 1][j + target_j - 1] = temp[3 - 1 - j][i];
			}
		}
	}

	static int countDiamond() {
		int total = 0;
		Queue<int[]> queue = new LinkedList<int[]>();
		boolean[][] visited = new boolean[5][5];
		for (int i = 0; i < 5; i++) {
			for (int j = 0; j < 5; j++) {
				if (!visited[i][j]) {
					int count = 1;
					queue.offer(new int[] { i, j });
					visited[i][j] = true;
					while (!queue.isEmpty()) {
						int[] node = queue.poll();
						for (int z = 0; z < 4; z++) {
							int ni = node[0] + di[z];
							int nj = node[1] + dj[z];
							if (check(ni, nj) && !visited[ni][nj] && map[i][j] == map[ni][nj]) {
								queue.offer(new int[] { ni, nj });
								visited[ni][nj] = true;
								count++;
							}
						}
					}
					if (3 <= count)
						total += count;
				}
			}
		}
		return total;
	}

	static int getDiamond() {
		int total = 0;
		Queue<int[]> queue = new LinkedList<int[]>();
		Queue<int[]> queue_save = new LinkedList<int[]>();
		boolean[][] visited = new boolean[5][5];
		for (int i = 0; i < 5; i++) {
			for (int j = 0; j < 5; j++) {
				if (!visited[i][j]) {
					queue.offer(new int[] { i, j });
					visited[i][j] = true;
					while (!queue.isEmpty()) {
						int[] node = queue.poll();
						queue_save.offer(node);
						for (int z = 0; z < 4; z++) {
							int ni = node[0] + di[z];
							int nj = node[1] + dj[z];
							if (check(ni, nj) && !visited[ni][nj] && map[i][j] == map[ni][nj]) {
								queue.offer(new int[] { ni, nj });
								visited[ni][nj] = true;
							}
						}
					}
					if (3 <= queue_save.size()) {
						total += queue_save.size();
						while (!queue_save.isEmpty()) {
							int[] node = queue_save.poll();
							map[node[0]][node[1]] = 0;
						}
					} else {
						queue_save.clear();
					}
				}
			}
		}
		return total;
	}

	static void fillMap() {
		for (int j = 0; j < 5; j++) {
			for (int i = 4; 0 <= i; i--) {
				if (map[i][j] == 0)
					map[i][j] = wall_numbers[wall_index++];
			}
		}
	}

	static boolean check(int i, int j) {
		return 0 <= i && i < 5 && 0 <= j && j < 5;
	}
}
```
