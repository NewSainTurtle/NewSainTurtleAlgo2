## [👻](https://www.codetree.ai/training-field/frequent-problems/problems/pacman/description) [ct] 팩맨

> **소요 시간: 110분<br>
> 메모리: 8MB<br>
> 시간: 104ms**

## 리뷰
- 문제가 길어서 복잡했다,, 4시간 주어지니까 차근차근 풀자!
- 방향을 가지고 있는 몬스터들을 한마리씩 저장하면 메모리가 부족할 것이므로,, 같은 방향의 몬스터들을 한번에 저장했다

## 문제 풀이
1. `pacman_i-j`: 팩맨의 위치를 저장한다.
2. `map`: 각 위치에서 각 방향에 위치하는 몬스터들을 저장한다.
3. `egg`: 각 위치에서 각 방향에 위치하는 알들을 저장한다.
4. `deadMonsters`: 각 위치에서 죽은 몬스터들 수를 저장한다. 0: 이번에 죽음, 1: 1회차, 2: 2회차
5. `init`함수를 통해 입력을 받는다.
6. `makeEgg`함수를 통해 몬스터 복제 시도를 한다.`map`을 완전탐색하며, map의 몬스터 수 만큼 `egg`에 저장하여 복제를 시도한다.
7. `moveMonster`함수를 통해 몬스터를 이동시킨다.
 1. `map`과 같은 크기의 `temp`를 정의하여 이동한 몬스터를 임시로 저장한다.
 2. `map`을 완전탐색하며, 몬스터가 있는 경우
 3. 해당 몬스터의 방향만큼 한칸 이동한다. 이때 그 위치가 범위를 벗어났거나, 팩맨의 위치거나 죽은 몬스터들이 있는 경우 방향을 45도 변경하며 이동할 수 있는 위치를 찾는다.
 4. 몬스터를 이동시킨다.(만약 방향이 그대로라면 이동할 수 없다는 의미로 이동하지 않는다.)
 5. 몬스터의 위치에 맞게 `temp`에 몬스터 수를 더한다.
 6. 완전 탐색이 끝나면 `map`에 `temp`를 저장한다.
8. `movePacman`함수를 통해 팩맨을 이동시킨다.
 1. 팩맨은 3번 이동해야하므로 3중 for문을 활용해 팩맨이 이동할 위치를 확인한다.
 2. 팩맨이 3번 이동하는 동안 해당 위치들이 범위를 벗어나지 않는지 확인한다.
 3. 팩맨이 3번 이동하는 동안 잡아먹을 수 있는 몬스터 수를 체크한다. (이때, 첫번째 이동과 세번째 이동의 위치가 같을 수 있는데 그 경우 몬스터를 중복으로 체크하지 않도록 주의한다.)
 4. 팩맨이 제일 많은 몬스터를 잡아먹을 수 있는 위치를 찾고, 완전탐색이 끝난 후 팩맨을 이동시킨다.
 5. 팩맨이 이동하면서 그 위치의 몬스터들은 `deadMonsters` 해당 위치의 0번째에 몬스터 수를 더한다. `map`에서는 죽은 몬스터 자리를 0으로 초기화한다. 
9. `dieMonster`함수를 통해 2회차 몬스터 시체를 소멸한다.
 1. 완전 탐색을 통해 `deadMonsters` 2회차 자리에 1회차 값을 저장하고, 1회차 자리에 이번 턴에 죽은 몬스터 값을 저장한다. 0번째 자리는 0으로 초기화한다.
10. `makeMonster`함수를 통해 몬스터 복제를 완성한다.
 1. 완전 탐색을 통해 `egg`에 값들을 같은 위치의 `map`에 더해서 알을 부화시킨다.
11. `T`번의 턴동안 (6~10)번을 반복한다.
12. 모든 턴이 종료되면 `countMonster`함수를 통해 살아남은 몬스터 수를 완전 탐색으로 `map`의 값을 모두 더한 후 이 값을 출력한다.

