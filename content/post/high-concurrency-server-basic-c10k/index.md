---
title: High-concurrency server (Basic - C10k)
date: 2022-02-19T05:16:03.029Z
summary: |-
  ***Epoll_server***

  ***Client***
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Epoll_server

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netinet/ip.h>
#include <string.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <ctype.h>
#include <pthread.h>
#include <stdlib.h>
#include <sys/epoll.h>
#define SERV_PORT 8000
#define MAX_LENGTH 5000
#define MAX_USRS 2000
int epfd;
char data[MAX_USRS][MAX_LENGTH];
pthread_mutex_t mutex[MAX_USRS];
typedef struct TaskQueue{
	int head, tail, size, total, *data;
	pthread_mutex_t tq_lock;
	pthread_cond_t tq_cond1, tq_cond2;
}taskqueue;

taskqueue* TaskQueue_init(taskqueue* tq, int n) {
	tq->head = 0, tq->tail = 0, tq->size = n, tq->total = 0;
	tq->data = (int *)malloc(sizeof(int) * n);
	pthread_mutex_init(&tq->tq_lock, NULL);
	pthread_cond_init(&tq->tq_cond1, NULL);
	pthread_cond_init(&tq->tq_cond2, NULL);
	return tq;
}

void TaskQueue_push(taskqueue* tq, int connfd) {
	pthread_mutex_lock(&tq->tq_lock);
	while (tq->total == tq->size) {
		//printf("Task Queue is full\n");
		pthread_cond_wait(&tq->tq_cond1, &tq->tq_lock);
	}
	tq->data[tq->tail] = connfd;
	tq->tail = (tq->tail + 1) % tq->size;
	tq->total++;
	pthread_cond_broadcast(&tq->tq_cond2);
	pthread_mutex_unlock(&tq->tq_lock);
	return ;
}

int TaskQueue_pop(taskqueue* tq) {
	pthread_mutex_lock(&tq->tq_lock);
	while (tq->total == 0) {
		//printf("Task Queue is empty\n");
		pthread_cond_wait(&tq->tq_cond2, &tq->tq_lock);
	}
	int connfd = tq->data[tq->head];
	tq->head = (tq->head + 1) % tq->size;
	tq->total--;
	pthread_cond_broadcast(&tq->tq_cond1);
	pthread_mutex_unlock(&tq->tq_lock);
	return connfd;
}
 

void* up_server(void* arg) {
	pthread_detach(pthread_self());
	char buf[MAX_LENGTH];
	taskqueue* tq = (taskqueue*)arg;
	while (1) {
		int connfd = TaskQueue_pop(tq);
		//printf("pop successful, have %d\n", tq->total);
		pthread_mutex_lock(&mutex[connfd]);
		int n = read(connfd, buf, MAX_LENGTH), ind = strlen(data[connfd]);
		//write(1, buf, n);
		if (!n) {
			epoll_ctl(epfd, EPOLL_CTL_DEL, connfd, NULL);
			close(connfd);
		}
		for (int i = 0 ; i < n; i++) {
			if (buf[i] >= 'a' && buf[i] <= 'z') data[connfd][ind++] = buf[i] - 32;
			else if (buf[i] >= 'A' && buf[i] <= 'Z') data[connfd][ind++] = buf[i] + 32;
			else {
				data[connfd][ind++] = buf[i];
				if (buf[i] == '\n') {
					write(connfd, data[connfd], ind);
					bzero(data[connfd], ind);
				}
				
			}
		}
		pthread_mutex_unlock(&mutex[connfd]);
		//char respbuf[100] = {"HTTP/1.1 200 OK\r\nContent-Type:text/html\r\n\r\n<html><body><H3>Hello Http</H3></body></html>"};
		//write(connfd, buf, n);
		//close(connfd);
	}
	return (void *) 0;
}

