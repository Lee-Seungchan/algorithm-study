#### DFS(Depth First Search)

- 깊이우선탐색, 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘
- 모든 정점을 발견하는 가장 단순하고 고전적인 방법이다.
- 현재 정점과 인접한 간선들을 하나씩 검사하다가, 아직 방문하지 않은 정점으로 향하는 간선이 있다면 그 간선을 무조건 따라간다. 더이상 갈 곳이 없는 막힌 정점에 도달하면 포기하고, 마지막에 따라왔던 간선을 뒤돌아간다.<br>

#### 깊이우선탐색 특징

- 트리 순회(전위, 중위, 후위 순회)는 DFS의 한 종류
- 그래프 탐색의 경우 어떤 노드를 방문했었는지 반드시 검사(무한루프 방지)<br>

#### 깊이우선탐색 장점

- 저장공간의 수요가 적다.
- 목표노드가 깊은 단계에 있을 경우 답을 빨리 구할 수 있다.<br>

#### 깊이우선탐색 단점

- 답이 해당 경로에 없어도 깊숙히 빠질 가능성이 있다.
- 최단 경로(최적)가 된다는 보장이 없다.<br>

#### 1. 인접 리스트로 구현한 DFS

```java

import java.util.*;

public class DFS_List {
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

		for (int i = 1; i <= n; i++) { // 방문 순서를 위해 오름차순 정렬
			Collections.sort(adjList[i]);
		}

		System.out.println("DFS - 인접리스트");
		dfs_list(v, adjList, visited);
	}

	// DFS - 인접리스트 - 재귀로 구현
	public static void dfs_list(int v, LinkedList<Integer>[] adjList, boolean[] visited) {
		visited[v] = true; // 정점 방문 표시
		System.out.print(v + " "); // 정점 출력

		Iterator<Integer> iter = adjList[v].listIterator(); // 정점 인접리스트 순회
		while (iter.hasNext()) {
			int w = iter.next();
			if (!visited[w]) // 방문하지 않은 정점이라면
				dfs_list(w, adjList, visited); // 다시 DFS
		}
	}

}
```

#### 입력

<img src="C:\Users\이승찬\Desktop\개인 공부자료\01 algorithm\image\dfs.png" width="380px" height="200px">

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
DFS - 인접리스트
3 1 2 5 4
```

<br>
#### 2. 인접 행렬로 구현한 DFS

```java
import java.util.*;

public class DFS_Array {
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

		System.out.println("DFS - 인접행렬 / 재귀로 구현");
		dfs_array_recursion(v, adjArray, visited);
		Arrays.fill(visited, false); // 스택 DFS를 위해 visited 배열 초기화

		System.out.println("\n\nDFS - 인접행렬 / 스택으로 구현");
		dfs_array_stack(v, adjArray, visited, true);
	}

	//DFS - 인접행렬 / 재귀로 구현
	public static void dfs_array_recursion(int v, int[][] adjArray, boolean[] visited) {
		int l = adjArray.length-1;
		visited[v] = true;
		System.out.print(v + " ");

		for(int i = 1; i <= l; i++) {
			if(adjArray[v][i] == 1 && !visited[i]) {
				dfs_array_recursion(i, adjArray, visited);
			}
		}
	}

	//DFS - 인접행렬 / 스택으로 구현
	public static void dfs_array_stack(int v, int[][] adjArray, boolean[] visited, boolean flag) {
		int l = adjArray.length-1;
		Stack<Integer> stack = new Stack<Integer>();
		stack.push(v);
		visited[v] = true;
		System.out.print(v + " ");

		while(!stack.isEmpty()) {
			int w = stack.peek();
			flag = false;

			for(int i = 1; i <= l; i++) {
				if(adjArray[w][i] == 1 && !visited[i]) {
					stack.push(i);
					System.out.print(i + " ");
					visited[i] = true;
					flag = true;

					break;
				}
			}

			if(!flag) {
				stack.pop();
			}
		}
	}

}
```

#### 입력

<img src="C:\Users\이승찬\Desktop\개인 공부자료\01 algorithm\image\dfs2.png" width="380px" height="200px">

```java
4 5 1
1 2
1 3
1 4
2 4
3 4
```

#### 출력

```java
DFS - 인접행렬 / 재귀로 구현
1 2 4 3
DFS - 인접행렬 / 스택으로 구현
1 2 4 3
```

<br>

#### DFS 시간 복잡도

정점의 수가 n이고, 간선의 수가 e인 그래프의 경우

- 그래프가 인접 리스트로 표현된 경우 O(n+e)
- 인접 행렬로 표현된 경우 O(n^2)
