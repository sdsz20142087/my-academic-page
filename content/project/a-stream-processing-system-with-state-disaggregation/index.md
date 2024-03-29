---
title: A Stream Processing System with State Disaggregation
subtitle: Feb. 2023 – May 2023
date: 2023-05-14T23:07:51.139Z
summary: >-
  We've created a cutting-edge prototype for a disaggregated data stream
  processing system using a standalone control plane, allowing task-state
  separation. The control plane serves as a routing table for state access,
  enabling seamless job reconfiguration without causing any downtime or
  disruption.


  ***T﻿ech Stack: Java, gRPC, RocksDB, Etcd, Docker Compose, Prometheus, Grafana***
draft: false
featured: true
tags:
  - Stream-Processing
external_link: ""
links:
  - url: https://github.com/sdsz20142087/A-Stream-Processing-System-with-State-Disaggregation
    name: Github
    icon_pack: fab
    icon: github
  - url: https://drive.google.com/file/d/1fG5Yl8mC4CsBSiW3WLtGBl965jnyThJr/view
    name: Video
    icon_pack: fab
    icon: youtube
image:
  filename: featured.jpg
  focal_point: Smart
  preview_only: false
---
### A﻿rchitecture

![](stream_processing_system.png)

### S﻿tate Disaggregation

![](state_disaggregation.png)

### D﻿escription

* Built a standalone control plane, separating tasks and states, optimizing the state migration mechanism in Flink.
* Applied Java and gRPC to create a distributed event-driven framework, where TaskManager manages operators.
* Utilized watermarks as the logical ingestion time to handle late-arriving events in window operators.
* Implemented consistent hashing with virtual nodes to minimize state migration cost during operator scaling.
* Used RocksDB to store state of TaskManager, employed etcd for storing routing table, ensuring fault-tolerance.
* Construct a scalable deployment on AWS EC2 using Docker Compose, auto-scaling, and load balancing.
* Evaluated latency during state migration with Prometheus and Grafana, finding no downtime, just a 30% rise.