int main() {
	int listenfd, connfd;
	struct sockaddr_in serveraddr, clientaddr;
	socklen_t clientlen;
	char info[INET_ADDRSTRLEN];
	taskqueue* tq = (taskqueue *)malloc(sizeof(taskqueue));
	TaskQueue_init(tq, 1000);
	pthread_t tid[5];
	for (int i = 0; i < 4; i++) {
		pthread_create(&tid[i], NULL, up_server, (void*)tq);
		//printf("new thread id = %#lx\n", tid[i]);
	}

	listenfd = socket(AF_INET, SOCK_STREAM, 0);

	bzero(&serveraddr, sizeof(serveraddr));
	serveraddr.sin_family = AF_INET;
	serveraddr.sin_port = htons(SERV_PORT);
	serveraddr.sin_addr.s_addr = htonl(INADDR_ANY);
	
	bind(listenfd, (struct sockaddr*)&serveraddr, sizeof(serveraddr));

	listen(listenfd, 150);
	for (int i = 0; i < MAX_USRS; i++) pthread_mutex_init(&mutex[i], NULL);

	epfd = epoll_create(256);
	struct epoll_event ev, event[256];
	ev.events = EPOLLIN | EPOLLET;
	ev.data.fd = listenfd;
	epoll_ctl(epfd, EPOLL_CTL_ADD, listenfd, &ev);


	printf("Accepting connection ... \n");

	while (1) {
		int nfds = epoll_wait(epfd, event, 256, -1);
		// printf("nfds value = %d\n", nfds);
		// printf("first one fd = %d\n", event[0].data.fd);
		for (int i = 0; i < nfds; i++) {
			if (event[i].data.fd == listenfd) {
				clientlen = sizeof(clientaddr);
				connfd = accept(listenfd, (struct sockaddr*) &clientaddr, &clientlen);
				//TaskQueue_push(tq, connfd);
				ev.events = EPOLLIN | EPOLLET;
				ev.data.fd = connfd;
				epoll_ctl(epfd, EPOLL_CTL_ADD, connfd, &ev);
				// printf("receive from %s : %d\n", \
				// inet_ntop(AF_INET, &clientaddr.sin_addr, info, sizeof(info)), ntohs(clientaddr.sin_port));
			} else if (event[i].events & EPOLLIN) {
				int task_id = event[i].data.fd;
				TaskQueue_push(tq, task_id);
				//printf("Get Task, have %d\n", tq->total);
			}
		}
		// pid_t pid = fork();
		// if (pid > 0) {
		// 	close(connfd);
		// 	while (waitpid(-1, NULL, WNOHANG) > 0){}
		// }

		// close(listenfd);

		// int n = read(connfd, buf, MAX_LENGTH);
		// for (int i = 0 ; i < n; i++) buf[i] = toupper(buf[i]);
		// write(connfd, buf, n);
		// close(connfd);
	}
	close(listenfd);
	free(tq);
	return 0;
}
```

## 2. client

```c
#include <stdio.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <netinet/ip.h>
#include <string.h>
#include <arpa/inet.h>
#include <unistd.h>
#include <ctype.h>
#define SERV_PORT 8000
#define MAX_LENGTH 100

int main() {
	int sockfd = socket(AF_INET, SOCK_STREAM, 0);
	struct sockaddr_in serveraddr;
	char buf[MAX_LENGTH] = "";
	bzero(&serveraddr, sizeof(serveraddr));
	serveraddr.sin_family = AF_INET;
	serveraddr.sin_port = htons(SERV_PORT);
	inet_pton(AF_INET, "101.201.75.177", &serveraddr.sin_addr);
	connect(sockfd, (struct sockaddr*) &serveraddr, sizeof(serveraddr));
	int n = read(0, buf, MAX_LENGTH);
	//while(cnt--) sleep(1);
	write(sockfd, buf, n);
	//write(sockfd, buf, n - 1);
	printf("response from server : \n");
	n = read(sockfd, buf, MAX_LENGTH);
	write(1, buf, n);
	close(sockfd);
	return 0;
}
```