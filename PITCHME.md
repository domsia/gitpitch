
---
<center>dask分布式建模平台</center>
===

<div style="text-align:right">
<a  href="10.22.64.81:8888">10.22.64.81:8888</a>
</div>


---
#### 目标

-  提高建模的效率
-  处理大数据量
-  尽量与当前建模代码提供一致的接口

---
#### 容器集群技术 docker swarm

相对于k8s，mesos等方案，docker swarm在我们的场景中有以下优势：

- docker Swarm和Docker环境很好的结合在一起
- Swarm 比较轻量，并且提供了很多的驱动，能够和其他的集群解决方案一起工作
- 易于使用

缺点： 不够成熟

---
#### docker swarm mode架构

<div align="center">
<img width="100%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhcwc18ezmj20j308vmyk.jpg"/>
</div>

 >  `data: nfs`  `network: overlay` `log: fluentd`

---

##### 集群结构：[http://10.22.64.81:8089/](http://10.22.64.81:8089/)
 
<div align="center">

<image  width="50%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhiiri9sqtj20k30qxwgw.jpg"/></div>

---
#### 建模平台结构图


<div align="center">
<img width="100%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhihv676naj20hr090dg8.jpg"/>
</div>

---
#####  dask searchcv demo
```python
from dask.distributed import Client
client = Client('10.0.0.3:8786')

from sklearn.datasets import load_digits
from sklearn.svm import SVC

# Fit with dask-searchcv
from dask_searchcv import GridSearchCV

## Fit with scikit-learn
#from sklearn.model_selection import GridSearchCV

param_space = {'C': [1e-4, 1, 1e4],
               'gamma': [1e-3, 1, 1e3],
               'class_weight': [None, 'balanced']}

model = SVC(kernel='rbf')
digits = load_digits()

search = GridSearchCV(model, param_space, cv=3)
search.fit(digits.data, digits.target)
search.visualize("search_graph", "png")
```
---
#####  [`searchcv execute graph`](https://10.22.64.85:8888/files/mydask/search_graph.svg)

<div align="center">
<img width="60%" src="http://ww1.sinaimg.cn/large/b433eefdgy1fhijhn8it3j21b717v456.jpg"/>
</div>

---
#### 一些bentchmark




---

