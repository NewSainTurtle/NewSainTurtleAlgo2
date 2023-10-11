## [💣](https://www.codetree.ai/training-field/frequent-problems/problems/destroy-the-turret/submissions) [ct] 포탑 부수기

> **소요 시간: 92분<br>
> 메모리: 19MB<br>
> 시간: 212ms**

## 문제 접근

* bfs, List, 정렬을 이용함

## 전체 코드

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.Collections;
import java.util.LinkedList;
import java.util.List;
import java.util.Queue;
import java.util.StringTokenizer;

public class Main {
	static int N, M, K;
	static Node[][] map;
	static List<Node> list;

	public static void main(String[] args) throws Exception {
		init();
		for (int k = 1; k <= K; k++) {
			if (list.size() == 1)
				break;

			list.get(0).attackTime = k;
			list.get(0).power += N + M;
			list.get(0).check = k;

			final Node weak = list.get(0);
			final Node strong = list.get(list.size() - 1);

			if (!laserAttack(weak.power / 2, weak.i, weak.j, strong.i, strong.j, k)) {
				topAttack(weak.power / 2, weak.i, weak.j, strong.i, strong.j, k);
			}
			strong.power -= weak.power;
			strong.check = k;
			
			sortList(k);
		}
		System.out.println(list.get(list.size() - 1).power);
	}

	static void sortList(int k) {
		list = new LinkedList<>();
		for (int i = 0; i < N; i++) {
			for (int j = 0; j < M; j++) {
				if (map[i][j].power > 0) {
					list.add(map[i][j]);
					if(map[i][j].check != k) map[i][j].power++;
				}
			}
		}
		Collections.sort(list);
	}

	static boolean laserAttack(int P, int weakI, int weakJ, int strongI, int strongJ, int k) {
		final int[] di = { 0, 1, 0, -1 };
		final int[] dj = { 1, 0, -1, 0 };

		Queue<int[]> queue = new LinkedList<>();
		boolean[][] visited = new boolean[N][M];

		visited[weakI][weakJ] = true;
		queue.add(new int[] { weakI, weakJ });

		while (!queue.isEmpty()) {
			int[] ijRoute = queue.poll();
			for (int z = 0; z < 4; z++) {
				int ni = checkIdx(ijRoute[0] + di[z], N);
				int nj = checkIdx(ijRoute[1] + dj[z], M);
				if (map[ni][nj].power > 0 && !visited[ni][nj]) {
					if (ni == strongI && nj == strongJ) {
						for (int t = 2; t < ijRoute.length; t += 2) {
							map[ijRoute[t]][ijRoute[t + 1]].power -= P;
							map[ijRoute[t]][ijRoute[t + 1]].check = k;
						}
						return true;
					} else {
						visited[ni][nj] = true;
						int[] temp = new int[ijRoute.length + 2];
						temp[0] = ni;
						temp[1] = nj;
						for (int t = 2; t < ijRoute.length; t++) {
							temp[t] = ijRoute[t];
						}
						temp[ijRoute.length] = ni;
						temp[ijRoute.length + 1] = nj;
						queue.add(temp);
					}
				}
			}

		}
		return false;
	}

	static void topAttack(int P, int weakI, int weakJ, int strongI, int strongJ, int k) {
		final int[] di = { -1, -1, -1, 0, 0, 1, 1, 1 };
		final int[] dj = { -1, 0, 1, -1, 1, -1, 0, 1 };
		for (int z = 0; z < 8; z++) {
			int ni = checkIdx(strongI + di[z], N);
			int nj = checkIdx(strongJ + dj[z], M);
			if (ni != weakI || nj != weakJ) {
				map[ni][nj].power -= P;
				map[ni][nj].check = k;
			}
		}
	}

	static int checkIdx(int idx, int size) {
		if (idx == size)
			return 0;
		if (idx == -1)
			return size - 1;
		return idx;
	}

	static class Node implements Comparable<Node> {
		int i, j, power, attackTime, check;

		public Node(int i, int j, int power) {
			super();
			this.i = i;
			this.j = j;
			this.power = power;
		}

		@Override
		public int compareTo(Node o) {
			return power == o.power
					? (attackTime == o.attackTime ? (i + j == o.i + o.j ? o.j - j : (o.i + o.j) - (i + j))
							: o.attackTime - attackTime)
					: power - o.power;
		}

	}

	static void init() throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		StringTokenizer st = new StringTokenizer(br.readLine(), " ");

		N = Integer.parseInt(st.nextToken());
		M = Integer.parseInt(st.nextToken());
		K = Integer.parseInt(st.nextToken());

		map = new Node[N][M];
		for (int i = 0; i < N; i++) {
			st = new StringTokenizer(br.readLine(), " ");
			for (int j = 0; j < M; j++) {
				map[i][j] = new Node(i, j, Integer.parseInt(st.nextToken()));
			}
		}
		sortList(0);
	}

}
```
