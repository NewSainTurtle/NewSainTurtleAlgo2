## [🎡](https://www.codetree.ai/training-field/frequent-problems/problems/go-on-the-rides/description) [ct] 놀이기구 탑승

> **소요 시간: 47분<br>
> 메모리: 10MB<br>
> 시간: 148ms**

## 문제 풀이
1. `student_infos`: 입력 순서대로 학생들의 번호 저장
2. `student_favorit_infos`: 학생들의 번호를 인덱스로 좋아하는 학생들 번호 저장
3. `map`: 놀이기구에 학생들이 탑승 위치를 나타내며, 외벽은 -1로 채워짐 (따로 유효범위 체크 안해도 됨)
4. `choose`함수를 호출해, 학생들을 놀이기구에 탑승시킨다.
5. 입력 순서대로 학생들을 불러온다.
6. 행(오름차순), 열(오름차순)으로 빈 공간을 찾아 `target` 위치의 초기값을 정한다.
7. `map`을 완전탐색하면서 인접한 곳에 좋아하는 학생(`favorite_count`)이 많은 빈칸을 확인한다.
	1. `favorite_count`이 `max_favorite_count`보다 큰 경우, `target` 위치 값을 갱신한다.
	2. `favorite_count`이 `max_favorite_count`같고 빈칸이 현재 위치가 더 적은 경우, `target` 위치 값을 갱신한다. 
8. `map`의 `target`위치에 학생번호를 저장한다.
9. 학생들을 모두 탑승시켰다면, `getScore`함수를 통해 점수를 계산하고 이를 출력한다.

## 전체 코드
```java
import java.util.*;
import java.io.*;

public class Main_놀이기구탑승 {
	static final int[] di = new int[] { 0, -1, 0, 1 };
	static final int[] dj = new int[] { -1, 0, 1, 0 };
	static int N;
	static int[][] map;
	static int[] student_infos;
	static int[][] student_favorit_infos;

	public static void main(String[] args) throws Exception {
		init();
		choose();
		System.out.println(getScore());

	}

	static void init() throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		N = Integer.parseInt(br.readLine().trim());
		map = new int[N + 2][N + 2];
		student_infos = new int[N * N];
		student_favorit_infos = new int[N * N + 1][4];

		for (int k = 0; k < N * N; k++) {
			StringTokenizer st = new StringTokenizer(br.readLine(), " ");
			// 학생 입력
			student_infos[k] = Integer.parseInt(st.nextToken());
			// 좋아하는 학생 입력
			student_favorit_infos[student_infos[k]][0] = Integer.parseInt(st.nextToken());
			student_favorit_infos[student_infos[k]][1] = Integer.parseInt(st.nextToken());
			student_favorit_infos[student_infos[k]][2] = Integer.parseInt(st.nextToken());
			student_favorit_infos[student_infos[k]][3] = Integer.parseInt(st.nextToken());
		}

		for (int n = 0; n <= N + 1; n++) {
			map[0][n] = map[N + 1][n] = map[n][0] = map[n][N + 1] = -1;
		}
	}

	static void choose() {
		for (int k = 0; k < N * N; k++) {
			int favorite_count = 0;
			int max_favorite_count = 0;
			int target_i = 1;
			int target_j = 1;

			exit: for (int i = 1; i <= N; i++) {
				for (int j = 1; j <= N; j++) {
					if (map[i][j] == 0) {
						target_i = i;
						target_j = j;
						break exit;
					}
				}
			}

			// step 1. 좋아하는 학생 많은 곳 체크
			for (int i = 1; i <= N; i++) {
				for (int j = 1; j <= N; j++) {
					if (map[i][j] != 0)
						continue;
					favorite_count = 0;
					for (int z = 0; z < 4; z++) {
						int ni = i + di[z];
						int nj = j + dj[z];

						for (int f = 0; f < 4; f++) {
							if (student_favorit_infos[student_infos[k]][f] == map[ni][nj]) {
								favorite_count++;
								break;
							}
						}
					}

					if (max_favorite_count < favorite_count) {
						max_favorite_count = favorite_count;
						target_i = i;
						target_j = j;
					} else if (max_favorite_count == favorite_count) {
						// step 2. 빈칸이 많은 곳 (빈칸도 같은 경우는 step 3,4에 의하여 target_i, target_j를 갱신 안시킴
						int a_blank = 0, b_blank = 0;
						for (int z = 0; z < 4; z++) {
							int ai = i + di[z];
							int aj = j + dj[z];
							if (map[ai][aj] == 0)
								a_blank++;
							int bi = target_i + di[z];
							int bj = target_j + dj[z];
							if (map[bi][bj] == 0)
								b_blank++;
						}
						if (b_blank < a_blank) {
							target_i = i;
							target_j = j;
						}
					}
				}
			}
			map[target_i][target_j] = student_infos[k];
		}
	}

	static int getScore() {
		int score = 0;
		int favorite_count;

		for (int i = 1; i <= N; i++) {
			for (int j = 1; j <= N; j++) {
				favorite_count = 0;
				for (int z = 0; z < 4; z++) {
					int ni = i + di[z];
					int nj = j + dj[z];
					for (int f = 0; f < 4; f++) {
						if (student_favorit_infos[map[i][j]][f] == map[ni][nj]) {
							favorite_count++;
							break;
						}
					}
				}

				score += Math.pow(10, favorite_count - 1);

			}
		}
		return score;
	}
}
```