## 전체 코드
```java
import java.io.*;
import java.util.*;

public class Main_팩맨 {
	static final int[] di = new int[] { -1, -1, 0, 1, 1, 1, 0, -1 };
	static final int[] dj = new int[] { 0, -1, -1, -1, 0, 1, 1, 1 };
	static int pacman_i, pacman_j;
	static int[][][] map;
	static int[][][] egg;
	static int[][][] deadMonsters;
	static int T;

	public static void main(String[] args) throws Exception {
		init();
		while (0 < T--) { // ↑, ↖, ←, ↙, ↓, ↘, →, ↗
			// 몬스터 복제 시도
			makeEgg();
			// 몬스터 이동
			moveMonster();
			// 팩맨 이동
			movePacman();
			// 몬스터 시체 소멸
			dieMonster();
			// 몬스터 복제 완성
			makeMonster();
		}
		System.out.println(countMonster());

	}

	static void init() throws Exception {
		//System.setIn(new FileInputStream("src/input.txt"));
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");
		int M = Integer.parseInt(st.nextToken());
		T = Integer.parseInt(st.nextToken());

		st = new StringTokenizer(br.readLine(), " ");
		pacman_i = Integer.parseInt(st.nextToken()) - 1;
		pacman_j = Integer.parseInt(st.nextToken()) - 1;

		map = new int[4][4][8];
		egg = new int[4][4][8];
		deadMonsters = new int[4][4][3];

		for (int m = 0; m < M; m++) {
			st = new StringTokenizer(br.readLine(), " ");
			int i = Integer.parseInt(st.nextToken()) - 1;
			int j = Integer.parseInt(st.nextToken()) - 1;
			int d = Integer.parseInt(st.nextToken()) - 1;
			map[i][j][d]++;
		}

	}

	static void makeEgg() {
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				for (int k = 0; k < 8; k++) {
					egg[i][j][k] = map[i][j][k];
				}
			}
		}

	}

	static void moveMonster() {
		int[][][] temp = new int[4][4][8];
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				for (int k = 0; k < 8; k++) {
					if (0 < map[i][j][k]) {
						int nd = k;
						int ni = i + di[nd];
						int nj = j + dj[nd];

						while (!check(ni, nj) || (pacman_i == ni && pacman_j == nj) || 0 < deadMonsters[ni][nj][1]
								|| 0 < deadMonsters[ni][nj][2]) {
							if (nd++ == 7)
								nd = 0;
							ni = i + di[nd];
							nj = j + dj[nd];
							if (nd == k) {
								ni = i;
								nj = j;
								break;
							}
						}

						temp[ni][nj][nd] += map[i][j][k];
					}
				}
			}
		}
		map = temp;
	}

	static void movePacman() {
		int max = -1;
		int[][] seq = new int[3][2];
		for (int i = 0; i < 7; i += 2) {
			int ni = pacman_i + di[i];
			int nj = pacman_j + dj[i];

			if (check(ni, nj)) {
				for (int j = 0; j < 7; j += 2) {
					int nni = ni + di[j];
					int nnj = nj + dj[j];
					if (check(nni, nnj)) {
						for (int k = 0; k < 7; k += 2) {
							int nnni = nni + di[k];
							int nnnj = nnj + dj[k];
							if (check(nnni, nnnj)) {
								int count = 0;
								for (int z = 0; z < 8; z++) {
									count += map[ni][nj][z];
									count += map[nni][nnj][z];
								}
								if (ni != nnni || nj != nnnj) {
									for (int z = 0; z < 8; z++) {
										count += map[nnni][nnnj][z];
									}
								}

								if (max < count) {
									
									max = count;
									seq[0] = new int[] { ni, nj };
									seq[1] = new int[] { nni, nnj };
									seq[2] = new int[] { nnni, nnnj };
								}
							}
						}
					}
				}

			}

		}

		for (int index = 0; index < 3; index++) {
			for (int k = 0; k < 8; k++) {
				deadMonsters[seq[index][0]][seq[index][1]][0] += map[seq[index][0]][seq[index][1]][k];
				map[seq[index][0]][seq[index][1]][k] = 0;
			}
		}
		pacman_i = seq[2][0];
		pacman_j = seq[2][1];

	}

	static void dieMonster() {
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				deadMonsters[i][j][2] = deadMonsters[i][j][1];
				deadMonsters[i][j][1] = deadMonsters[i][j][0];
				deadMonsters[i][j][0] = 0;
			}
		}
	}

	static void makeMonster() {
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				for (int k = 0; k < 8; k++) {
					map[i][j][k] += egg[i][j][k];
				}
			}
		}
	}

	static int countMonster() {
		int count = 0;
		for (int i = 0; i < 4; i++) {
			for (int j = 0; j < 4; j++) {
				for (int k = 0; k < 8; k++) {
					count += map[i][j][k];
				}
			}
		}

		return count;
	}

	static boolean check(int i, int j) {
		return 0 <= i && i < 4 && 0 <= j && j < 4;
	}
}
```
