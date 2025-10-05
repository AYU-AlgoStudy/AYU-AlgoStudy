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

---

# 1주차 2번 문제 백준 1991번

https://www.acmicpc.net/problem/1991

## 풀이 코드:
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.OutputStreamWriter;

import com.sun.source.tree.Tree;

public class Main {

	public static class Tree {
		Node root;

		private StringBuilder sb = new StringBuilder();

		class Node {
			String data;
			Node leftNode, rightNode;

			Node(String data) {
				this.data = data;
				leftNode = null;
				rightNode = null;
			}
		}

		public String getPreOrderResult() {
			sb.setLength(0);
			preOrder(root);
			return sb.toString();
		}

		private void preOrder(Node node) {
			if (node == null)
				return;
			//방문 처리
			sb.append(node.data);
			preOrder(node.leftNode);
			preOrder(node.rightNode);
		}

		public String getInOrderResult() {
			sb.setLength(0);
			inOrder(root);
			return sb.toString();
		}

		private void inOrder(Node node) {
			if (node == null)
				return;
			inOrder(node.leftNode);
			sb.append(node.data);
			inOrder(node.rightNode);
		}

		public String getPostOrderResult() {
			sb.setLength(0);
			postOrder(root);
			return sb.toString();
		}

		private void postOrder(Node node) {
			if (node == null)
				return;
			postOrder(node.leftNode);
			postOrder(node.rightNode);
			sb.append(node.data);
		}

		public void addNode(String data, String left, String right) {

			//root에 아무것도 없는 경우
			if (this.root == null) {
				root = new Node(data);
				if (!left.equals(".")) {
					root.leftNode = new Node(left);
				}
				if (!right.equals(".")) {
					root.rightNode = new Node(right);
				}
			} else {
				search(root, data, left, right);
			}
		}

		public void search(Node node, String data, String left, String right) {

			//최하위 노드로 이동했지만 없는 경우
			if (node == null) {
				return;
			}
			//내가 찾는 노드를 찾은 경우 삽입
			if (node.data.equals(data)) {
				if (!left.equals(".")) {
					node.leftNode = new Node(left);
				}
				if (!right.equals(".")) {
					node.rightNode = new Node(right);
				}
				return;
			}
			// 현재 삽입하는 노드를 못 찾는 경우 자식 노드를 기준으로 다시 탐색
			else {
				search(node.leftNode, data, left, right);
				search(node.rightNode, data, left, right);
			}
		}

	}

	public static void main(String[] args) throws IOException {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(System.out));
		Tree tree = new Tree();
		Integer lenght = Integer.parseInt(br.readLine());
		for (int i = 0; i < lenght; i++) {
			String[] line = br.readLine().split(" ");
			tree.addNode(line[0], line[1], line[2]);
		}

		bw.write(tree.getPreOrderResult() + "\n");
		bw.write(tree.getInOrderResult() + "\n");
		bw.write(tree.getPostOrderResult() + "\n");

		bw.flush();
		bw.close();
		br.close();
	}
}
```
### 배운점:
트리 문제를 경험해 본적이 없어서 지금 실력에도 낮은 난이도의 트리 문제는 충분히 풀 수 있다고 생각했지만 아니라는 것을 깨달았다.
이번주의 DFS와 BFS를 풀고 다음주는 트리 위주로 학습이 필요할 것 같다.
이번과 동일하게 처음 풀어보는 문제였고 20분 정도 고민했지만 실행까지 너무 어려워서 아래 블로그를 먼저 읽고 코드를 다시 구현했다.
학교에서 이론적으로 배운 내용을 코드로 녹여낼 수 있는 실력은 아직 부족한 것 같다.
때문에 다양한 알고리즘 문제를 접해보고 나만의 문제 해결 패턴을 익히는 것이 중요하다고 깨달았다.

### 참고자료:

https://minhamina.tistory.com/80

---

# 1주차 3번 문제 프로그래머스 네트워크

https://school.programmers.co.kr/learn/courses/30/lessons/43162?language=java

# 1주차 4번 문제 백준 2606번

https://www.acmicpc.net/problem/2606

# 1주차 5번 문제 백준 2667번

https://www.acmicpc.net/problem/2667

### 배운점:
1주차의 3번에서 5번 문제는 내가 비교적으로 빠르게 해결할 수 있는 BFS방식을 사용했다.
때문에 별도의 참고자료가 없이 비교적 빠른 시간 내의 문제를 해결할 수 있었다.
왜? BFS로 빠르게 풀었어?
- BFS/DFS 중에 어떤 것이 더 효율적인지는 문제의 상황에 따라 다르다.
- 문제 사항에서 DFS가 효율적이지 않은데 익숙하지 않은  DFS를 사용하는 것은 비효율적이다.
- 기존의 BFS 방식으로 빠르게 문제를 해결 할 수 있는지 테스트 해보았다.

DFS를 필요로 하는 다른 문제들을 풀어보는 것이 상황에 맞는 곳에서 적절히 사용할 수 있다고 생각한다.