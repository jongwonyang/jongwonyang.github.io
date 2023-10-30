---
layout: post
title: "다익스트라 알고리즘"
date: 2023-10-30 15:15:00 +0900
categories: algorithm
---

```cpp
#include <bits/stdc++.h>

using namespace std;

int main() {
    int v, e, k;
    scanf("%d %d %d", &v, &e, &k);

    int distance[v + 1];
    for (int i = 1; i <= v; i++) {
        distance[i] = INT_MAX;
    }
    distance[k] = 0;

    vector<pair<int, int>> graph[v + 1]; // {to, distance}
    int a, b, w;
    for (int i = 0; i < e; i++) {
        scanf("%d %d %d", &a, &b, &w);
        graph[a].push_back({b, w});
    }

    priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq; // {distance, to}
    pq.push({0, k});
    while(!pq.empty()) {
        int now = pq.top().second;
        pq.pop();
        for (auto next : graph[now]) {
            int next_node = next.first;
            int next_dist = next.second;
            if (distance[next_node] > distance[now] + next_dist) {
                distance[next_node] = distance[now] + next_dist;
                pq.push({distance[next_node], next_node});
            }
        }
    }

    for (int i = 1; i <= v; i++) {
        if (distance[i] < INT_MAX)
            printf("%d\n", distance[i]);
        else
            printf("INF\n");
    }

    return 0;
}
```
