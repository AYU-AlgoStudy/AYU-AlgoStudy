## 4주차 1번 문제 BOJ 11657번
🔗| 문제 Link

https://www.acmicpc.net/problem/11657

### 문제 풀이
https://github.com/zldzldzz/Algorithm/commit/a1938d813bc00c5ea5202a2dba1f3accd49c87ee

### 개념 정리

**문제 풀이 과정에서 개선점 & 배운점**

    벨만-포드(Bellman-Ford) 알고리즘을 전혀 몰랐다. 때문에 음수 사이클 유무에 대해 전혀 고려하지 못해서 오랜 시간을 소모 했다.
    다익스트라 알고리즘도 모르는 상태 였기 때문에 더욱 어려웠다.

해당 문제 풀이 설명
    
>    간선의 정보를 바탕으로 한번 node[]를 저장하고 한번더 간선을 기준으로 반복하여 변경점이 있는 경우 음수 사이클 존재

    # 각 변수에 대한 설명    
    node[] : 시작점으로 부터 해당 배열까지의 거리를 저장
    list: 간선의 정보를 저장
    isCycle: 음수 사이클 판별 변수

**벨만-포드(Bellman-Ford) 알고리즘**

> 언제 쓰나?
> - 간선 가중치에 음수가 포함
> - 음수 사이클이 있는지까지 확인 필요

절차(실행 단계)
```
function BellmanFord(N, edges, source):
    # 1) 초기화: 시작점만 0, 나머지는 INF
    dist[1..N]   = INF
    parent[1..N] = NIL
    dist[source] = 0

    # 2) 종료 조건에 도달했는가? (모든 최단 경로는 간선 최대 N-1개)
    #    → N-1번 라운드 동안 완화를 시도
    for round in 1 .. N-1:

        updated = false

        # 3) 선택지(간선)들을 순회
        for each (u, v, w) in edges:

            # 4) 제약/유망성 검사: u에 아직 도달하지 못했다면 스킵
            if dist[u] == INF:
                continue

            # 5-1) 선택(완화 후보 계산)
            cand = dist[u] + w

            # 5-2) 탐색(더 “짧은” 경로인가?)
            if cand < dist[v]:
                # 5-3) 선택 확정(갱신)
                dist[v]   = cand
                parent[v] = u
                updated   = true

        # 6) 선택 취소/조기 종료: 이 라운드에서 아무 갱신도 없다면 수렴
        if updated == false:
            break

    # 7) 음수 사이클 탐지(추가 1회 완화로 더 줄어드는 정점이 있는가?)
    cycleEntry = NIL
    for each (u, v, w) in edges:
        if dist[u] != INF and dist[u] + w < dist[v]:
            cycleEntry = v
            parent[v]  = u      # 추적용으로 마지막 부모 갱신
            break

    if cycleEntry == NIL:
        # 8-A) 종료 처리: 최단거리 확정
        return (dist, parent, hasNegativeCycle = false)

    else:
        # 8-B) (선택) 음수 사이클 복원
        #     사이클 내부 정점으로 들어가기 위해 N번 parent를 거슬러 올라감
        x = cycleEntry
        repeat N times:
            x = parent[x]

        cycle = empty list
        cur = x
        do:
            cycle.push_back(cur)
            cur = parent[cur]
        while cur != x and cur != NIL

        # 보기 좋게 순서 정리(선택)
        reverse(cycle)

        return (dist, parent, hasNegativeCycle = true, cycle)

```
    

### 총평
벨만-포드(Bellman-Ford) 알고리즘을 사용할 수 있는 기회가 되었다.
아직 능숙하게 사용하는 것이 아니라 더 많은 문제를 풀어야 할 것 같다.

---

## 4주차 2번 문제 BOJ 11403번
🔗| 문제 Link

https://www.acmicpc.net/problem/11403

### 문제 풀이
https://github.com/zldzldzz/Algorithm/commit/0274a7a91f45266e003eef309452a259f4fcc4c4

### 해당 문제 풀이 설명

    가장 상위의 행부터 처리하는 방식으로 간다. 만약 해당 행의 1인 값을 찾으면 해당 열값의 행을 탐섹히여 1인 
    곳을 탐색하고 해당 행을 검섹하는 과정을 반복한다.

**문제 풀이 과정에서 개선점 & 배운점**

    위에 같은 문제를 분석하고 코드 작성을 완성했지만 seen을 추가하는 것을 못해서 일부 코드에서는 무한의 반복을
    때문에 런타임 에러를 직면했다. seen은 이미 확인한 행을 체크하여 이미 처리한 행을 반복하여 처리하여
    또한 현재 코드에서는 스택을 통해서 직렬로 변경해야 하는 곳을 찾았지만 큐 혹은 디큐와 같은 자료형에 상관 없이
    가능하다고 생각한다. 이유는 단순히 “row에서 start를 통해 도달 가능한 모든 정점”을 찾는 도달성 탐색이기 때문이다.


### 총평
단순히 경로 찾는 문제라고 생각하고 코드를 작성했지만 런타임 에러를 만나서 오랜 시간을 소모해 해결했다.

---

