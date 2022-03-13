---
title: ThreadPool
date: 2022-03-13T09:23:58.311Z
summary: "***Create a txt file and insert some sentences, and apply the thread
  pool to output the contents of the file to the terminal.***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
***Create a txt file and insert some sentences, and apply the thread pool to output the contents of the file to the terminal.***

## 1. "ThreadPool.h"

```c
#ifndef _THREADPOOL_H
#define _THREADPOOL_H

typedef struct{
    int head, tail, size, total;
    void **data;
    pthread_mutex_t mutex;
    pthread_cond_t cond1, cond2;
}task_queue;

void task_queue_init(task_queue* Queue, int size);
void task_queue_push(task_queue* Queue, void* data);
void *task_queue_pop(task_queue* Queue);

void *thread_run(void *arg);

#endif
```

## 2. "ThreadPool.c"

```c
#include <stdio.h>
#include "head.h"

void task_queue_init(task_queue* Queue, int size){
    Queue->size = size;
    Queue->total = Queue->head = Queue->tail = 0;
    Queue->data = calloc(size, sizeof(void*));
    pthread_mutex_init(&Queue->mutex, NULL);
    pthread_cond_init(&Queue->cond1, NULL);
    pthread_cond_init(&Queue->cond2, NULL);
    return ;
}

void task_queue_push(task_queue* Queue, void* data){
    pthread_mutex_lock(&Queue->mutex);
    while (Queue->total == Queue->size) {
        DBG(YELLOW"<push> Queue is full.\n"NONE);
        pthread_cond_wait(&Queue->cond2, &Queue->mutex);
    }
    Queue->total++;
    Queue->data[Queue->tail] = data;
    DBG(PINK"<push> Queue push successfully.\n"NONE);
    if (++Queue->tail == Queue->size) {
        Queue->tail = 0;
        DBG(PINK"<push> Queue change tail to zero.\n"NONE);
    }
    pthread_cond_signal(&Queue->cond1);
    pthread_mutex_unlock(&Queue->mutex);
    return ;
}

void *task_queue_pop(task_queue* Queue) {
    pthread_mutex_lock(&Queue->mutex);
    while (Queue->total == 0) {
        DBG(YELLOW"<pop> Queue is empty.\n"NONE);
        //pthread_mutex_unlock(&Queue->mutex);
        //return ;
        pthread_cond_wait(&Queue->cond1, &Queue->mutex);
    }
    //Queue->data[Queue->head] = 0;
    void* data = Queue->data[Queue->head];
    Queue->total--;
    DBG(BLUE"<pop> Queue pop successfully.\n"NONE);
    if (++Queue->head == Queue->size) {
        Queue->head = 0;
        DBG(BLUE"<pop> Queue change head to zero.\n"NONE);
    }
    pthread_cond_signal(&Queue->cond2);
    pthread_mutex_unlock(&Queue->mutex);
    //sleep(10);
    return data;
}

void *thread_run(void *arg) {
    task_queue* Queue = (task_queue*) arg;
    pthread_detach(pthread_self());
    while (1) {
        void* data = task_queue_pop(Queue);
        DBG(GREEN"Got a task.\n"NONE);
        printf("%s", data);
    }
}
```

## 3. "head.h"

```c
#ifndef _HEAD_H
#define _HEAD_H
#include <unistd.h>
#include <string.h>
#include <stdlib.h>
#include <pthread.h>
#include "color.h"
#include "common.h"
#include "ThreadPool.h"
#ifdef _D
#define DBG(fmt, args...) printf(fmt, ##args)
#else
#define DBG(fmt, args...)
#endif
#endif
```

## 4. test demo

```c
#include <stdio.h>
#include "./common/head.h"
#define INS 5
#define MAX 100
int main() {
    FILE* fp;
    pthread_t tix[INS];
    char buff[MAX][1024];
    task_queue* Queue = (task_queue*)malloc(sizeof(task_queue));
    task_queue_init(Queue, MAX);
    for (int i = 0; i < INS; i++) {
        pthread_create(&tix[i],NULL, thread_run, (void*)Queue);
    }
    int cnt = 1;
    while(cnt--) {
        int sub = 0;
        if ((fp = fopen("./a.txt", "r")) == NULL) {
            perror("fopen");
            exit(1);
        }
        int ind = 0;
        while(fgets(buff[sub], 1024, fp) != NULL) {

            task_queue_push(Queue, buff[sub]);
            sleep(1);

            if (++sub == MAX) sub = 0;
        }
        fclose(fp);
        printf("==========\n");
    }
    return 0;
}
```