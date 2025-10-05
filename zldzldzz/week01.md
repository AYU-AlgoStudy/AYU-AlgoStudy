# 1주차 1번 문제 백준 1260번

https://www.acmicpc.net/problem/1260

## 풀이 코드:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;
import java.util.LinkedList;
import java.util.Queue;

public class Main {
	static int n, m, v;
	static int[][] map;
	static boolean[] dfsVisited, bfsVisited;

	public static void dfs(int v){
		System.out.print(String.valueOf(v)+" ");

		dfsVisited[v] = true;

		for(int i=1;i<=n;i++){
			if(1 == map[v][i]&&!dfsVisited[i]){
				dfs(i);
			}
		}
	}

	public static void bfs(int v){
		Queue<Integer> q = new LinkedList<>();
		q.add(v);
		bfsVisited[v] = true;
		while(!q.isEmpty()){
			int u = q.poll();
			System.out.print(String.valueOf(u)+" ");
			for(int i=1;i<=n;i++){
				if(1 == map[u][i]&&!bfsVisited[i]){
					bfsVisited[i] = true;
					q.add(i);
				}
			}
		}
	}

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

		String[] nmv = br.readLine().split(" ");
		n = Integer.parseInt(nmv[0]);
		m = Integer.parseInt(nmv[1]);
		v  = Integer.parseInt(nmv[2]);
		map = new int[n+1][n+1];

		for (int i = 0; i < m; i++) {
			String[] line = br.readLine().split(" ");
			map[Integer.parseInt(line[0])][Integer.parseInt(line[1])]=1;
			map[Integer.parseInt(line[1])][Integer.parseInt(line[0])]=1;
		}
		dfsVisited = new boolean[n+1];
		bfsVisited = new boolean[n+1];

		dfs(v);
		System.out.println();
		bfs(v);


		br.close();
	}
}
```
### 배운점:

지금까지 탐색 코드를 모두 BFS를 풀다가 DFS를 사용해 보려고 하니 개념은 이해했지만 구현을 하는 부분에서 감조차 오지 않았다.
그래서 파이썬으로 되어있는 코드를 먼저 보면서 이해하고 자바 코드로 구현했다.
단순한 구현이라서 어렵지는 않았지만 dfs로 구현해야 하는 다른 문제도 풀어볼 것이다.

### 참고자료:
https://velog.io/@falling_star3/%EB%B0%B1%EC%A4%80Python-1260%EB%B2%88-DFS%EC%99%80BFS