## 4주차 3번 문제 BOJ 1753번
🔗| 문제 Link

https://www.acmicpc.net/problem/1753

### 문제 풀이
https://github.com/zldzldzz/Algorithm/commit/38ae3695b56d1ffb138292542e1a362551b84591
### 해당 문제 풀이 설명

**문제 풀이 과정에서 개선점 & 배운점**

    이전 문제 였던 11657의 방식을 통해 문제를 해결하려고 시도 했지만 시간 초과를 3번 겪고 나서 다른 문제점이 있다고 알게 되었다.
    문제를 해결하기 위해서는 우선 순위큐를 사용하는 것이었다. 먼저 음수의 간선이 없으므로 벨만-포드 알고리즘을 통한 문제 해결은 너무 오랜 시간이
    걸린다. 정점이나 다리 수도 적은 수가 아니기 때문에 BFS 같은 탐색 방법도 비효율인 것이다.
    문제의 핵심은 시작 정점으로 부터 가장 가까운 정점으로 부터 범위를 넓히기 위해 우선 순위큐를 통해 가장 가까운 정점을 찾고
    해당 정점부터 처리하는 방싣이다.

### 총평
우선 순위큐를 익숙하지 않아서 단순 활용부터 아직은 언제 써야 하는지 잘 떠 오르지는 않지만 사용 방법이 약간은 친숙해 진것 같다.

---

## 4주차 4번 PGS 모음 사전 문제 
🔗| 문제 Link

https://www.acmicpc.net/problem/1753

### 문제 풀이
https://github.com/zldzldzz/Algorithm/commit/43a64bad6ad2880f18d19797ae3aa54661d1fc28

### 해당 문제 풀이 설명

**문제 풀이 과정에서 개선점 & 배운점**

    정말 바보 같지만 나는 헤당 문제를 해결하지 못하고 정답을 확인 했다.
    사전식 순서라는 것 자체와 dfs방식이라는 것을 생각하지도 못했기 때문이다.
    왜 글자수가 줄었다. 늘었다. 하는 거지? 이 순서는 도대체 무슨 순서인지 이해 조차 못했다.
    하지만 찾아보니 정말 많은 곳에서 사용하는 정렬 방식이었다..
    dfs 방식으로 단어를 조합하고 문제에서 나오는 단어와 같은지 확인하는 방식으로 구현했다.
    

### 총평
지금까지 dfs를 그렇게 많이 만들고 해당 순서에 대해서 생각 조차 하지 못한다는 것을 느낀 하루였다.

---

## 4주차 5번 문제 BOJ 17073번
🔗| 문제 Link

https://www.acmicpc.net/problem/17073

### 문제 풀이
https://github.com/zldzldzz/Algorithm/commit/f51a8a46236f687e920ea29efa438f913cfffdf1

### 해당 문제 풀이 설명

**문제 풀이 과정에서 개선점 & 배운점**

    이전 문제에 이어서 풀고 나서 오랜시간 충격을 받은 문제이다. 트리, 확률이라는 설명을 읽으면서 도저히 어떤 식으로
    문제를 해결해야 하는지 전혀 생각하지 못하고 오랜 시간 고민했지만 시작 조차 할 수 없었다.
    같이 문제를 푸는 친구에게 도움을 요청해 전체 자식 노드의 수로 나누라는 말을 듣고 충격에 말을 할 수 없었다.

### 총평
이런 문제도 못 풀고 틀리는데 나는 언제 코딩 테스트 시험을 볼 수 있을지 모르겠다.

---
## 4주차 6번 문제 PGS 가장 큰 수
🔗| 문제 Link

https://school.programmers.co.kr/learn/courses/30/lessons/42746

### 문제 풀이
https://github.com/zldzldzz/Algorithm/commit/a87aa843cdf6f1ecd2917cf6eeb04db8fbe461a7

### 해당 문제 풀이 설명

**문제 풀이 과정에서 개선점 & 배운점**

    지금까지 많은 문제를 만나서 그런지 compar가 필요하다는 것을 빠르게 파악할 수 있었다.
    하지만 처음에는 자리수로 나눠서 계산하기 위해서 수많은 시도를 했지만 모두 실패하고 String형을 통해 사전식을 
    사용하면 해결 할 수 있다는 것을 알게 되어 문제를 해결 할 수 있었다.
    마지막에는 0이 주어졌을 때 예외처리 안해서 마지막까지 틀렸다. 

### 총평
이것도 경험이겠지.. 하다보면 늘겠지

---
## 4주차 7번 문제 BOJ 1463
🔗| 문제 Link

https://www.acmicpc.net/problem/1463

### 문제 풀이
https://github.com/zldzldzz/Algorithm/commit/8cc3ad5561ae8cbe25df4ce7aa7640e988c0c869

### 해당 문제 풀이 설명

**문제 풀이 과정에서 개선점 & 배운점**

    DP를 사용한다고 해서 겁을 먹었지만 생각했던 것 보다 매우 빠른 시간에 해결 할 수 있었다.
    DP란 큰 문제를 해결하기 위해 작은 단위의 문제로 나눠서 해결하는 방식이었다.
    ex) 피보나치 수열

### 총평
가볍다..?

---

