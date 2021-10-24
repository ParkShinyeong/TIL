### BFS(Breadth-First-Search)

- **너비 탐색**: 루트 노드부터 시작해 인접 노드들을 먼저 탐색하는 것이다.
- **두 노드 사이의 최단 경로** 혹은 **임의의 경로**를 찾고 싶을 때 사용한다.
- 재귀적으로 동작하지 않는다. (계속 인접 노드들을 향해서 나아가기만 함)
  - 어떤 노드를 탐색했는지 여부를 검사해야 한다.
- 방문한 노드를 차례로 저장하고 꺼내기 위해 **queue**를 사용한다.
- 인접 리스트로 표현된 경우 O(N), 인접 행렬로 표현되었을 경우 O(N^2)의 시간 복잡도를 가진다.
- 깊이 우선 탐색(DFS)과 마찬가지로 그래프 내에 적은 숫자의 간선만을 가지는 희소 그래프(Sparse Graph) 의 경우 인접 행렬보다 인접 리스트를 사용하는 것이 유리하다.

  ##

### DFS(Depth-First-Search)

- **깊이 우선 탐색** : 루트 노드부터 시작해, 다음 분기로 넘어가기 전에 해당 분기를 모두 탐색하는 것
- 한 방향으로 계속 가다가 더 갈 수 없게 되면, 다시 가까운 갈림길로 돌아와서 탐색하는 방법
- **모든 노드를 방문**하고 싶을 때 선택한다.
- 재귀적으로 동작한다. (자기 자신을 계속 호출한다.)
  - 전위 순회같은 트리 순회들은 모두 DFS의 한 종류
- 구현 방법은 2가지로 순환 호출을 사용하거나, 스택을 사용하여 정점을 스택에 저장했다가 꺼내서 작업한다.
- 인접 리스트로 표현된 경우 O(N), 인접 행렬로 표현되었을 경우 O(N^2)의 시간 복잡도를 가진다.

참고
[https://howtolivelikehuman.tistory.com/75](https://howtolivelikehuman.tistory.com/75)
[https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html](https://gmlwjd9405.github.io/2018/08/15/algorithm-bfs.html)
[https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html](https://gmlwjd9405.github.io/2018/08/14/algorithm-dfs.html)
