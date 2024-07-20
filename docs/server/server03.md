---
title: Airflow
layout: default
parent: Server
nav_order: 3

---

# Airflow

workflow 관리하는 platform - opensource

### Workflow의 정의

![](../../../assets/server/images/airflow1.png)

DAG ( directed acyclic graph )로 정의된다. 하나의 작업단위는 Task이며 DAG는 TASK 간의 flow를 graph로 나타낸다.

Task는 python operator 등으로 operator로 작성된다.    

### Archtecture

![](../../../assets/server/images/airflow2.png)

- scheduler 는 workflow를 trigger하고 task를 executor가 실행하게 한다.
- webserver 는 user interface
- database: medata 정보들을 보관. workflow 상태 등 보관

### 특징

- git syn로 dag 파일를 관리 할 수 있다.
