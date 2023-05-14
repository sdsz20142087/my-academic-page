---
title: A Stream Processing System with State Disaggregation
subtitle: Feb. 2023 – May 2023
date: 2023-05-14T23:07:51.139Z
summary: "T﻿ech Stack: Java, gRPC, RocksDB, Etcd, Docker Compose, Prometheus, Grafana"
draft: false
featured: true
external_link: https://www.yingzhe-dong.com/project/a-stream-processing-system-with-state-disaggregation/
links:
  - url: https://github.com/sdsz20142087/A-Stream-Processing-System-with-State-Disaggregation
    name: Star
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


* Built a standalone control plane, separating tasks and states, optimizing the state migration mechanism in Flink.
* Applied Java and gRPC to create a distributed event-driven framework, where TaskManager manages operators.
* Utilized watermarks as the logical ingestion time to handle late-arriving events in window operators.
* Implemented consistent hashing with virtual nodes to minimize state migration cost during operator scaling.
* Used RocksDB to store state of TaskManager, employed etcd for storing routing table, ensuring fault-tolerance.
* Construct a scalable deployment on AWS EC2 using Docker Compose, auto-scaling, and load balancing.
* Evaluated latency during state migration with Prometheus and Grafana, finding no downtime, just a 30% rise.
