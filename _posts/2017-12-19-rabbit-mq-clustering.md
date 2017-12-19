---
layout: post
title: "2017/12/19 RabbitMQ clustering & mirroring 설정"
---

# RabbitMQ clustering
RabbitMQ 는 분산된 노드들을 묶어서 클러스터링을 구성할 수 있다. 이번 포스트에서는 RabbitMQ 클러스터링을 구성하는 방법에 대해서 알아본다.

# Clustering 설정
## 얼랭 쿠키값 맞추기
RabbitMQ 에서는 각 노드들이 서로 통신을 하기위해서 erlang cookie 를 이용한다. 각 노드들은 erlang cookie를 공유해야한다. 
클러스터로 묶을 노드틀은 모두 같은 erlang cookie 값을 같은 값으로 맞춰줘야 한다. /var/lib/rabbitmq/.erlang.cookie 파일에 저장되어 있으며, 이 쿠키값을 복사해서 다른 노드에도 동일하게 저장한다.

## Clustering 구성하기
Clustering 구성을 위해서 서버를 띄워야 한다. 이 포스트에서는 편의상 한서버에 두개의 RabbitMQ 인스턴스를 띄우도록 한다. 
```
RABBITMQ_NODE_PORT=5672 RABBITMQ_NODENAME=node1 rabbitmq-server -detached
RABBITMQ_NODE_PORT=5673 RABBITMQ_NODENAME=node2 rabbitmq-server -detached
```

Clustering 구성을 하기위해서는 둘중 하나의 노드를 중지 시키고 cluster 에 join 해야한다.
```
rabbitmqctl -n node2 stop_app
rabbitmqctl -n node2 join_cluster node1@`hostname -s`
rabbitmqctl -n node2 start_app
```

Clustering 구성이 되었는지 다음 명령어로 확인한다.
```
rabbitmqctl cluster_status
```

## Mirroring 설정 하기
RabbitMQ 는 high availability를 위해서 mirroring 모드를 지원한다. Mirroring 모드는 다음과 같이 활성화 할수 있다.

```
rabbitmqctl set_policy ha-all "^node" '{"ha-mode":"all"}'
```

# References
* Clustering Guide - https://www.rabbitmq.com/clustering.html
* Highly Available (Mirrored) Queues - https://www.rabbitmq.com/ha.html
