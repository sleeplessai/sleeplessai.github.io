---
layout: post
title: "Second Post"
date:  2018-12-26 00:48:48 +0800
categories: [dev]
comments: true
excerpt: "测试代码高亮！做一次调色艺术家！"
---

这是第二篇Post！

在这一片篇post里面我们要测试代码！

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <cstring>

using namespace std;

const int maxn = 20;
const int inf = 999999999;

struct Edge {
    int from, to, dist;
    Edge(int u, int v, int d): from(u), to(v), dist(d) {}
};

struct Dijkstra {
    int n, m;
    vector<Edge> edges;
    vector<int> G[maxn];
    bool done[maxn];
    int d[maxn];
    int p[maxn];

    void GetDistances() {
        for (int i = 1; i <= n; ++i) {
            if (d[i] == inf) cout << '*' << ' ';
            else cout << d[i] << ' ';
        }
        cout << endl;
    }

    void init(int n) {
        this->n = n;
        for(int i = 0; i <= maxn; i++) G[i].clear();
        edges.clear();
    }

    void AddEdge(int from, int to, int dist) {
        edges.push_back(Edge(from, to, dist));
        m = edges.size();
        G[from].push_back(m - 1);
    }

    struct HeapNode{
        int d, u;
        bool operator < (const HeapNode& rhs) const {
            return d > rhs.d;
        }
    };

    void dijkstra(int s) {
        priority_queue<HeapNode> Q;
        for (int i = 0; i <= n; i++) d[i] = inf;
        d[s] = 0;
        memset(done, 0, sizeof(done));
        Q.push(HeapNode{0, s});
        while(!Q.empty()) {
            GetDistances();
            HeapNode x = Q.top(); Q.pop();
            int u = x.u;
            if (done[u]) continue;
            done[u] = true;
            cout << "pq:" << u << endl;
            for (int i = 0; i < G[u].size(); i++) {
                Edge &e = edges[G[u][i]];
                if (d[e.to] > d[u] + e.dist) {
                    d[e.to] = d[u] + e.dist;
                    p[e.to] = G[u][i];
                    Q.push((HeapNode){d[e.to], e.to});
                }
            }
        }
        GetDistances();
    }
};

int main() {
    Dijkstra solver;

    int n, m;
    cin >> n >> m;
    solver.init(n);
    for (int e = 0; e < m; ++e) {
        int u, v, w;
        cin >> u >> v >> w;
        solver.AddEdge(u, v, w);
        solver.AddEdge(v, u, w);
    }
    solver.dijkstra(1);
    solver.GetDistances();

    return 0;
}

/*
8 13
1 2 1
2 3 2
3 4 1
1 5 4
2 6 6
3 7 2
4 8 4
5 6 5
7 6 1
7 8 1
1 6 8
2 7 6
4 7 1

*/
```


那么接下来是我唯二会的语言！Python！

```python
import numpy as np 
import os
import skimage.io as io
import skimage.transform as trans
import numpy as np
from keras.models import *
from keras.layers import *
from keras.optimizers import *
from keras.callbacks import ModelCheckpoint, LearningRateScheduler
from keras import backend as keras


def unet(pretrained_weights = None,input_size = (256,256,1)):
    inputs = Input(input_size)
    conv1 = Conv2D(64, 3, activation='relu', padding='same', kernel_initializer='he_normal')(inputs)
    conv1 = Conv2D(64, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv1)
    pool1 = MaxPooling2D(pool_size=(2, 2))(conv1)
    conv2 = Conv2D(128, 3, activation='relu', padding='same', kernel_initializer='he_normal')(pool1)
    conv2 = Conv2D(128, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv2)
    pool2 = MaxPooling2D(pool_size=(2, 2))(conv2)
    conv3 = Conv2D(256, 3, activation='relu', padding='same', kernel_initializer='he_normal')(pool2)
    conv3 = Conv2D(256, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv3)
    pool3 = MaxPooling2D(pool_size=(2, 2))(conv3)
    conv4 = Conv2D(512, 3, activation='relu', padding='same', kernel_initializer='he_normal')(pool3)
    conv4 = Conv2D(512, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv4)
    drop4 = Dropout(0.5)(conv4)
    pool4 = MaxPooling2D(pool_size=(2, 2))(drop4)

    conv5 = Conv2D(1024, 3, activation='relu', padding='same', kernel_initializer='he_normal')(pool4)
    conv5 = Conv2D(1024, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv5)
    drop5 = Dropout(0.5)(conv5)

    # up6 = Conv2D(512, 2, activation='relu', padding='same', kernel_initializer='he_normal')(UpSampling2D(size = (2,2))(drop5))
    # merge6 = merge([drop4,up6], mode='concat', concat_axis = 3)
    # conv6 = Conv2D(512, 3, activation='relu', padding='same', kernel_initializer='he_normal')(merge6)
    # conv6 = Conv2D(512, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv6)

    # up7 = Conv2D(256, 2, activation='relu', padding='same', kernel_initializer='he_normal')(UpSampling2D(size = (2,2))(conv6))
    # merge7 = merge([conv3,up7], mode='concat', concat_axis = 3)
    # conv7 = Conv2D(256, 3, activation='relu', padding='same', kernel_initializer='he_normal')(merge7)
    # conv7 = Conv2D(256, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv7)

    # up8 = Conv2D(128, 2, activation='relu', padding='same', kernel_initializer='he_normal')(UpSampling2D(size = (2,2))(conv7))
    # merge8 = merge([conv2,up8], mode='concat', concat_axis = 3)
    # conv8 = Conv2D(128, 3, activation='relu', padding='same', kernel_initializer='he_normal')(merge8)
    # conv8 = Conv2D(128, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv8)

    up9 = Conv2D(64, 2, activation='relu', padding='same', kernel_initializer='he_normal')(UpSampling2D(size = (2,2))(conv8))
    merge9 = merge([conv1,up9], mode='concat', concat_axis = 3)
    conv9 = Conv2D(64, 3, activation='relu', padding='same', kernel_initializer='he_normal')(merge9)
    conv9 = Conv2D(64, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv9)
    conv9 = Conv2D(2, 3, activation='relu', padding='same', kernel_initializer='he_normal')(conv9)
    conv10 = Conv2D(1, 1, activation='sigmoid')(conv9)

    model = Model(input = inputs, output = conv10)
    model.compile(optimizer = Adam(lr = 1e-4), loss='binary_crossentropy', metrics = ['accuracy'])
    #model.summary()
    if (pretrained_weights):
    	model.load_weights(pretrained_weights)

    return model
```

然后我查看了一下高亮色彩，发现！正则表达式（Regex）的颜色有点暗淡！

```javascript
var regex0 = /(\w+):\/\/([^/:]+)(:\d*)?([^# ]*)/
```
