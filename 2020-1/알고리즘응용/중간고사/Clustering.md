# Clustering

클러스터링(군집화)은 개체들이 주어졌을 때, 개체들을 몇 개의 클러스터로 나누는 과정을 의미한다. 이 과정을 통해 동일한 집단내에서는 서로 가깝거나 비슷하게, 타 집단 개체 사이에는 서로 멀거나 비슷하지 않게 하는 것이 클러스터링의 목표이다. 

<img src="https://www.secmem.org/assets/images/clustering-jihoon/clustering_example.png" alt="img" style="zoom:48%;" />

비지도 학습은 정답을 알려주지 않고 비슷한 데이터를 군집화 하여 미래를 예측하는 학습 방법이다. 기계가 라벨링이 되어있지 않은 데이터로부터 특성을 찾아야 한다. 일반적으로 데이터는 라벨링이 되어있지 않고, 종류또한 무엇인지 알 수 없는 경우가 많기 때문에 지도 학습에 비해 더 많이 활용된다.

클러스터링 알고리즘은 대표적인 비지도 학습 알고리즘이며, 주로 성향이 불분명한 시장을 분석하는 경우, 고객 분석, 패턴인식에 사용되며 최근에는 패턴인식, 음성인식의 기본 알고리즘으로도 활용되고 있다.

클러스터링에 사용되는 알고리즘은 크게 분할군집화(Partitional Clustering)와 계층군집화(Hierarchical clustering)로 나눌 수 있는데, 본 글에서는 분할군집화의 **k-means**와 **DBSCAN (Density-based method)**을 알아보고 계층 군집화(**Hierarchical Clustering**)에 대해서도 알아본다.



## k-means

1. 사용자가 클러스터의 수(k)를 정한다.
2. 이 point가 각 클러스터의 centroid마다 얼마나 가까운지. 모든 점을 다 돌아서 k 개의 클러스터를 만든다.
3. 각 클러스터의 평균을 구하여 새로운 centroid로 설정한다.
4. centroid가 바뀌지 않을 때까지 2~3 과정을 반복한다.

k와 centroid가 굉장히 중요하며 이상치(outlier)에 취약하다.



## Hierarchical Clustering

- 응집성(Agglomerative) 계층 클러스터링 : 하나의 데이터들이 뭉치며 하나의 군집을 형성하도록 반복적으로 병합한다.
- 분열성(Divisive) 계층 클러스터링 : 하나의 클러스터로 시작하여, 각각의 클러스터가 하나의 데이터만을 가질 때까지 반복적으로 분기한다.



## DBSCAN (Density-based method)

k-means 등을 활용한 클러스터링의 경우 이상치에 굉장히 민감한 영향을 받는데, 이에 대한 해법 중 하나이다.

Eps : cluster의 반지름.

MinPts : min points. core point가 되기 위한 Eps 안에 있는 최소한의 point의 수.

- **핵심점(core point)** : 속하는 점

- **경계점(border point)** : 경계에  있는 점

- **잡음점(noise point)** : core도, border도 아닌 점

1. 모든 점들을 핵심점, 경계점, 잡음점으로 표시한다.
2. 잡음점들을 제거한다
3. 서로간에 EPS의 거리 내에 있는 모든 핵심점들 사이에 간선을 만든다.
4. 각각의 연결된 핵심점들의 그룹을 독립적인 군집으로 만든다.
5. 각각의 경계점들을 관련된 핵심적의 군집들 중 하나에 속하게 한다.



### 활용

**scikit-learn**을 사용하여 거리를 구할 수 있다.

```python
from sklearn.cluster import KMeans, AgglomerativeClustering, DBSCAN

model = KMeans(n_clusters=2, init="random", n_init=1, max_iter=n, random_state=6).fit(X)
agg = AgglomerativeClustering(n_clusters=3)
db = DBSCAN(eps=0.2, min_samples=10).fit(test_data)
```
