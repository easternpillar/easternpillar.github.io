---
title: Linux OS, Container, Container Orchestration 기술 변천
date: 2023-11-28
categories: [ DevOps ]
tags: [ devops, k8s, kubernetes, container, docker, linux ]
---

> 참조: 쿠버네티스 어나더 클래스(일프로)
{: .prompt-warning }
 
# 개요
![linux-container-history-overall](/assets/img/posts/linux-container-history-overall.png)

## 컨테이너 런타임
![linux-container-history-container-runtime](/assets/img/posts/linux-container-history-container-runtime.png)
> 기존의 도커는 root 권한으로 설치하는 방식으로 인해 host와의 격리가 불안전한 이슈가 있었으나 rootless 설치 방식을 지원하게 되었다.
{: .prompt-info }
