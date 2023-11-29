---
title: 쿠버네티스 구성요소
date: 2023-11-29
categories: [ DevOps ]
tags: [ kubernetes, k8s, devops ]
---
> 참조: 쿠버네티스 어나더 클래스(일프로)

![kubernetes-components-overall](/assets/img/posts/kubernetes-components-overall.png)

<br>

## 컨트롤 플레인 구성요소

> 마스터 노드에서 클러스터의 전반적인 관리와 제어 역할을 하는 구성요소

> 쿠버네티스가 기본적으로 제공하는 컨트롤 플레인 구성요소 외에 클러스터 수준에서 추가적인 기능을 제공하기 위해 별도의 Pod를 추가할 수 있는데 이를 Add-on Pod라고 부른다.
{: .prompt-info }

<br>

### kube-apiserver

> 클러스터와의 모든 상호작용을 위한 API 엔드포인트를 제공하는 구성요소

<br>

### etcd

> 클러스터의 모든 상태 및 데이터를 저장하는 분산 키-값 저장소 **(NoSQL)**

<br>

### kube-scheduler

> 새로 생성된 파드를 적절한 노드에 할당하기 위한 스케줄링 역할의 구성요소

<br>

### kube-controller-manager

> 클러스터 상태를 원하는 상태로 유지하기 위해 etcd를 관찰하고 컨트롤러를 실행시키는 구성요소

<br>

### cloud-controller-manager

> 기본적으로 kube-controller-manager와 동일한 기능을 하나 클라우드 서비스 제공자에 특화된 컨트롤러를 실행시키는 구성요소

<br>

## 워커 노드 구성요소

> 실제 애플리케이션의 Pod가 실행되는 노드의 구성요소

<br>

### kubelet

> 각 노드에서 실행되어 컨테이너 런타임을 제어하여 파드의 생명주기를 관리하는 구성요소

<br>

### kube-proxy

> 각 노드의 네트워크 프록시 역할을 담당하며 네트워크 규칙을 관리하고 제어하는 구성요소
