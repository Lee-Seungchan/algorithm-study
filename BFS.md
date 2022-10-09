#### BFS(Breadth First Search)

- 너비우선탐색, 가까운 인접 노드부터 탐색하는 방법
- 재귀함수가 쓰이지않고 큐를 사용해서 현재 노드에서 인접해 있는 노드를 queue에 넣는 방식으로 작동한다.<br>

#### 너비우선탐색 특징

- BFS는 방문한 노드들을 저장한 후 차례로 꺼낼 수 있는 큐를 사용한다.(FIFO)
- 두 노드사이의 최단경로를 탐색할 때 활용한다.(멀리 떨어진 노드는 나중에 탐색하기 때문)<br>

#### 너비우선탐색 장점

- 최단경로를 찾음을 보장한다.
- 최단 경로가 존재하면 깊이가 무한정 깊어도 답을 찾을 수 있다.<br>

#### 너비우선탐색 단점

- 경로가 매우 길 경우에는 탐색 가지가 급격히 증가함에 따라 보다 많은 기억 공간을 필요로 하게 된다.
- 유한 그래프의 경우, 해가 존재하지 않으면 모든 그래프를 탐색한 후 실패로 끝난다.
- 무한 그래프의 경우에는 해를 찾지도 못하고, 끝내지도 못한다.<br>

#### 1. 인접 리스트로 구현한 BFS

```java
import java.util.*;

public class BFS_List {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int n = sc.nextInt(); // 정점의 개수
		int m = sc.nextInt(); // 간선의 개수
		int v = sc.nextInt(); // 탐색을 시작할 정점의 번호

		boolean visited[] = new boolean[n + 1]; // 방문 여부를 검사할 배열

		LinkedList<Integer>[] adjList = new LinkedList[n + 1];

		for (int i = 0; i <= n; i++) {
			adjList[i] = new LinkedList<Integer>();
		}

		// 두 정점 사이에 여러 개의 간선이 있을 수 있다.
		// 입력으로 주어지는 간선은 양방향이다.
		for (int i = 0; i < m; i++) {
			int v1 = sc.nextInt();
			int v2 = sc.nextInt();
			adjList[v1].add(v2);
			adjList[v2].add(v1);
		}

		for (int i = 1; i <= n; i++) {
			Collections.sort(adjList[i]); // 방문 순서를 위해 오름차순 정렬
		}

		System.out.println("BFS - 인접리스트");
		bfs_list(v, adjList, visited);
	}

	// BFS - 인접리스트
	public static void bfs_list(int v, LinkedList<Integer>[] adjList, boolean[] visited) {
		Queue<Integer> queue = new LinkedList<Integer>();
		visited[v] = true;
		queue.add(v);

		while(queue.size() != 0) {
			v = queue.poll();
			System.out.print(v + " ");

			Iterator<Integer> iter = adjList[v].listIterator();
			while(iter.hasNext()) {
				int w = iter.next();
				if(!visited[w]) {
					visited[w] = true;
					queue.add(w);
				}
			}
		}
	}

}
```

#### 입력

<img src="https://user-images.githubusercontent.com/103403612/194749803-0690dbea-509b-40f6-b26a-7919947d22a5.png" width="380px" height="200px">

```java
5 5 3
5 4
5 2
1 2
3 4
3 1
```

#### 출력

```java
BFS - 인접리스트
3 1 4 2 5
```

#### 2. 인접 행렬로 구현한 BFS

```java
import java.util.*;

public class BFS_Array {
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);

		int n = sc.nextInt(); // 정점의 개수
		int m = sc.nextInt(); // 간선의 개수
		int v = sc.nextInt(); // 탐색을 시작할 정점의 번호

		boolean visited[] = new boolean[n + 1]; // 방문 여부를 검사할 배열

		int[][] adjArray = new int[n+1][n+1];

		// 두 정점 사이에 여러 개의 간선이 있을 수 있다.
		// 입력으로 주어지는 간선은 양방향이다.
		for(int i = 0; i < m; i++) {
			int v1 = sc.nextInt();
			int v2 = sc.nextInt();

			adjArray[v1][v2] = 1;
			adjArray[v2][v1] = 1;
		}

		System.out.println("BFS - 인접행렬");
		bfs_array(v, adjArray, visited);
	}

	// BFS - 인접행렬
	public static void bfs_array(int v, int[][] adjArray, boolean[] visited) {
		Queue<Integer> q = new LinkedList<>();
		int n = adjArray.length - 1;

		q.add(v);
		visited[v] = true;

		while (!q.isEmpty()) {
			v = q.poll();
			System.out.print(v + " ");

			for (int i = 1; i <= n; i++) {
				if (adjArray[v][i] == 1 && !visited[i]) {
					q.add(i);
					visited[i] = true;
				}
			}
		}
	}

}
```

#### 입력

<img src="https://user-images.githubusercontent.com/103403612/194749842-9302c67a-4ce7-4158-b388-179e58986c83.png" width="380px" height="200px">

```java
4 5 1 3
1 2
1 3
1 4
2 4
3 4
```

#### 출력

```java
BFS - 인접행렬
1 2 3 4
```

<br>

#### BFS 시간복잡도

정점의 수가 n이고, 간선의 수가 e인 그래프의 경우

- 그래프가 인접 리스트로 표현된 경우 O(n+e)
- 인접 행렬로 표현된 경우 O(n^2)
