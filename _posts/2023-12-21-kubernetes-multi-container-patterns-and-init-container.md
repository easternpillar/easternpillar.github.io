---
title: 쿠버네티스 멀티 컨테이너 패턴과 초기화 컨테이너
date: 2023-12-21
categories: [ DevOps ]
tags: [ devops, k8s, kubernetes, container, design, pattern ]
---

<br>

## 멀티 컨테이너 디자인 패턴

![kubernetes-multi-container-patterns](/assets/img/posts/kubernetes-multi-container-patterns.png)

### 사이드카(Sidecar) 패턴

> 주 컨테이너를 보조하는 컨테이너를 같은 파드에 배치하고 동일한 파일시스템을 공유하는 패턴

<br>

### 어댑터(Adapter) 패턴

> 주 컨테이너의 출력물 규격을 보조 컨테이너를 통해 필요에 맞게 다듬기 위한 패턴

<br>

### 앰버서더(Ambassador) 패턴

> 네트워크 프록시 형태의 보조 컨테이너가 외부 서비스와의 통신을 중계하여 주 컨테이너로의 접근을 간소화하는 패턴


## 초기화 컨테이너

> 파드의 주 컨테이너 실행 전 초기화 작업을 위한 컨테이너

- 여러 초기화 컨테이너가 존재하는 경우, 순차적으로 실행된다.

> 의도된 작업을 수행 후 __반드시__ 종료되므로 probe 설정이 불가능하다.
{: .prompt-warning }

