---
title: 다익스트라 알고리즘 (Dijkstra's Algorithm)
layout: post
date: 2023-10-30 15:15:00 +0900
categories: algorithm
---
## 개요

다익스트라 알고리즘은 가중치가 있는 그래프의 한 정점에서 다른 모든 정점까지의 최단 거리를 구하는 알고리즘이다.

단 음의 간선이 포함된 경우 사용할 수 없다(벨만-포드 알고리즘을 사용해야 한다).

이전 최단거리들을 이용해 다음 최단거리를 구한다는 점에서 다이나믹 프로그래밍이라고 할 수 있다.

## 알고리즘

### 기본 알고리즘

1. 모든 정점을 방문하지 않음으로 초기화한다.

2. 시작점을 제외한 모든 정점까지의 거리를 무한으로 초기화한다.

3. 시작점을 현재 노드로 설정한다.

4. 현재 노드의 이웃 노드 중 방문하지 않은 노드에 대하여 새로운 거리(현재 노드의 최단거리 + 이웃 노드로의 거리)를 계산한다.

5. 새로운 거리와 기존 거리(이웃 노드의 현재 최단거리)를 비교하여 더 작은 거리를 이웃 노드의 최단거리로 업데이트한다.

6. 모든 이웃 노드를 방문하였다면 현재 노드를 방문됨으로 표시한다.

7. 방문하지 않은 노드 중 최단거리가 가장 짧은 노드를 현재 노드로 설정한다.

8. 더 이상 방문할 수 있는 노드가 없을 때 까지 4단계로 돌아가서 반복한다.

### 개선된 알고리즘

우선순위 큐를 사용하여 알고리즘을 개선할 수 있다.

기본 알고리즘의 7단계에서 최단거리가 가장 짧은 노드를 선형적으로 찾는다면 전체 시간 복잡도는 O(N^2)이 된다.

이것을 우선순위 큐를 이용하여 관리한다면 O(NlogN)으로 줄일 수 있다.

또한 업데이트된 노드를 큐에 삽입하는 방식으로 구현한다면 이웃 탐색을 마친 노드는 항상 큐에서 제외된다는 성질로 인해 방문 여부를 따로 체크하지 않아도 된다.

## 시간 복잡도

-   기본 알고리즘: O(N^2)
-   우선순위 큐를 이용: O(NlogN)

## 구현 예시 (C++)

[백준 1753번 문제](https://www.acmicpc.net/problem/1753)에 대한 제출 코드이다.

참고로 `cin`, `cout`을 사용하면 시간초과가 된다. `ios::sync_with_stdio(false); cin.tie(NULL);`을 추가하거나 `scanf, printf`를 사용해야 한다.

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_V 20001

vector<pair<int, int>> graph[MAX_V]; // {to, distance}
int dist[MAX_V]; // 시작점으로부터 최단거리

void dijkstra(int start) {
    priority_queue<pair<int, int>, // {distance, to}
                   vector<pair<int, int>>,
                   greater<pair<int, int>>> pq;

    pq.push({0, start});

    while(!pq.empty()) {
        int now = pq.top().second; // 현재 노드
        pq.pop();
        for (auto next : graph[now]) {
            int next_node = next.first; // 이웃 노드
            int next_dist = next.second; // 이웃 노드로의 거리
            // 기존 최단거리보다 새로운 최단거리가 더 짧으면 업데이트
            if (dist[next_node] > dist[now] + next_dist) {
                dist[next_node] = dist[now] + next_dist;
                pq.push({dist[next_node], next_node});
            }
        }
    }
}

int main() {
    // 정점 수, 간선 수, 시작점 입력 받기
    int v, e, k;
    scanf("%d %d %d", &v, &e, &k);

    // 그래프 입력 받기
    int a, b, w;
    for (int i = 0; i < e; i++) {
        scanf("%d %d %d", &a, &b, &w);
        graph[a].push_back({b, w});
    }

    //  거리 초기화
    for (int i = 1; i <= v; i++) {
        dist[i] = INT_MAX;
    }
    dist[k] = 0;

    // 다익스트라 알고리즘 실행
    dijkstra(k);

    // 결과 출력
    for (int i = 1; i <= v; i++) {
        if (dist[i] < INT_MAX)
            printf("%d\n", dist[i]);
        else
            printf("INF\n");
    }

    return 0;
}
```
