---
title: 쿠버네티스 오브젝트와 labels 사용
date: 2023-11-28
categories: [ DevOps ]
tags: [ kubernetes, k8s, devops ]
---

> 참조: 쿠버네티스 어나더 클래스(일프로)
{: .prompt-info }

### 쿠버네티스 오브젝트

![kubernetes-objects](/assets/img/posts/kubernetes-objects.png)

- HPA(Horizontal Pod Autosacler): 리소스 사용량에 따라 파드의 개수를 자동으로 조정하기 위한 오브젝트
- Deployment: 파드의 배포를 관리하기 위한 오브젝트로 replicas 속성에 의해 ReplicaSet 오브젝트가 자동 생성되며 배포 방식을 정의할 수 있다.
  - 배포 방식
    - Rolling Update
    - Canary Update
    - Blue-green Update
- ReplicaSet: 지정된 수의 Pod 복제본이 항상 실행되도록 하기 위한 오브젝트

> SatefulSet: 기본적인 기능은 ReplicaSet과 동잃하나 상태 유지를 필요로 하는 애플리케이션에 특화되어 고정된 네트워크와 저장소 환경을 구축하기 위해 사용된다.
{: .prompt-info }

- Pod: 컨테이너 실행의 기본 단위로 컨테이너들의 집합이다. ReplicaSet에 의해 Pod의 복사본이 생성된다.
- ConfigMap: 설정 정보와 환경변수를 저장하기 위한 오브젝트
- Namespace: 리소스를 분리하기 위한 클러스터 수준 오브젝트
- PV(Persistent Volume): 클러스터 수준의 스토리지 볼륨 오브젝트
- PVC(Persistent Volume Claim): PV를 사용하기 위한 요청 오브젝트
- Service: Pod의 네트워크 접근을 제공하는 오브젝트로 로드밸런싱에 사용된다.
- Secret: 민감 정보를 저장하기 위한 리소스로 클러스터 수준에서 암호화된다.

### labels, selector, naming

![kubernetes-objects-naming-example](/assets/img/posts/kubernetes-objects-naming-example.png)
![kubernetes-objects-labels-and-selector](/assets/img/posts/kubernetes-objects-labels-and-selector.png)
