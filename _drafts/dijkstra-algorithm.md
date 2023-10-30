---
layout: post
title: "다익스트라 알고리즘"
date: 2023-10-30 15:15:00 +0900
categories: algorithm
---

```cpp
#include <bits/stdc++.h>

using namespace std;

#define MAX_V 20001

vector<pair<int, int>> graph[MAX_V]; // {to, distance}
int dist[MAX_V];

void dijkstra(int start) {
    priority_queue<pair<int, int>,
                   vector<pair<int, int>>,
                   greater<pair<int, int>>> pq; // {distance, to}

    pq.push({0, start});

    while(!pq.empty()) {
        int now = pq.top().second;
        pq.pop();
        for (auto next : graph[now]) {
            int next_node = next.first;
            int next_dist = next.second;
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
