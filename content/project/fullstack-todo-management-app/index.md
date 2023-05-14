---
title: A Stream Processing System with State Disaggregation
subtitle: Feb. 2023 – May 2023
date: 2023-01-23T22:15:01.831Z
summary: "T﻿ech Stack: Java, gRPC, RocksDB, Etcd, Docker Compose, Prometheus, Grafana"
draft: false
featured: false
tags:
  - Full-stack
external_link: https://www.yingzhe-dong.com/project/fullstack-todo-management-app/
links:
  - icon: github
    name: Star
    icon_pack: fab
    url: https://github.com/sdsz20142087/FullStack-Todo-Management-App
image:
  filename: todom.jpeg
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