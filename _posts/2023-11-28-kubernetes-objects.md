---
title: 쿠버네티스 오브젝트와 labels 사용
date: 2023-11-28
categories: [ DevOps ]
tags: [ kubernetes, k8s, devops ]
---

> 참조: 쿠버네티스 어나더 클래스(일프로)
{: .prompt-info }

## 쿠버네티스 오브젝트

![kubernetes-objects](/assets/img/posts/kubernetes-objects.png)

- HPA(Horizontal Pod Autosacler): 리소스 사용량에 따라 파드의 개수를 자동으로 조정하기 위한 오브젝트
- Deployment: 파드의 배포를 관리하기 위한 오브젝트로 replicas 속성에 의해 ReplicaSet 오브젝트가 자동 생성되며 배포 방식을 정의할 수 있다.
  - 배포 방식
    - Rolling Update: 애플리케이션을 점진적으로 배포하는 방법으로 오브젝트 속성에서 직접 설정할 수 있다.
    - Canary Update: 새 버전을 특정 사용자에게 먼저 배포하는 방법으로 별도의 Deployment를 사용하거나 서비스 메쉬를 통해 구현한다. 
    - Blue-green Update: 새로운 버전의 검증을 목적으로 사용하며 이전 버전(Blue)과 새로운 버전(Green)을 공존한 상태로 두어 새로운 버전을 검증하는 방식으로 별도의 Deployment로 만들어 구현한다.
- ReplicaSet: 지정된 수의 Pod 복제본이 항상 실행되도록 하기 위한 오브젝트

> SatefulSet: 기본적인 기능은 ReplicaSet과 동잃하나 상태 유지를 필요로 하는 애플리케이션에 특화되어 고정된 네트워크와 저장소 환경을 구축하기 위해 사용된다.
{: .prompt-info }

![kubernetes-objects-probe](/assets/img/posts/kubernetes-objects-probe.png)

- Pod: 컨테이너 실행의 기본 단위로 컨테이너들의 집합이다. ReplicaSet에 의해 Pod의 복사본이 생성된다.
  - Probe: Pod의 상태를 점검하기 위한 설정으로 HTTP API 성공 여부, TCP 소켓 연결 여부, 컨테이너 내 특정 명령어 성공 여부를 통해 성패를 결정 지을 수 있다. 
    - startupProbe: Pod 내 서비스가 정상적으로 초기화되었는지 확인하는 설정이며 startupProbe가 성공해야만 readinessProbe와 livenessProbe 점검이 가능하다.
    - readinessProbe: 시스템 초기화뿐만 아니라 개발자가 의도한 설정이 정상적으로 초기화되었는지 확인하는 설정
    - livenessProbe: 시스템이 정상 동작중인지 확인하는 설정

> startupProbe와 readinessProbe는 성공할때까지 Service 리소스의 바인딩을 하지 않기 때문에 트래픽을 받을 수 없다.
{: .prompt-info }

- ConfigMap: 설정 정보와 환경변수를 저장하기 위한 오브젝트
- Namespace: 리소스를 분리하기 위한 클러스터 수준 오브젝트
- PV(Persistent Volume): 클러스터 수준의 스토리지 볼륨 오브젝트
- PVC(Persistent Volume Claim): PV를 사용하기 위한 요청 오브젝트
- Service: Pod의 네트워크 접근을 제공하는 오브젝트로 로드밸런싱에 사용된다.
- Secret: 민감 정보를 저장하기 위한 리소스로 클러스터 수준에서 암호화된다.

<br>

## labels, selector, naming

![kubernetes-objects-naming-example](/assets/img/posts/kubernetes-objects-naming-example.png)
![kubernetes-objects-labels-and-selector](/assets/img/posts/kubernetes-objects-labels-and-selector.png)
